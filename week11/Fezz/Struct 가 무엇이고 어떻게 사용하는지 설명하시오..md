# Week 11 - Fezz

## Struct 가 무엇이고 어떻게 사용하는지 설명하시오.

 구조체와 클래스는 데이터를 용도에 맞게 묶어 표현하고자 할 때 유용하며, 프로퍼티와 메서드를 사용하여 구조화된 데이터와 기능을 가질 수 있다.



### 구조체와 클래스의 차이

#### 공통점

- 값을 저장하기 위해 프로퍼티를 정의할 수 있다.
- 기능 실행을 위해 메서드를 정의할 수 있다.
- 서브스크립트 문법을 통해 구조체나 또는 클래스가 갖는 값(프로퍼티)에 접근하도록 서브스크립트를 정의할 수 있다.
- 초기화될 때의 상태를 지정하기 위해 이니셜라이저를 정의할 수 있다.
- 초기구현과 더불어 새로운 기능 추가를 위해 익스텐션을 통해 확장할 수 있다.
- 특정 기능을 실행하기 위해 특정 프로토콜을 준수할 수 있다.

<br>

#### 차이점

- 구조체는 상속할 수 없다.
- 타입캐스팅은 클래스의 인스턴스에만 허용된다.
- 디이니셜라이저는 클래스의 인스턴스에만 활용할 수 있다.
- 참조 횟수 계산은 클래스의 인스턴스에만 적용된다.



### 구조체와 클래스 선택하기

애플은 가이드라인에서 다음 조건 중 하나 이상에 해당한다면 구조체를 사용하는 것을 권장한다.

- 연관된 간단한 값의 집합을 캡슐화하는 것만이 목적일 때
- 캡슐화한 값을 참조하는 것보다 복사하는 것이 합당할 때
- 구조체에 저장된 프로퍼티가 값 타입이며 참조하는 것보다 복사하는 것이 합당할 때
- 다른 타입으로부터 상속받거나 자신을 상속할 필요가 없을 때



### 같이 생각해보고 싶은 부분

네트워크 통신을 할 때 

- 한 번만 인스턴스 생성
- 내부에서 값만 전달

```swift
struct MarketNetwork: OpenMarketNetwork {
    private let session: URLSession = .shared
    
    func excuteNetwork(request: URLRequest, completion: @escaping (Result<Data, Error>) -> Void) {
        session.dataTask(with: request) { data, response, error in
            if let error = error { return }
            guard let response = response as? HTTPURLResponse else { return }
            guard (200...299) ~= response.statusCode else { return }
            guard let data = data else { return }
            
            completion(.success(data))
        }.resume()
    }
}
```

![구조체](https://raw.githubusercontent.com/Fezravien/UploadForMarkdown/forUpload/img/%E1%84%80%E1%85%AE%E1%84%8C%E1%85%A9%E1%84%8E%E1%85%A6.jpg)

