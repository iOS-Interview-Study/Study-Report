# Week 3 - Fezz

## 모든 View Controller 객체의 상위 클래스는 무엇이고 그 역할은 무엇인가?



### UIViewController란?

앱에서 **View 계층을 관리하는 객체**이다.

<br>

#### ViewController의 주요 역할

- 데이터의 변경에 대한 응답으로 **View의 콘텐츠를 업데이트**
- View들을 통해 **유저와 상호작용**
- View의 사이즈를 정의하고 **전체적인 인터페이스 레이아웃을 관리**
- 앱의 다른 ViewController 같은 **다른 뷰 객체들과 상호작용**

<br>

---

### UIResponder란?

UIResponder는 **이벤트에 응답하거나 이벤트를 처리하는 추상 인터페이스**이다.

UIResponder의 인스턴스인 Responder 객체는 **UIKit App의 이벤트 처리의 백본(backbone)**이 됩니다. **UIApplication**, **UIViewController**, 그리고 모든 **UIView**와 같은 많은 주요 객체들이 Responder라고 할 수 있습니다. 이벤트가 발생하면, UIKit는 이 Responder 객체들이 이벤트를 처리할 수 있도록 신속하게 전달한다. 

여기서 이벤트라 함은, **Touch Event, Motion Event, Remote-Control Event, Press Event**들이라고 할 수 있습니다. 만약 **특정 타입의 이벤트를 처리하기 위해서는 Responder가 연관된 메서드를 오버라이딩**해야합니다. 예를 들어, Touch Event를 처리하기 위해서는 Responder 가 [touchesBegan(_ : with :)](https://developer.apple.com/documentation/uikit/uiresponder/1621142-touchesbegan), [touchesMoved(_ : with :)](https://developer.apple.com/documentation/uikit/uiresponder/1621107-touchesmoved), [touchesEnded(_ : with :)](https://developer.apple.com/documentation/uikit/uiresponder/1621084-touchesended), [touchesCancelled(_ : with :)](https://developer.apple.com/documentation/uikit/uiresponder/1621116-touchescancelled)와 같은 메서드를 구현해야하죠. Touch가 발생한 경우, Responder는 UIKit에서 제공되는 이벤트 정보를 사용해서 **터치되는 내용을 추적**하고, 앱의 **인터페이스를 상황에 맞게 업데이트**합니다. 

이벤트를 처리하는 것 외에도, UIKit Responder는 앱의 다른 부분에서 처리되지 않은 이벤트들을 관리합니다. 만약 해당 Responder가 이벤트를 처리하지 않는다면, 그 이벤트는 **Responder Chain**에 있는 다음 이벤트로 넘겨줍니다. UIKit는 Responder Chain을 동적으로 관리하는데, 이때 **어떤 객체가 다음에 이벤트를 받을지 우선순위를 미리 정합니다**. 예를 들면, View는 자신의 슈퍼뷰에게 이벤트를 넘겨주도록 하고, 뷰 계층의 루트뷰는 자신의 ViewController에게 이벤트를 넘겨주도록 합니다. 즉, 이 예시에 따르면 이벤트는 View -> View의 슈퍼뷰 == 뷰 계층의 루트뷰 -> 자신의 ViewController 로 전달 되죠.

![img](https://raw.githubusercontent.com/Fezravien/UploadForMarkdown/forUpload/img/img-20210711160543484.png)



Responder은 보통 UIEvent 객체를 처리하지만, input view를 통해 들어오는 커스텀한 입력을 받을 수도 있습니다. Input View의 대표적인 예가 바로 시스템 키보드죠. 유저가 스크린에서 UITextField나 UITextView를 클릭하면, 클릭된 View는 First Responder가 되고 시스템 키보드 같은 input view를 보여주게 됩니다. 이런식으로, 우리는 어떠한 Responder가 활성화될 때 custom input view를 만들어서 보여줄 수 있습니다. 우리는 이렇게 Responder가 가진 custom input view와 어떠한 input view 프로퍼티를 연결할 수 있겠죠?

<br>

---

참고 📄 

https://beenii.tistory.com/166 [끄적이는 개발노트]