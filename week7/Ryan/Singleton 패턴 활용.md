# Singleton 패턴을 활용하는 경우를 예를 들어 설명하시오.
## Singleton Pattern
> 프로그램 전체에 걸쳐 하나의 인스턴스만 존재하는 것이 타당할 때 사용할 수 있는 프로그래밍 디자인 패턴 

싱글턴 패턴으로 구현된 인스턴스는 주로 타입 프로퍼티로 구현하므로 아래와 같은 장점이 있습니다. 
- 어디에서나 접근하기 편리합니다.
- 동일한 기능을 다른 인스턴스가 처리하지 않음을 보장하기에 다수의 인스턴스를 다룰 때보다 인스턴스 내부의 프로퍼티를 예측하기 용이합니다.

반면, 단 하나의 인스턴스라는 구조적 특징으로 인해 테스트하기 쉽지 않다는 단점이 있습니다.
코드로는 주로 아래와 같이 구현합니다.

```swift
// 앱 전체에 걸쳐 사용할 테마
sturct Theme {

    // 하나의 공유 인스턴스를 다루므로 주로 shared라는 네이밍을 사용
    static let shared = Theme()

    // 추가적인 인스턴스를 생성할 수 없도록 접근제한자로 이니셜라이저 사용을 제한
    private init() { }
    
    // shared 인스턴스에서 사용할 프로퍼티와 메서드 정의
    private var backgroundColor: UIColor = .systemBackground

    func setbackgroundColor(to color: UIColor) {
        backgroundColor = color
    }
}

// 활용
Theme.shared.setbackgroundColor(to: .systemGray4)
// Theme의 backgroundColor를 바라보고 있는 모든 ViewController들의 색상이 변경됨
```

iOS에서 활용되고 있는 singleton object들은 아래와 같은 것들이 있습니다.

```swift
let application = UIApplication.shared
let screen = UIScreen.main
let userDefaults = UserDefaults.standard
let fileManager = FileManager.default
let session = URLSession.shared
```

저도 Core Data를 활용할 때 앱 전반에 걸쳐 단 하나의 Persistent Container를 사용하기 위해 아래와 같이 활용했었습니다.

```swift
import CoreData

struct CoreDataStack: CoreDataStackProtocol {

    static let shared = CoreDataStack()
    private var persistentContainer: NSPersistentContainer = { ... }()

    private init() { }
}
```