# UIWindow객체의 역할은 무엇인가?

---

> 사용자 앱 인터페이스의 배경이면서 View로 이벤트를 전달 해 주는 개체 입니다.



UIWindow는 View Controller와 협업 하면서 이벤트를 처리하고 app 동작에 있어서 핵심적인 작업을 실행합니다. UIKit이 전반적인 window와 관련된 상호작용들을 처리합니다.



프로그래머가 window를 설정을 주는 경우는 아래와 같습니다:

- app content의 메인화면을 제공할 때
- 추가 app content를 보여주기 위한 추가 window를 생성할 때

기본적으로, Xcode가 사용자 앱의 main window를 제공합니다. iOS project는 storyboard를 통해 app의 view를 구성하고 Storyboard는 app delegate의 window 프로퍼티를 필요로 합니다.(xcode가 이 부분은 자동으로 제공합니다.) 만약 앱을 구현할 때 스토리보드를 사용하지 않는다면 프로그래머는 window를 직접 생성해야 합니다.



대부분의 앱은 하나의 window를 필요로합니다. 해당 윈도우에는 기기의 메인화면이 보여집니다. 메인화면에 추가적으로 window를 생성할 수 있고 이 추가된 window는 보통 외부모니터에 연결된 화면에 컨텐츠를 보여줄 때 사용됩니다.



Window는 직접적으로 눈에 보이지 않습니다. **대신, window는 하나 또는 그 이상의 view를 갖고 있고 이 view는 window의 root view controller에 의해 관리됩니다.** 프로그래머는 스토리보드를 활용하여 이 rootviewcontorlller의 환경 설정을 해 줄 수 있습니다.(인터페이스에게 적합한 view를 더하는 것 과같은 작업)

UIWindow를 상속받는 경우는 매우 드뭅니다. 왜냐하면 보통 window에게 할당하려는 작업은 window보다 더 상위 계층에 있는 view controller가 처리할 수 있기 때문입니다. 다만 window의 key 상태가 바뀌는 경우 UIWindow를 상속 받아서 `becomekey()` 또는 `resignKey()` 메서드를 정의 해 줄 수 있습니다.



## Understanding Keyboard Interactions

좌표값을 가지고 있는 사용자의 터치 이벤트가 window로 전달되는 것에 반해 좌표값을 가지고 있지 않는 이벤트같은 경우 `key window`에게 전달 됩니다. window는 `isKeyWindow` boolean 프로퍼티를 갖고 있고 해당 값이 true인 경우 window가 keywindow가 되어서 키보드작업 또는 터치와 관련되지 않는 이벤트를 받고 처리합니다. 한 번에 하나의 window만 key window가 될 수 있고 거의 대부분의 경우 app의 main window가 key window가 됩니다만 UIKit이 필요에 따라 다른 window에게 key window역할을 부여할 수 있습니다.

만약 특정 window가 key window인지 확인하고 싶은 경우 `didBecomeKeyNotification` `didResignKeyNotification` 과 같은 notification을 통해 확인이 가능합니다. 시스템이 key window의 상태 변화에 따라 상태변화 알림을 보내줄 수 있습니다. 특정 window를 key window로 설정하거나 key window 설정을 해제하기를 원한다면 `becomeKey()` 또는 `resignKey()` 메서드를 활용 하세요.



## appDelegate의 window property

> 스토리보드를 보여줄 때 사용되는 window

```swift
optional var window: UIWindow? { get set }
```

appDelegate의 window 프로퍼티는 기기의 메인화면에 보여질 앱의 시각적 컨텐츠를 위해 사용될 window를 갖고 있는 프로퍼티입니다.

사용자 앱의 `Info.plist` 파일에 `UIMainStoryboardFile key`가 있다면 해당 프로퍼티는 필수로 구성되어있어야 합니다.

Xcode 프로젝트는 app delegate가 필요로하는 window property가 이미 선언 되어있기 때문에 별도로 설정 해 줄 필요가 없지만 만약에 커스텀 윈도우를 구현 해 주기 위해서는 해당 프로퍼티를 getter 메서드를 활용하여 구현해야하고 해당 프로퍼티를 활용하여 커스텀 window를 생성하고 반환 해주어야 합니다.



## 종합

종합 해 보자면 UIWindow는 사용자 인터페이스와 객체에 배경을 제공합니다. UIWindow는 화면에 시각적으로 보이지는 않지만 화면에 나타내는 모든 view를 갖고, View Controller와의 협업을 통해서 발생하는 이벤트를 처리합니다. iOS 13이후에는 scene이라는 개념이 생기면서 window는 각 



[UIWindow | Apple Developer Document](https://developer.apple.com/documentation/uikit/uiwindow)

[isKeyWindow | Apple Developer Document](https://developer.apple.com/documentation/uikit/uiwindow/1621612-iskeywindow)