---
layout: post
title: "escaping 클로저"
image:
categories: iOS
tags: 
  - closure
  - escaping closure
sitemap:
  changefreq: daily
  priority: 1.0
---

## Escaping Closure를 사용하는 방법

```swift
func getImages(completionHandler: @escaping (Result<UIImage, Error>) -> Void)
```

클로저의 타입 앞에 `@escaping` 키워드를 붙여주면 해당 클로저는 escaping closure로 사용할 수 있다.



## Escaping vs Non-Escaping 

클로저를 함수 내부에서 사용할 때, 함수가 종료되었더라도 클로저가 사용될 수 있도록 해주는 키워드가 `@escaping`이다.

함수가 종료되었는데 전달인자로 받은 클로저를 실행해야되는 경우로는 아래와 같은 예시들이 있다.

- 비동기 작업으로 인해 클로저가 나중에 실행되는 경우
- return 값으로 다시 그 클로저를 반환하는 경우
- 외부 변수에 클로저를 저장해야하는 경우



`@escaping` 키워드를 사용했지만 클로저를 밖에서 쓰지 않았다고 warning이나 error가 발생하지는 않는다. 다만 성능 개선을 위해서 가능한 경우에는 life cycle 명확한 non-escaping closure를 사용하는 것이 바람직하다.



### 비동기 작업으로 인해 클로저가 나중에 실행되는 경우 예시

```swift
func postItem(data: PostingItem, completionHandler: @escaping (Result<MarketItem, OpenMarketError>) -> Void) {
        sessionManager.request(method: .post, path: .item(), data: data) { result in
            switch result {
            case .failure(let error):
                completionHandler(.failure(error))
            case .success(let data):
                guard let decodedData = try? JSONDecoder().decode(MarketItem.self, from: data) else {
                    return completionHandler(.failure(.invalidData))
                }
                completionHandler(.success(decodedData))
            }
        }
    }
```

sessionManage.request는 비동기적으로 실행될 메서드이므로 completionHandler는 나중에 실행되게 된다. 이런 경우에 `@escaping` 키워드를 붙여주어야 한다.



### return 값으로 클로저를 반환하는 경우 예시

```swift
func fetchImageDataTask(urlString: String?, completionHandler: @escaping (Data) -> Void) -> URLSessionDataTask? {
        guard let urlString = urlString,
              let url = URL(string: urlString) else { return nil }

        return dataTask(request: URLRequest(url: url)) { result in
            guard let data = try? result.get() else { return }
            return completionHandler(data)
        }
    }
```



## 주의할 점

Escaping closure은 함수의 전달인자로 전달하게 되는데 이 말은 클로저의 구현부는 다른 곳에 존재한다는 것이다.

클로저를 구현할 때 내부에서 사용할 reference는 항상 `weak`으로 참조하여 사용해야한다. 이는 retain cycle을 통한 memory leak을 막기 위함이다. 보통 self를 사용하는 경우가 많으므로 클로저의 앞 부분에 `[weak self]`를 붙여줌으로써 메모리 누수를 막는다.



## 참고한 링크

- [https://www.youtube.com/watch?v=xiS5gJOIQxI&ab_channel=SeanAllen](https://www.youtube.com/watch?v=xiS5gJOIQxI&ab_channel=SeanAllen)