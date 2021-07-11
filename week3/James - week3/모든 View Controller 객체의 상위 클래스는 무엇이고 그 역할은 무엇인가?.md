# 모든 View Controller 객체의 상위 클래스는 무엇이고 그 역할은 무엇인가?

---



## UIResponder

> 이벤트에 반응하고 처리 해 주는 추상적인 인터페이스



`Responder` 객체는 UIKit 앱이 이벤트를 처리하는 중추 역할을 합니다. application 객체, viewcontroller 객체, 그리고 모든 view 객체(uiwindow를 포함하는)가 responder 객체 이기도 합니다.



터치이벤트, 모션이벤트, 원격 조종 이벤트, 입력 이벤트 등 여러 이벤트가 존재하고 특정한 이벤트타입을 처리하기 위해서는 responder 객체는 관련된 메서드들을 오버라이드 해야 합니다. 예를 들어 터치 이벤트 처리를 위해서는 리스폰더 객체는[`touchesBegan(_:with:)`](https://developer.apple.com/documentation/uikit/uiresponder/1621142-touchesbegan), [`touchesMoved(_:with:)`](https://developer.apple.com/documentation/uikit/uiresponder/1621107-touchesmoved), [`touchesEnded(_:with:)`](https://developer.apple.com/documentation/uikit/uiresponder/1621084-touchesended), 그리고 [`touchesCancelled(_:with:)`](https://developer.apple.com/documentation/uikit/uiresponder/1621116-touchescancelled) 와 같은 메서드가 필요합니다. UIKit이 제공하는 이벤트 정보를 가지고 리스폰더 객체는 터치의 변화를 캐치하고 터치에 따라서 앱 인터페이스를 업데이트 시켜줍니다.

이벤트 처리에 더해서 UIKit responder 객체는 처리되지 않은 이벤트를 다른 시스템 객체에게 전달하는 역할을 합니다. 만약 주어진 responder 객체가 이벤트를 처리하지 않는다면 다음 responder chain에 있는 객체에게 해당 이벤트를 전달합니다. UIKit이 이 responder chain을 동적으로 관리합니다. UIKit은 정해진 규칙에 따라 처리되어야 하는 이벤트가 responder chain 속 어떤 객체에게 전달되어야 할지 결정합니다. 예를 들면 서브뷰가 처리하지 못한 이벤트는 부모뷰에게 넘겨지고 rootview가 처리하지 못한 이벤트는 해당 view의 view controller에게 이관됩니다.

Responder 객체는 이벤트 객체를 처리하기도 합니다만 input view를 통해 커스텀 입력을 받아서 처리하기도 합니다. 화면의 키보드가 가장 좋은 예 중 하나입니다. 유저가 화면에 보이는 텍스트필드 또는 텍스트뷰 객체를 터치할 경우 view가 가장 처음의 responder가 되고 입력되는 값을 화면에 보여줍니다. 커스텀 입력 view를 responder와 결합시키려면 view에게 responder의 inputview 프로퍼티를 할당해야합니다.



## 결론

모든 View Controller의 상위 클래스는 UIResponder이고 UIResponder는 터치, 모션, 입력과 같은 이벤트를 처리 해 주는 추상적인 인터페이스 입니다. 



[UIResponder | Apple Developer Document](https://developer.apple.com/documentation/uikit/uiresponder)

