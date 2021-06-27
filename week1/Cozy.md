

##### 1. 앱이 In-Active 상태가 되는 시나리오를 설명하시오.

```
App이 실행중이지만, 이벤트를 받지 않는 상태이며, 다른 상태로 넘어가기 전에 앱은 반드시 이 상태를 거친다.
- Foreground 상태에서 전화나 메시지 같은 외부의 interrupt 발생 시
- 미리 알림 같은 특정 알림창이 화면을 덮어 실질적으로 event를 받지 못하는 상태 
- multitasking window로 진입 시(iOS 13 이상)
```

<img width="503" alt="In-Active" src="https://user-images.githubusercontent.com/29880961/123540849-04b7c100-d77c-11eb-9b13-a08e8daf0ed9.png">

 

##### Respond to Scene-Based Life-Cycle Events

Scene는 기기에서 실행되는 UI의 한 인스턴스를 나타낸다. 각 Scene는 고유한 수명주기를 갖고 있기 때문에 각기 다른 실행 상태를 가질 수 있다. 

현재 app의 상태를 보고 어떠한 것을 할 수 있고 없는지를 결정한다. 

`foreground 앱`은 user가 사용하기 때문에 user의 관심을 갖게 되고, 시스템 자원적으로 우선순위를 갖게 된다. 

​		- `In-active` : 앱이 실행중이지만, 아무런 이벤트를 받지 않은 상태

​		- `Active`: 앱이 실행중이며 현재 이벤트를 받고 있고, 발생한 상태 



`background 앱`은 화면 밖의 일이므로 가능한 작업을 수행하지 않도록 한다. 



앱의 상태가 바뀔 때 우리의 앱은 이에 대응한 동작을 조정해야하며,  UIKit는 적절한 delegate object의 메서드를 호출하여 우리에게 알려준다.  



iOS 12까지는 하나의 앱에 하나의 window(창)인 `UIApplicationDelegate`를 채택하여 한 앱을 동시에 키는 것이 불가능 하였으나, iOS 13부터는 window의 개념이  <u>Scene 기반 앱의 수명주기</u>로 대체되어  `UISceneDelegate`를 사용한다. 즉, iOS 13에서는 하나의 앱에서 여러개의 scene을 가질 수 있게 되어 한 앱을 동시에 키는 것이 가능해졌다. 



##### iOS 13부터 AppDelegate가 하는 일 

<img src="/Users/lumi/Library/Application Support/typora-user-images/스크린샷 2021-06-27 오후 5.31.18.png" alt="스크린샷 2021-06-27 오후 5.31.18" style="zoom:50%;" />





##### 2. UINavigationController 의 역할이 무엇인지 설명하시오.

: 화면을 나타내주는 VC를 관리하는 역할이다. `view `들을 네비게이션 스택에 담아두어 `container viewcontroller` 라고 불리기도 한다. 

컨테이너에 담기는 뷰 컨트롤러들을 `contents viewcontroller` 라고 한다. 



스택의 가장 아래에 있어 빠져나오지 못하는 특별한 vc를 `root viewcontroller` 라고 한다. 

stack의 vc를 push/pop하여 순차적으로 꺼내올 수도 있고, 뒤로가기나 별도의 제스처를 이용하여 순서에 상관없이 vc를 보여줄 수도 있다. 





##### 3. Delegation와 Notification 방식의 차이점에 대해 설명하시오.

`Delegation` 

: 한 속성에 대한 다양한 이벤트를 처리할 때 사용(tableview의 delegate함수와 같이 셀 갯수, 셀 설정, 셀 크기 등 꼭 해줘야 하는 이벤트가 다양할 때)

- 보통 `Protocol`을 정의하여 사용한다. `protocol`을 사용할 때에는 지정된 객체가 어떠한 일을 해야하는지에 대한 메소드의 원형을 적어놓으므로, 세팅이 된 후 이전 객체에서 어떤 이벤트가 일어났을 때에 지정한 객체에게 알려주는 역할을 한다. 
- 많은 줄의 코드가 필요하며, 순환 참조가 되고 있는지 확인해보아야 한다. 
- 알려주는 역할 뿐만이 아니라 정보를 받을 수도 있다. 



`Notification`

: 한 이벤트에 대해 다수의 수신자가 있을 경우에 사용(다음 화면이 아니라 depth가 깊은 상속관계가 있는 화면들에 이벤트를 줄 때, 화면간의 연결관계가 없을 때)

- singleton 객체를 통해 옵저버를 등록한 객체들에게 이벤트의 발생 여부에 대한 notification을 post 하는 방식으로 사용한다. 

- Notification name이라는 Key값을 통해 정보를 보내고 받을 수 있다. 

- 코드가 비교적 단순하며, 다수의 객체들에게 동시에 이벤트의 발생을 알려줄 수 있다. 

- 트래킹이 쉽지 않고, post 한 이후 정보를 받을 수 없다. 

  





##### 4. ARC란 무엇인지 설명하시오.

```
컴파일러가 자동으로 메모리 관리 코드를 삽입한다. 코드의 양도 적고, 안정성 또한 높다. 

(Swift는 ARC만을 지원하기 때문에 Reference Counting에 대하여 추가 정리를 아래에 해두었다)

 

인스턴스, 즉 객체는 하나 이상의 참조카운팅(소유자)가 있을 때에 메모리에 유지된다. 

참조한다는 것은 사본이 아니라 원본 자체를 참조한다는 것.

 

참조카운팅이 0, 즉 소유자가 없으면 메모리에서 즉시 제거가 된다. 

제거 시점을 파악하기 위하여 참조 카운팅을 하여 소유자의 수를 저장하는 것이 Reference Count이다.

 

클래스의 인스턴스를 특정 변수에 저장하면, 변수가 소유자가 되며 참조 카운트는 1이 된다.

또 다른 변수에 클래스의 인스턴스를 저장하면 카운트는 2가 된다. 


참조 방식은 Strong, Weak, Unowned 가 있다. 
```





##### 5. hugging, resistance에 대해서 설명하시오.

<img src="/Users/lumi/Library/Application Support/typora-user-images/스크린샷 2021-06-27 오후 6.09.33.png" alt="스크린샷 2021-06-27 오후 6.09.33" style="zoom:50%;" />

**content hugging**

: 두 객체 중 하나가 커져야 하는 상황에서 사용.

최대 크기에 대한 제약. 우선순위가 높으면 나의 크기가 유지되고, 우선순위가 낮으면 크기가 늘어난다. 

**Content Compression Resistance** 

: 두 객체 중 하나가 작아져야 하는 상황에서 사용.

최소 크기에 대한 제한. 우선순위가 낮으면 크기가 작아진다. 







