# 모든 View Controller 객체의 상위 클래스는 무엇이고 그 역할은 무엇인가?

## UIResponder

이벤트 응답 등을 처리하는 추상 인터페이스

![image-20210711201905624](/Users/ryan-son/Library/Application Support/typora-user-images/image-20210711201905624.png)

리스폰더 객체(Responder objects), 즉 UIResponder의 인스턴스는 UIKit **앱 이벤트 처리 백본(backbone)을 구성**합니다. 

많은 핵심 객체는 UIApplication객체, UIViewController객체 및 모든 UIView객체(UIWindow포함)을 포함하여 리스폰더입니다. 

**이벤트가 발생하면, UIKit은 이를 처리할 수 있도록 앱의 리스폰더 객체에 전달합니다.** 

터치이벤트, 모션이벤트, 원격제어 이벤트 및 press이벤트를 비롯한 여러 종류의 이벤트가 있습니다. 특정 타입의 이벤트를 처리하려면, 리스폰더가 해당 메소드를 override해야합니다

예를들어 터치 이벤트를 처리하기 위해 리스폰더는 **[`touchesBegan(_:with:)`](https://developer.apple.com/documentation/uikit/uiresponder/1621142-touchesbegan), [`touchesMoved(_:with:)`](https://developer.apple.com/documentation/uikit/uiresponder/1621107-touchesmoved), [`touchesEnded(_:with:)`](https://developer.apple.com/documentation/uikit/uiresponder/1621084-touchesended)****,그리고 ** `touchesCancelled(_:with:)` 를 구현합니다. 터치이벤트의 경우, 리스폰더는 UIKit에서 제공한 이벤트 정보를 사용하여 해당 터치의 변경사항을 추적하고, 앱의 인터페이스를 적절하게 업데이트해야합니다.

이벤트처리 이외에도 **UIKit 리스폰더는 처리되지 않은 이벤트를 앱의 다른 부분으로 전달하는 작업을 관리**합니다. 

**주어진 리스폰더가 이벤트를 처리하지않으면, 리스폰더 체인의 다음 이벤트로 해당 이벤트를 전달합니다**. UIKit은 미리 정의된 규칙을 사용하여 리스폰더 체인을 동적으로 관리하여 어떤 객체가 이벤트를 수신한 다음에 어떤 객체를 선택할지 결정합니다. 예를들어 View는 이벤트를 상위 View로 전달하고, 계층의 **루트 View**는 이벤트를 해당 ViewController로 전달합니다. 

리스폰더는 UIEvent객체를 처리하지만, input View를 통해 사용자 정의 input을 허용 할 수 있습니다. 시스템의 키보드는 input View의 가장 분명한 예입니다. 사용자가 UITextField및 UITextView객체를 화면 상에서 tap하면 View가 첫번쨰 리스폰더가 되어 시스템 키보드 input View가 표시됩니다. 마찬가지로, 시스템 정의 input View를  만ㄷ르어 다른 리스폰더가 활성ㄷ화 될 떄 표시할 수 있습니다. 사용자 지정 input View를 리스폰더와 연결하려면, 해당 View를 리스폰더의 `inputView프로퍼티에 할당합니다.`

# 참고자료

- [UIResponder](https://developer.apple.com/documentation/uikit/uiresponder) - Apple Developer
- [iOS ) UIResponder](https://zeddios.tistory.com/538) - Zedd0202 Blog