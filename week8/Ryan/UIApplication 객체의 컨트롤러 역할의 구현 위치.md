# UIApplication 객체의 컨트롤러 역할은 어디에 구현해야 하는가?
`UIApplication` 객체는 아래 `UIApplication` 문서에서 다루고 있듯 모든 앱에 유일하게 하나만 존재하는 `UIApplication`의 `shared` 싱글턴 인스턴스를 의미합니다. 

`UIApplication` 클래스의 타입 프로퍼티로 정의된 `shared` 인스턴스는 앱을 구성하는 모든 위치에서 접근이 가능하지만 `shared` 인스턴스를 컨트롤하기 위한 목적으로 호출하는 경우는 매우 드물 것입니다. 하지만 이 유일한 `UIApplication` 객체를 부담 없이 가지고 작업할 수 있는 공간이 있는데요. 바로 `AppDelegate` 타입입니다.

![](https://images.velog.io/images/ryan-son/post/e968e35f-008b-4d40-af77-bce1b222391f/image.png)

`AppDelegate` 객체는 앱의 실행 시점에서 `UIApplicationMain(_:_:_:_:)` 메서드를 통해 `UIApplication` 객체와 함께 생성되는 객체로 생성되는 시점에 아래 그림에 나타난 `UIApplication.shared`의 `delegate`로 설정됩니다.

![](https://images.velog.io/images/ryan-son/post/0ae8ccce-2cf4-4784-b3bd-34c6bb812c34/image.png)

![](https://images.velog.io/images/ryan-son/post/261de85b-392b-4af7-b0d4-bf71722a9e9d/image.png)

`AppDelegate`에서 `UIApplication.shared`과 같은 방식으로 인스턴스에 접근하지 않더라도 우리는 여러가지 `UIApplicationDelegate` 메서드를 통해 이에 접근해 다양한 작업을 할 수 있었는데요, 대표적인 메서드가 바로 `func application(_:didFinishLaunchingWithOptions:) -> Bool`과 같은 것입니다. 저희는 이 메서드의 첫 번째 매개변수의 전달인자로 전달된 `application`을 가지고 `UIApplication.shared`에 접근할 수 있으며, 이를 이용해 local notifications를 예약, 취소하거나 remote notification을 등록하고, remote-control에 대한 반응을 처리하고, App 상태를 복구하는 등의 작업을 수행할 수 있습니다.

따라서 이와 같은 근거로 미루어보아 `UIApplication` 객체의 컨트롤러 역할은 `AppDelegate`가 수행한다고 보는 것이 적절하다고 판단합니다.

## UIApplication
> iOS에서 실행되는 앱의 컨트롤 및 조정의 중심점

`UIApplication`의 인스턴스는 모든 iOS 앱이 정확히 하나만 가지고 있습니다 (매우 드문 경우로 `UIApplication`의 서브클래스 인스턴스를 가지는 경우도 있습니다). 앱이 시작될 때 시스템이 `UIApplicationMain(_:_:_:_:)` 함수를 호출하여 `shared`라 부르는 `UIApplication` 싱글턴 인스턴스를 생성합니다.

Application 객체는 들어오는 사용자 이벤트에 대한 초기적인 라우팅을 수행합니다. 자세히는 `UIControl` 클래스의 인스턴스인 control 객체가 전달한 액션 메시지를 적절한 대상 객체에 전달합니다. application 객체는 열린 windows 리스트 (`UIWindow` 객체들)를 가지고 application의 `UIView` 객체를 가져오는데 사용합니다.

`UIApplication` 클래스는 `UIApplicationDelegate` 프로토콜을 준수하는 딜리게이트를 정의하고 있는데, 이 프로토콜을 준수하기 위해서는 몇 가지의 메서드들을 필수적으로 구현하여야 합니다. Application 객체는 딜리게이트에게 앱 실행, 메모리 부족 경고 및 앱 종료와 같이 중요한 런타임 이벤트를 알려 상황에 맞게 적절하게 대응할 수 있도록 합니다.

앱들은 `open(_:options:completionHandler:)` 메서드를 이용하여 이메일이나 이미지 파일과 같은 리소스를 처리할 수 있습니다. 예를 들어, 이메일 URL을 사용하여 이 메서드를 호출하는 앱은 메일 앱을 시작하고 메시지를 표시합니다.

이 클래스의 API를 사용하면 아래와 같은 분야의 장치별 동작을 관리할 수 있습니다.

- 일시적으로 들어오는 터치 이벤트 중지 (`beginIgnoringInteractionEvents()`)
- Remote notifications 등록 (`registerForRemoteNotifications()`)
- Undo-rodo UI 트리거 (`applicationSupportsShakeToEdit`)
- URL scheme을 처리할 설치된 앱이 등록되어 있는지 판단 (`canOpenURL(_:)`)
- 백그라운드에서 작업을 완료할 수 있도록 앱 실행 연장 (`beginBackgroundTask(expirationHandler:)` 및 `beginBackgroundTask(withName:expirationHandler:)`)
- Local notifications 예약 및 취소 (`scheduleLocalNotification(_:)` 및 `cancelLocalNotification(_:)`)
- Remote-control 이벤트 접수 및 조정 (`beginReceivingRemoteControlEvents()` 및 `endReceivingRemoteControlEvents()`)
- 앱 수준의 복구 작업 수행 (동 문서 Managing the State Restoration Behavior 작업 그룹에 속한 메서드)

### 서브클래싱 (상속) 시 참고할 점
대부분의 앱들은 `UIApplication`을 서브클래싱할 필요가 없습니다. 대신, 시스템과 앱 간에 일어나는 상호작용들은 **app delegate**를 사용하세요.

매우 드문 상황이기는 하지만, 만약 앱이 시스템이 처리하기 전에 들어오는 이벤트를 처리해야 한다면 사용자 지정 이벤트 또는 동작을 분배하는 메커니즘을 구현할 수 있습니다. 이를 위해서는 `UIApplication`을 서브클래싱하고 `sendEvent(_:)` 및/또는 `sendAction(_:to:from:for:)` 메서드를 오버라이드 (재정의)하세요. 가로채는 모든 이벤트에 대해 이벤트를 처리한 후 아래 메서드를 호출하여 시스템이 본래 분배하는 방식으로 되돌려 놓으시면 됩니다.
```swift
super.sendEvent(event)
```
이벤트를 가로채는 작업은 매우 드물게 필요한 상황이니 가능하시면 피하는 것이 좋습니다.

## UIApplicationDelegate
> 앱의 공유 동작을 다루기 위한 메서드들의 집합

앱 딜리게이트 객체는 앱의 공유 동작을 관리합니다. 앱 딜리게이트는 앱의 근간을 이루는 객체이며 `UIApplication`과 함께 작동하여 시스템에 들어오는 일부 상호 작용을 관리합니다. `UIApplication` 객체와 마찬가지로 `UIKit`은 앱 딜리게이트 객체가 항상 존재할 수 있도록 앱 실행 사이클의 초기에 앱 딜리게이트 객체를 생성합니다.

아래 작업들을 처리하는데 앱 딜리게이게이트 객체를 사용할 수 있습니다.
- 앱의 중앙 데이터 구조 초기화
- 앱 scene들의 구성
- 메모리 부족 경고, 다운로드 완료 알림 등과 같이 앱의 외부로 부터 발생한 알림에 반응
- 앱의 scenes, vies, view controllers와 관련 없이 순수하게 앱 자체를 목표로하는 이벤트에 대한 반응

앱 시작 시점에 앱 딜리게이트 객체를 이용해 앱을 초기화하는 방법에 대해 더 알아보고 싶으시다면 [앱 실행에 반응하기](https://developer.apple.com/documentation/uikit/app_and_environment/responding_to_the_launch_of_your_app)를 읽어보세요.

# 참고자료
- [UIApplication](https://developer.apple.com/documentation/uikit/uiapplication/) - Apple Developer
- [UIApplicationDelegate](https://developer.apple.com/documentation/uikit/uiapplicationdelegate) - Apple Developer
- [About the App Launch Sequence](https://developer.apple.com/documentation/uikit/app_and_environment/responding_to_the_launch_of_your_app/about_the_app_launch_sequence) - Apple Developer

