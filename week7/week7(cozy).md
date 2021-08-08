### 오토레이아웃을 코드로 작성하는 방법은 무엇인가?

##### 1. Initializer - NSLayoutConstraint

객체간의 레이아웃 관계 

`item1.attribute1 = multiplier × item2.attribute2 + constant`

```swift
let constraint = NSLayoutConstraint(item: label, attribute: .height, relatedBy: .equal, toItem: button, attribute: .height, multiplier: 1, constant: 0)
```



##### 2. Visual Format Language - 레이아웃의 시각적 표현

view :  [] (대괄호), view간의 연결: - (하이픈)

NSLayoutConstraint.constraints를 이용한다.

```swift
let horzFmt = "|[b]|"
let views: [String: Any] = ["b": blueView]
     
let horzConstraints = NSLayoutConstraint.constraints(withVisualFormat: horzFmt, options: [], metrics: nil, views: views)
```



##### 3. Anchor

```swift
blueView.leadingAnchor.constraint(equalTo: margins.leadingAnchor).isActive = true
```

제약을 주고 싶은 item의 `anchor` 프로퍼티에 접근한다. `Layout Anchor` 는 제약조건들을 생성하는 다양한 메소드를 가지고, 결과에 영향을 주는 방정식의 요소들만 parameter로 전달한다. 

또한,  `Layout Anchor` 는 추가적인 `type safety` 를 제공하는데, 이로 인해 잘못된 제약조건이 우연히 발생하는 것을 예방한다. 



------

### Singleton 패턴을 활용하는 경우를 예를 들어 설명하시오.

> #### Singleton Pattern
>
> 생성자가 여러 차례 호출되더라도, 실제로 생성되는 객체는 하나이다.
>
> 메모리의 할당을 최초 한번만 하고, 그 메모리에 인스턴스를 만들어서 사용한다.
>
> 공용으로 사용하고 싶을 때 쓰는 디자인 패턴
>
>
> 객체가 프로그램 내부에서 단 1개만 생성됨을 보장하며, 멀티 스레드에서 이 객체를 공유하고 동시에 접근하는 경우에 발생하는 동시성 문제도 해결해준다. 

Userdefaults - User의 정보를 저장하여 전역에 두고 Instance만 접근 가능하게 함

결제 시스템은 앱의 로직에서 항상 한번에 하나의 결제만을 수행해야 하며, 중복 결제가 이루어지지 않게 구현을 해야한다. 

```
class PaymentInfo {
    static let shared = PaymentInfo()

    var userInfo: String? 
    var payType: String? // 결제 방법
    var payData: String? // 결제 내용
    var payDateTime: String? // 결제일시
    
    private init() { }
}
```



##### Singletone의 장점과 단점

1) 장점:
   - 한번의 Instance만 생성하므로 메모리 낭비를 방지할 수 있다
   - 전역 Instance로 생성하기 때문에 다른 클래스들과의 자원 공유가 쉽다
   - 주로 공통된 객체를 여러개 생성해서 사용해야하는 상황들에서 사용한다. 

2. 단점: 

   - Singleton Instance가 너무 많은 일을 하거나, 많은 데이터를 공유하는 경우에는 다른 클래스의 Instance간의 결합도가 높아져 '개방-폐쇄 원칙'을 위배한다.
   - 수정과 테스트가 어려워진다. 

   참고:  https://babbab2.tistory.com/66 [개발자 소들이]

------



### 앱이 foreground에 있을 때와 background에 있을 때 어떤 제약사항이 있나요?

**foreground**에 있을 때에는 user가 사용중이기 때문에 메모리 및 기타 시스템 리소스에 높은 우선순위를 가진다.

시스템은 이러한 리소스를 사용할 수 있도록 필요에 따라 background 앱을 종료해주기도 한다.



**background**에 있을 때에는 화면 밖의 일이기 때문에 가능한 적은 메모리 공간을 사용해야 한다. 공유 시스템 리소스 해제, 이미지 객체 참조 해제 등 메모리 제한을 주며,  특정 유형의 앱만 백그라운드에서 실행 가능하도록 제한해야 한다. 

 참고

[1]  https://exception-log.tistory.com/184 

[2] https://memohg.tistory.com/124


------


### NotificationCenter 동작 방식과 활용 방안에 대해 설명하시오.

> #### Notification
>
> 노티피케이션 센터를 통해 등록된 모든 옵저버에게 정보를 뿌려주는 컨테이너.
> 등록된 노티피케이션은 노티피케이션 센터를 통해 정보를 전달하기 위한 구조체이다.



Notifiation의 주요 프로퍼티 

1) name: 알림 식별 태그

   `var name: Notification.Name`

2) object: 발송자가 옵저버에게 보내려고 하는 객체. 주로 발송자 객체를 전달하는 데에 쓰인다.

   `var object: Any?`

3) userInfo: 노티피케이션과 관련된 값 또는 객체의 저장소

   `var userInfo: [AnyHashable: Any]?`

   

> #### Notification Center
>
> 등록된 옵저버에 정보를 뿌릴 수 있는 노티피케이션 발송 매커니즘
>
> 등록된 옵저버에게 동시에 노티피케이션을 전달하는 클래스이다.
>
> Notification Center 클래스는 Notification을 발송하면, Notification Center에서 메시지를 전달한 Observer의 처리가 완료될 때까지 대기한다. (동기적 처리)



연결되어있지 않고, 독립적인 뷰 컨트롤러가 존재할 때 사용한다. 계속적으로 observing하고 있는 방법이라. 모든 데이터에 이러한 방법을 쓰면 굉장히 비효율적이므로 간단한 데이터를 넘길 때 사용한다. 



```swift
 NotificationCenter.default.post(name: NSNotification.Name("NotificationExample"), object: nil, userInfo: nil)
// Name에 해당하는 옵저버에 일을 수행 할 것을 post(요청)한다.
```



```swift
NotificationCenter.default.addObserver(self, selector: #selector(didRecieveNotification(_:)), name: NSNotification.Name("NotificationExample"), object: nil)
// addObserver를 통해 관찰자를 대기시키며, selector는 관찰자가 수행해야 할 업무를 의미한다.

 @objc func didRecieveNotification(_ notification: Notification) {
         print("Test Notification")
 }

```



observer는 계속하여 살아있으므로 제거는 따로 해주어야 한다. 

```swift
NotificationCenter.default.removeObserver(self, name: NSNotification.Name("NotificationExample"), object: nil)
```







