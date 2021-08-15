# Week8 - Fezz

## UIApplication 객체의 컨트롤러 역할은 어디에 구현해야 하는가?



### UIApplication

iOS 앱을 구동하기 위한 조정과 중앙 집중식 제어를 담당하는 곳이다.

모든 iOS앱에는 UIApplication인스턴스가 "하나만" 있습니다. 앱이 시작되면, 시스템은 `UIApplicationMain(_:_:_:_:)` 함수를 호출하고 다른 task들 중에서 싱글톤 `UIApplication`객체를 만듭니다. 그런 다음, shared클래스 메소드를 호출하여 객체에 접근합니다.

```swift
open class UIApplication : UIResponder {
    
	open class var shared: UIApplication { get }
	unowned(unsafe) open var delegate: UIApplicationDelegate?
    
    ...
}
```

application 객체의 주요 역할은, 들어오는 사용자 이벤트의 초기 라우팅을 처리하는 것입니다. 컨트롤 객체(UIControl의 인스턴스)가 적절한 target 객체에 전달한 action 메세지를 전달합니다. application 객체는 열린 window(UIWindow의 객체)의 목록을 유지관리하며, 이를 통해 앱의 UIView객체를 검색할 수 있습니다.

<br>

#### UIApplicationMain

`UIApplicationMain(_:_:_:_:)`은 application객체와 application delegate를 만들고, 이벤트 사이클을 설정하는 역할을 가지고 있습니다.

```swift
func UIApplicationMain(_ argc: Int32, 
                     _ argv: UnsafeMutablePointer<UnsafeMutablePointer<CChar>?>, 
                     _ principalClassName: String?, 
                     _ delegateClassName: String?) -> Int32
```

- **argc** 

  argv의 개수. 대게 main에 해당하는 파라미터입니다.

- **argv** 

  argument의 변수 목록. 대게 main에 해당하는 파라미터입니다.

- **prinsipalClassName** 

  UIApplication클래스 또는 하위 클래스의 이름입니다. nil을 지정하면, UIApplication으로 가정됩니다.

- **delegateClassName**

   application delegate가 인스턴스화 되는 클래스 이름입니다. prinsipalClassName이 UIApplication의 하위클래스를 지정하는 경우, 하위 클래스를 delegate로 지정 할 수 있습니다. 하위클래스 인스턴스는 앱의 delegate 메세지를 받습니다. 앱의 기본 nib파일에서 delegate 객체를 로드하는 경우, nil을 지정합니다.

<br>

### AppDelegate

```swift
@main
class AppDelegate: UIResponder, UIApplicationDelegate { ... }
```

