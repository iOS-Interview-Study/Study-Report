# Delegation Pattern

---



> 위임하다
>
> - 어떤 일을 책임 지워 맡기다.
>
> 위임자
>
> - 다른사람에게 사무의 처리를 맡기는 사람
>
> 대리인
>
> - 다른 사람을 대신하는 사람
>
> [출처]: 표준 국어대사전

> Delegate:
>
> - the act of empowering to act for another
>
> [출처]: Meriam-Webster Dictionary



According to [Apple](https://developer.apple.com/library/archive/documentation/General/Conceptual/DevPedia-CocoaCore/Delegation.html):

> Delegation은 특정 개체가 다른 개체를 대신하여 일을 보게 해주는 심플하고 강력한 패턴입니다. delegating object 또는 위임자 개체는 대리자 개체에 참조를 두고 적절한 시간에 메세지를 보내게 됩니다. 위임자가 어떠한 작업을 맡게되었는지 또는 맡을 것인지를 대리자에게 메세지로 보내게 됩니다. 대리자는 자기자신의 상태를 업데이트 하거나 앱 안의 다른 개체를 업데이트하고 또 어떤 경우에는 임박한 이벤트를 처리하는데 영향을 주는 값을 위임자에게 반환하여 위임자의 메세지에 응답합니다. Delegation의 가장 큰 이점은 하나의 중심 개체를 통해 여러 개체들을 중심 개체가 추구하는 방향으로  커스텀화할 수 있다는 것입니다....위임과 동일한 효과를 얻기 위해서는 수신한 객체는 자기 자신을 delegate에게 전달합니다.





## Delegating?? Delegate??

> Delegate는 특정한 object가 이벤트를 맞닥뜨릴 때 해당 object를 대신하거나 협동해 주는 object입니다.
>
> Delegating object는 흔히 Appkit 또는 UIKit의 `NSResponder`를 상속 받아서 사용자의 이벤트에 응답 해 주는 object입니다.
>
> [출처]: https://developer.apple.com/library/archive/documentation/General/Conceptual/CocoaEncyclopedia/DelegatesandDataSources/DelegatesandDataSources.html



쉽게 말해서 위임자 == Delegating, 대리자 == Delegate 라고 보면 됩니다.

위 내용을 종합 해 보자면 

#### Delegating object(일을 맡기는 위임자)는

- 일반적으로 framework 개체 입니다.
- delegate 또는 대리자에 대한 참조를 유지합니다.
- 적절한 때 에 대리자에게 메세지를 보냅니다.
- 대부분의 Cocoa framework class의 위임자는 대리자에 의해 발생하는 알림의 **관찰자로** 자동 등록 됩니다.



#### Delegate(일을 맡는 대리자)는

- 사용자가 만든 컨트롤러 객체입니다.
- 프레임워크 클래스에서 선언한 프로토콜을 채택하여 대리자로서 역할하게 됩니다.
- 위임자로부터 메세지를 받기 위해서 채택한 프로토콜의 메서드를 구현하변 됩니다.
- 어플리케이션 안에 있는 자신의 상태 또는 모양 그리고 더 나아가서 다른 객체의 상태를 업데이트 해 위임자의 메세지에 응답합니다.



![Screen Shot 2021-06-26 at 9.28.48 PM](https://raw.githubusercontent.com/inwoodev/uploadedImages/uploadedFiles/20210626212902.png)

애플 개발자 문서에 있는 예제를 좀 이해해 보자면 

`NSWindow`는 Appkit framework class의 instance이고 위임자 개체 중 하나입니다.  

프로그래머가 만드는 Custom View Controller에서 `NSWindow` 일을 위임 해 주는 `NSWindowDelegate` 프로토콜을 채택합니다.

`NSWindowDelegate`를 선언하고 `NSWindowDelegate` 채택한 해당 custom View Controller는 `NSWindow`의 대리인이 됩니다.

유저가 좌측 상단에 window object 안에 있는 x버튼을 클릭하면 NSWindow 개체는 그의 대리인에게 `windowShouldClose:` 라는 메세지를 보내서 윈도우를 닫아도 되는지 컨펌을 받습니다. Custom View Controller는 은 창을 닫아도 되는지 확인을 한뒤 bool 값을 리턴해서 닫을지 말지 결정을 내리고 `NSWindow`는 return 값에 의해 제어됩니다.



## Delegate pattern을 구현하기 위한 요구사항: Protocol

#### Delegate Design pattern은 위임된 책임을 캡슐화하는 프로토콜을 정의하여 사용할 수 있습니다.



## View Controller는 수신자인데 대리자이기도 한건가?

View Controller가 Delegate가 되기 위해서는 아래와 같이 3가지가 필요합니다.

1. 수신자(View Controller)
2. Delegate(protocol)
3. 대리자에게 수신자 자신을 전달하는 것



따라서 아래와 같이 delegate를 채택하는 것 뿐 아니라 대라지에게 수신자 자신을 전달하는 행위가 필요합니다.

``` swift
// 수신자가 대리자 채택
class ReceiverViewController: TapDelegate {
  var button = DestroyButton()
	
  override func viewDidLoad() {
    // 대리자에게 수신자 자신을 전달
    button.delegate = self
  }
  
  func didTapButton() {
    
  }
}
```

``` swift
protocol TapDelegate {
  func didTapButton()
}
```

``` swift
class DestroyButton: UIButton {
  // 위임자 개체가 대리자 개체의 참조를 두고 있다.
  weak var delegate: TapDelegate?
  
  func shutDownScreen() {
    // 대리자 개체에게 메세지 전달
    delegate?.didTapButton()
  }
}
```





# Observer and Notification Pattern

---

## Notification

According to [apple](https://developer.apple.com/library/archive/documentation/Cocoa/Conceptual/Notifications/Articles/Notifications.html#//apple_ref/doc/uid/20000215-73929)...

> Notification은 어떠한 창이 열린다, 또는 네트워크 연결이 끊긴다는 것과 같은 특정한 이벤트의 정보를 캡슐화 합니다. 창이 닫히는 정보를 알아야 하는 개체들은 notification에 등록을 해서 창이 닫히는 이벤트가 실행시 해당 이벤트에 대해서 알림을 받을 수 있습니다. 해당 이벤트가 일어나면, notification이 notification center 에 post 되고 해당 알림은 등록된 개체에게 즉시 알려줍니다.



## Notification Center 

등록된 observer에게 동시에 notification을 전달하는 Class 입니다. Notification Center는 싱글턴 객체이고 이벤트 발생 여부를 observer로 등록한 객체에게 notification을 post 하는 방식으로 메세지를 전달합니다.

> NSNotification Center는 subject - observer 사이에 위치하여 indirection의 역할을 수행한다. 즉, subject와 observer 사이에서 변화에 대해 알려주면 observer에게 notify 하는 역할을 담당한다. subject가 직접 주던 notification을 담당하는 것이다.
>
> [출처]: https://daheenallwhite.github.io/design%20pattern/2019/07/02/Observer-Pattern/

개체와 개체간의 소통을 하기 위해서는 매세지를 주고 받아야 합니다. 그런데 메세지를 전달하기 위해서는 보내는 개체가 받는 개체를 알아야 하고 이는 높은 결합성으로 이어지고 이러한 높은 coupling은 바람직하지 않습니다. 이런 높은 결합도를 방지하고자 제시된 것이 바로 notification center 입니다.

Subject와 observer 사이에 이렇게 중간다리 역할이 들어가게 되어서 개체간의 통신이 더 유연한 구조로서 구현될 수 있습니다. Subject가 그저 observer를 등록한 뒤 어떠한 이벤트에 대해 NSNotification Cneter에게 알려주기만 하면 일이 끝나기 때문입니다. 이러한 구조 덕분에 subject는 observer의 행위에 대해 더이상 알지 않아도 되니 subject와 observer간의 결합도가 낮아질 수 있다는 장점도 있습니다.



## 동작 원리

![Screen Shot 2021-06-27 at 5.52.49 PM](https://raw.githubusercontent.com/inwoodev/uploadedImages/uploadedFiles/20210627175254.png)

[출처]: https://nsios.tistory.com/36

- 특정 객체에서 이벤트가 발생했다는 것을 Notification Center로 송신해서 등록하면
- Notification Center에서 이벤트발생을 등록된 모든 observer에게 보내줍니다.
- 해당 이벤트 내용을 구독하고 있는 observer를 찾아가 observer의 화면에서 해당 이벤트에 대한 처리를 할 수 있습니다.



## 주의

> subject가 notification을 post 하는 작업은 매우 오버헤드가 큰 작업입니다. post 된 notification은 전 시스템서버로 보내지고 notification center를 observe 하고 있는 개체들에게 전달이 됩니다. notification이 post 되고 observe 하고 있는 개체에게 전달되는 시간은 정해져있습니다. 만약 너무 많은 notification이 post 되어 있다면 서버의 queue에 더 이상 notification이 post되지 않을 것이고 해당 notification은 누락될 수 있습니다.
>
> [출처]: https://developer.apple.com/library/archive/documentation/Cocoa/Conceptual/Notifications/Articles/NotificationCenters.html#//apple_ref/doc/uid/20000216-BAJGDAFC

이런 경고문을 공식문서에서 읽을 수 있기 때문에 notification을 남발하는 것은 바람직하지 않습니다.



또한 iOS 9 이후로 추가된 observer 개체를 제거할 필요는 없지만 아래와 같은 경우 retain cycle에 의한 메모리 누수를 막기 위해서는 remove Observer를 꼭 해줘야 합니다.

``` swift
addObserver(forName:object:queue:using:)
```

[addObserver(forName:object:queue:using:) | Apple Develper Document](https://developer.apple.com/documentation/foundation/notificationcenter/1411723-addobserver)

## Delegate와 Notification 방식의 차이점은?

두 방식 모두 특정 이벤트가 일어나면 원하는 객체에게 알려줘서 받은 객체가 이벤트를 처리하는 방식입니다. 

첫 번째 차이점은 Delegate 방식으로 소통하면 대리자역할을 하는 수신자 개체만 메세지를 받을 수 있는것에 반해 notification 방식은 어떤 개체든 observer로 등록을 하게 되면 메세지를 받을 수 있습니다.

두 번째 차이점은 Delegate 방식은 protocol에 부합하는 메세지만 받을 수 있는 것에 반해 notification 방식은 어떠한 메세지라도 개체가 전달 받을 수 있습니다.

세 번째 차이점은 notification의 소통방식에서는 보내는 쪽 개체가 받는 쪽 개체가 존재하는지 몰라도 되지만 delegate 방식은 참조를 통해 대리자의 존재를 알 수 있다는 것입니다.



[참고자료]:

[Understanding Delegates and Delegation in Swift 4 - Andrew Jafffe](https://www.appcoda.com/swift-delegate/)

[Delegate pattern in iOS - delmasong](https://velog.io/@delmasong/Delegate-pattern-in-iOS-x1k6f9jzx8)

[Delegation | Apple Developer Documentation Archive](https://developer.apple.com/library/archive/documentation/General/Conceptual/DevPedia-CocoaCore/Delegation.html)

[[Swift] - Delegate Pattern에 대해서 - wannagohome](https://velog.io/@iwwuf7/Swift-Delegate-Pattern%EC%97%90-%EB%8C%80%ED%95%B4%EC%84%9C)

[iOS 앱 프로그래밍 - 1) 노티피케이션 센터와 노티피케이션](https://www.boostcourse.org/mo326/lecture/16919)

[[Design Pattern] Observer Pattern - 구독하고 알림받기](https://daheenallwhite.github.io/ios/2019/10/13/Notification-Center/)

[[iOS] NotificationCenter - 관찰해서 알려주는 역할](https://daheenallwhite.github.io/ios/2019/10/13/Notification-Center/)

[[iOS] Swift Event - Delegate, Notification, KVO란? (2/3) - NamS의 iOS일기](https://nsios.tistory.com/36)

[Swift Memory Part 3](https://velog.io/@architecture/Swift-Memory-Part.3)

