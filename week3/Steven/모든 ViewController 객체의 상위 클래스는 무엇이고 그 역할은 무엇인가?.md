# 모든 ViewController 객체의 상위 클래스는 무엇이고 그 역할은 무엇인가?

> UIResponder

- 사용자의 이벤트를 받고 처리하는 추상 인터페이스
- UIApplication, UIViewController, UIView 모두 상속하고 있는 클래스

### First Responder

- 사용자의 이벤트를 가장 처음 받는 Responder

> 이벤트 타입에 따른 FirstResponder
![4](https://user-images.githubusercontent.com/35272802/125194273-753a0400-e28b-11eb-9917-f6a322ad872d.jpeg)


- 해당 Responder가 이벤트를 처리하지 못하면 상위의 인스턴스로 넘어감. 이를 `Responder Chain` 이라고 함.

### Responder Chain
![5](https://user-images.githubusercontent.com/35272802/125194298-97338680-e28b-11eb-8168-28b6597982cc.png)


- 이벤트가 처리 되지 못하면 계속 상위 객체로 넘어가게 됨.
- 만약 최상단인 AppDelegate에서 까지 처리하지 못하면 해당 이벤트는 버려지게 됨.

### UITextField, UITextView, UISearchBar에서의 First Responder

- 위 3개의 뷰는 다른 뷰들과 다름. `canBecomeFirstResponder` 메소드가 true를 리턴하게 오버라이드 되있음.
- 이유는 모두 키보드를 사용해야 하기 때문임 → 뷰들을 클릭하면 키보드가 뜸
- 만약 ViewController가 뜨자마자 키보드를 바로 띄우고 싶으면 해당 뷰에 `becomeFirstResponder` 를 호출 하면 됨.

### 참고자료

[https://developer.apple.com/documentation/uikit/uiresponder](https://developer.apple.com/documentation/uikit/uiresponder)

[https://developer.apple.com/documentation/uikit/touches_presses_and_gestures/using_responders_and_the_responder_chain_to_handle_events](https://developer.apple.com/documentation/uikit/touches_presses_and_gestures/using_responders_and_the_responder_chain_to_handle_events)

[https://jcsoohwancho.github.io/2019-07-25-Responder와-Responder-Chain,-그리고-First-Responder/](https://jcsoohwancho.github.io/2019-07-25-Responder%EC%99%80-Responder-Chain,-%EA%B7%B8%EB%A6%AC%EA%B3%A0-First-Responder/)

[https://zeddios.tistory.com/538](https://zeddios.tistory.com/538)
