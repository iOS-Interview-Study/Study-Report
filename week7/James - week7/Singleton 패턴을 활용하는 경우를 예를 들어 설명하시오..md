# Singleton 패턴을 활용하는 경우를 예를 들어 설명하시오.

---

**싱글톤 패턴은 주어진 클래스의 하나의 인스턴스가 존재하고 해당 인스턴스 접근이 전역으로 가능하게 하는 패턴입니다.** 



```swift
final class LibraryAPI {
  // 1
  static let shared = LibraryAPI()
  // 2
  private init() {

  }
}
```

- `shared` 라는 static 상수를 통해서 해당 클래스의 싱글턴 객체를 전역에서 접근이 가능해집니다.

- private 이니셜라이저를 설정하여 해당 클래스가 외부에서 새로운 인스턴스로 만들어지는 것을 방지합니다.

전역으로 접근할 수 있는 싱글턴과 같은 공유 리소스의 인스턴스는 하나로 제한 하여 해당 리소스 접근 을 thread-safe하게 구현할 수 있습니다. 여러 인스턴스를 통해 동시다발적으로 싱글턴객체를 수정한다면 문제가 생길 수 있으니 이를 방지하는 것이죠. 해당 객체는 lazy하게 로딩됩니다. 즉 해당 객체가 필요 없을 때는 메모리에 올라와 있지 않고 호출시 메모리에 로딩 되는 것입니다.



## 사용예시

``` swift
import Foundation

final class PersistencyManager {
  private var albums = [Album]()
  init() {
    //Dummy list of albums
    let album1 = Album(title: "Best of Bowie",
                       artist: "David Bowie",
                       genre: "Pop",
                       coverUrl: "https://s3.amazonaws.com/CoverProject/album/album_david_bowie_best_of_bowie.png",
                       year: "1992")
    
    albms = [album1]
  }
  
  func getAlbums() -> [Album] {
    return albums
  }
}
```

```swift
final class LibraryAPI {
  static let shared = LibraryAPI()
  private let persistencyManager = PersistencyManager()
  
  private init() { }
  
  func getAlbums() -> [Album] {
    return persistencyManager.getAlbums()
  }
}
```

``` swift
import UIKit

final class ViewController: UIViewController {
  private var allAlbums = [Album]()
  
  override func viewDidLoad() {
    super.viewDidLoad()
    
    allAlbums = LibraryAPI.shared.getAlbums()
  }
}
```

#### 싱글턴의 가장 큰 문제점: Testing

싱글턴 객체를 테스트하려면 테스트의 순서 또한 굉장히 중요 해 집니다 왜냐하면 전역적으로 호출 되기 때문에 상태변화의 순서를 잘 알아야지 정상적인 테스트가 가능한기 때문입니다 그리고 이 점은 해당 객체를 mock하기 굉장히 어렵게 만듭니다. 프로젝트 단위가 커지면 커질 수록 테스트가 힘들어집니다.

[참고]:

[Design Patterns on iOS using Swift - Part 1/2](https://www.raywenderlich.com/477-design-patterns-on-ios-using-swift-part-1-2)