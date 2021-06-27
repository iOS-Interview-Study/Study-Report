

# Delegates와 Notification 방식의 차이점에 대해 설명하시오.

# Delegates

- Protocol을 정의하여 사용
- 많은 객체들에게 이벤트를 알려주는 것이 어렵고 비효율적임

# Notification


- Notification Center라는 싱글턴 객체를 사용
- 다수의 객체들에게 동시에 이벤트 발생을 알려줄 수 있음
- 발행 이후 정보를 받을 수 없음
- 추적이 쉽지 않음

## 차이점
### 관계
Delegate는 위임되는 객체와 위임하는 객체간 1:1관계, NotificationCenter는 알림을 보내는 객체와 등록된 객체들간 1:n 관계를 가진다.

```swift
// Delegate
@protocol SomeDelegate
...
delegatedObject.delegate = self

// NotificationCenter
NotificationCenter.default.addObserver(self, selector: @Selector(.one), name: NotificationName, object: nil)
NotificationCenter.default.addObserver(self, selector: @Selector(.two), name: SecondNotificationName, object: nil)
...
```

### 작동
Delegate는 특정 protocol을 구현함으로써 위임하는 객체에 대해 알 필요 없이 프로토콜을 통해 대신 메소드를 사용할 수 있다.
Notification은 하나의 공유되는 default NotificationCenter 객체에 observer를 등록함으로써 정보를 브로드캐스트하고, observer들은 수동적으로 정보를 받는 개념이다. 하나의 객체가 여러 specific한 notification들에 대해 개별적으로 등록은 가능할지만, 객체가 원할 때 특정 메소드를 사용한다거나 정보를 받아올 수는 없다.

### 사용처
관계와 작동의 차이에 따라 두 가지는 사용하는 곳도 조금 달라진다.
iOS에서 Delegate는 주로 MVC 패턴을 만족하기 위해 View와 ViewController 사이에서 사용되거나, 프레임워크를 만들 때 사용자들이 디테일한 내용을 알지 못하더라도 프로토콜을 통해 메소드를 사용할 수 있도록 하기 위해서 사용된다.
반면 Notification은 특정 이벤트가 여러 객체에 전달되어야 할 때 주로 사용된다. register-> post-> receive의 간단한 메커니즘을 가지고 있지만 observer들이 많아지면 상호작용하는 객체들간의 관계를 파악하기 어려워진다는 단점이 있다.