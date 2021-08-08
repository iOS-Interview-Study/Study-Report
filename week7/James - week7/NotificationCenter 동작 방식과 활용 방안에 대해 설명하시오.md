# NotificationCenter 동작 방식과 활용 방안에 대해 설명하시오.

---



## The Observer Pattern

Observer 패턴을 활용하면 하나의 객체는 상태 변화사항을 감지하고 다른 객체에게 알려줍니다. 이 과정에서 객체들은 서로를 몰라도 됩니다 그리고 이 부분은 코드간의 결합성을 낮춰주는 장점이 있습니다.

메커니즘은 이와 같습니다: observer 객체가 어떠한 객체의 변화에 대한 관심을 등록하게 되면 등록된 객체의 프로퍼티에 변화가 일어나게 되면 이 변화사항을 관심있어하는 객체들은 알림을 받을 수 있게 됩니다.



MVC 패턴을 기반으로 이런 코드를 짜기 위해서는 Model과 View간의 소통은 가능해야 하지만 다이랙트한 소통은 지양되어야 합니다.

![Album cover scroller ](https://koenig-media.raywenderlich.com/uploads/2017/07/ScrollerNoImages-180x320.png)

Pop Music을 보여주는 앱이 있을 때 상단의 수평 스크롤뷰의 사진을 선택할 때마다 사용자는 해당 음악의 상세 내용을 확인할 수 있습니다. 여기서 각 음악의 커버사진을 구현할 때 **Notification**을 사용 해 볼 수 있습니다.

그러기 위해서는 이미지를 다운로드 받아야 하는데요. 이 과정에서 유의해야 할 점은 아래와 같습니다.

1.  `AlbumView` 는 `API` 모델과 직접적으로 소통하면 안됩니다. View와 모델의 소통은 지양해야 하기 때문이죠. 또한 View의 로직과 소통의 로직이 섞이면 바람직한 모델이 될 수 없습니다.
2. 위와 같은 이유로 `API` 또한 `AlbumView` 를 알면 안됩니다.
3. `API` 는 이미지가 다운이 완료가 되면 `AlbumView`에게 알려줘야 합니다.`AlbumView` 가 사용자에게 음악의 커버사진을 알려줘야 하기 때문이죠

## Notifications && NotificationCenter

Notification은 구독-출판 model에 기반을둡니다.

출판(publish)하는 객체가 구독하는 객체에게 메세지를 보낼 수 있게 됩니다.

출판 객체는 구독자 객체에 대해서 몰라도 됩니다.



#### AlbumView: 출판하는 객체

``` swift
class AlbumView: UIView {
  
  private var coverImageView: UIImageView!
  private var indicatorView: UIActivityIndicatorView!
  
  required init?(coder aDecoder: NSCoder) {
    super.init(coder: aDecoder)
    commonInit()
  }
  
  init(frame: CGRect, coverUrl: String) {
    super.init(frame: frame)
    commonInit()
    NotificationCenter.default.post(name: Notification.Name.BLDownloadImage, object: self, userInfo: ["imageView" : coverImageView, "coverULR" : coverUrl])
  }
}
```

``` swift
init(frame: CGRect, coverUrl: String) {
  super.init(frame: frame)
  
  NotificationCenter.default.post(name: .BLDownloadImage, object: self, userInfo: ["imageView": coverImageView, "coverUrl" : coverUrl])
}
```

위 코드는 `NotificationCenter` 싱글톤에게 notification을 보내는 코드 입니다. 그리고 이 notification은 `UIImageView` 와 해당 이미지를 다운로드 받아야 하는 URL을 갖고 있습니다. 해당 코드를 `AlbumView` 가 초기화 되는 시점에 포함하여 `AlbumView` 의 객체가 생성될 때 notification을 보낼 수 있개 설정합니다.



#### LibraryAPI: 구독하는 객체

```swift
final class LibraryAPI {
  private init() {
    NotificationCenter.default.addObserver(self, selector: #selector(downloadImage(with:)), name: .BLDownloadImage, object: nil)
  }
  
  @objc func downloadImage(with notification: Notification) {
    guard let userInfo = notification.userInfo,
      let imageView = userInfo["imageView"] as? UIImageView,
      let coverUrl = userInfo["coverUrl"] as? String,
      let filename = URL(string: coverUrl)?.lastPathComponent else {
        return
    }
    
    DispatchQueue.global().async {
      let downloadedImage = self.httpClient.downloadImage(coverUrl) ?? UIImage()
      DispatchQueue.main.async {
        imageView.image = downloadedImage
      }
    }
  }
}
```

``` swift
private init() {
    NotificationCenter.default.addObserver(self, selector: #selector(downloadImage(with:)), name: .BLDownloadImage, object: nil)
  }
```

`AlbumView` 가 매 번 `BLDownloadImage` notification을 post하게될 때 `LibraryAPI` 가 `BLDownloadImage` notification을 구독하고있게됩니다. 그리고 위 코드에 따라서 `LibraryAPI` 는 `downloadImage(with:)` 메서드를 호출하여 응답하게 됩니다.



[참고]:

[Design Patterns on iOS using Swift - Part 2/ 2](https://www.raywenderlich.com/476-design-patterns-on-ios-using-swift-part-2-2)
