# [week1] - Fezz

## Delegates와 Notification 방식의 차이점에 대해 설명하시오.



## Delegation

Delegate는 보통 `Protocol`을 정의하여 사용한다. 

Protocol이란 **일종의 기능 명세서** 같은 것으로 Delegate로 지정된 객체가 해야 하는 메소드들의 원형을 적어 놓는다. Delegate 역할을 하려는 객체는 이 Protocol을 따르며 원형만 있던 메소드들의 구현을 한다. 이렇게 세팅 후 이전 객체는 어떤 이벤트가 일어났을 시 delegate로 지정한 객체에 알려줄 수 있다.

```swift
// 1) Delegate 프로토콜 선언
protocol SomeDelegate {
    func someFunction(someProperty: Int)
}

class SomeView: UIView {
    // 2) 순환 참조를 막기 위해 weak으로 delegate 프로퍼티를 가지고 있음
    weak var delegate: SomeDelegate?

    func someTapped(num: Int) {
        // 3) 이벤트가 일어날 시 delegate가 동작하게끔 함
        delegate?.someFunction(someProperty: num)
    }
}
// 4) Delegate 프로토콜을 따르도록 함
class SomeController: SomeDelegate {
    var view: SomeView?

    init() {
        view = SomeView()
        // 6) delegate를 자신으로 설정
        view?.delegate = self
        someFunction(someProperty: 0)
    }

    // 5) Delegate 프로토콜에 적힌 메소드 구현
    func someFunction(someProperty: Int) {
        print(someProperty)
    }
}

let someController = SomeController()
// prints 0
```

<br/>

### 장점

- 매우 엄격한 Syntax로 인해 프로토콜에 필요한 메소드들이 명확하게 명시됨.
- 컴파일 시 경고나 에러가 떠서 프로토콜의 구현되지 않은 메소드를 알려줌.
- 로직의 흐름을 따라가기 쉬움.
- 프로토콜 메소드로 알려주는 것뿐만이 아니라 정보를 받을 수 있음.
- 커뮤니케이션 과정을 유지하고 모니터링하는 제 3의 객체(ex: NotificationCenter 같은 외부 객체)가 필요없음.
  프로토콜이 컨트롤러의 범위 안에서 정의됨.

### 단점

- 많은 줄의 코드가 필요.
- delegate 설정에 nil이 들어가지 않게 주의해야함. 크래시를 일으킬 수 있음.
- 순환 참조를 조심해야 함.
- 많은 객체들에게 이벤트를 알려주는 것이 어렵고 비효율적임.(가능은 하지만)

<br/>

## Notification

`Notification Center`라는 **싱글턴 객체**를 통해서 이벤트들의 발생 여부를 옵저버를 등록한 객체들에게 Notification을 post하는 방식으로 사용한다. Notification name이라는 key 값을 통해 보내고 받을 수 있다.

```swift
// 1) Notification을 보내는 ViewController
class PostViewController: UIViewController {
    @IBOutlet var sendNotificationButton: UIButton!

    @IBAction func sendNotificationTapped(_ sender: UIButton) {
        guard let backgroundColor = view.backgroundColor else { return }

        // Notification에 object와 dictionary 형태의 userInfo를 같이 실어서 보낸다.
        NotificationCenter.default.post(name: Notification.Name("notification"), object: sendNotificationButton, userInfo: ["backgroundColor": backgroundColor])
    }
}

// 2) Notification을 받는 ViewController
class ViewController: UIViewController {

    override func viewDidLoad() {
        super.viewDidLoad()

        // 옵저버를 추가해 구독이 가능하게끔 함
        NotificationCenter.default.addObserver(self, selector: #selector(notificationReceived(notification:)), name: Notification.Name("notification"), object: nil)
    }

    // iOS 9 이상이 아닐 경우에는 removeObserver를 해줘야 함
    deinit {
        NotificationCetner.default.removeObserver(self)
    }

    @objc func notificationReceived(notification: Notification) {
        // Notification에 담겨진 object와 userInfo를 얻어 처리 가능
        guard let notificationObject = notification.object as? UIButton else { return }
        print(notificationObject.titleLabel?.text ?? "Text is Empty")

        guard let notificationUserInfo = notification.userInfo as? [String: UIColor],
            let postViewBackgroundColor = notificationUserInfo["backgroundColor"] else { return }
        print(postViewBackgroundColor)
    }
}
```

<br/>

### 장점

- 많은 줄의 코드가 필요없이 쉽게 구현이 가능.
- 다수의 객체들에게 동시에 이벤트의 발생을 알려줄 수 있음.
- Notification과 관련된 정보를 `Any?` 타입의 `object`, `[AnyHashable: Any]?` 타입의 `userInfo`로 전달할 수 있음.

### 단점

- key 값으로 Notification의 이름과 userInfo를 서로 맞추기 때문에 컴파일 시 구독이 잘 되고 있는지, 올바르게 userInfo의 value를 받아오는지 체크가 불가능함.
- 추적이 쉽지 않을 수 있음.
- `post` 이후 정보를 받을 수 없음.

<br/>

## Key Value Observing(KVO)

KVO는 A 객체에서 B 객체의 프로퍼티가 변화됨을 감지할 수 있는 패턴이다. 위의 두 패턴이 주로 Controller와 다른 객체 사이의 관계를 다룬다면 KVO 패턴은 **객체와 객체 사이의 관계**를 다루는데 적합하다. 메소드나 다른 액션에서 나타나는 것이 아니라 프로퍼티의 상태에 반응하는 형태이다.

<br/>

### 장점

- 두 객체 사이의 정보를 맞춰주는 것이 쉬움.
- new/old value를 쉽게 얻을 수 있음.
- key path로 옵저빙하기 때문에 nested objects도 옵저빙이 가능함.

### 단점

- `NSObject`를 상속받는 객체에서만 사용이 가능함.
- `dealloc`될 때 옵저버를 지워줘야 함.
- 많은 value를 감지할 때는 많은 조건문이 필요.

덧붙여서 Swift 4 전까지는 NSObject의 메소드인 `observeValue(forKeyPath:change:context:)`를 오버라이드하여 옵저버를 추가했으나 Swift4부터는 구독하고 싶은 프로퍼티에 `observe()`를 추가하여 클로저로 사용할 수 있게 하였다. 

**그러나 Swift 상에서는 `didSet`이나 `willSet` 같은 것으로 충분히 대체가 가능하다.**

그럼에도 불구하고, **다른 개발자가 구성한 모듈의 프로퍼티에 감시자를 달 경우**에는 KVO가 더 적합하다.

<br/>

------

## 그렇다면 언제, 어디서 이것들을 나눠서 써야할까?

`When to use delegation, notification, or observation in iOS`의 글쓴이의 경우 KVO를 사용하는 경우(프로퍼티 단위의 변화 감지)는 명확하기 때문에 제외하고 Delegation과 Notification을 구분하였다. 그래서 그 글쓴이가 내린 결론은 명확히 프로토콜로 정의되어 있는 Delegation을 웬만하면 사용하자는 것이다. 조금의 노력을 들여서 Delegate로 연결을 하면 **코드가 읽기도 쉽고 추적이 쉬워진다**는 이유에서이다.

<br/>

---



참고 

[When to use delegation, notification, or observation in iOS](https://shinesolutions.com/2011/06/14/delegation-notification-and-observation/)

[Delegation, Notification, 그리고 KVO](https://wnstkdyu.github.io/2018/01/19/threepattern/)