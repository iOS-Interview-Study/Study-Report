# [Week 2] Fezz

## iOS 앱을 만들고, User Interface를 구성하는 데 필수적인 프레임워크 이름은 무엇인가?

**먼저 정답부터 말하자면 `UIKit` 이다.**

`UIKit`을 알아보면서 우리가 자주 사용하고 있는 프레임워크를 살펴보도록 하자 

- **Cocoa Touch Framework**

- **Cocoa Framework**

- **Foundation Framework**

- **UIKit**

<br/>



### Cocoa Touch Framework

<img src="https://raw.githubusercontent.com/Fezravien/UploadForMarkdown/forUpload/img/img-20210701235452106.png" alt="img" style="zoom:60%;" />

Cocoa Touch Framwork는 **UIKit / Foundation / CoreData / MapKit / CoreAnimation 등**.. 포함하고 있는 `Framework`로 iOS, iPadOS 등 애플 기기에서 구동되는 App을 개발하기 위해 사용하는 통합 프레임워크이다.

그 중 가장 많이 사용되고 있는 것이 `UIKit`, `Foundation` 이다.

- Objective-C Runtime을 기반으로 한다.
- Objective-C의 최상위 클래스인 `NSObject`를 상속한다.

Objective-C Runtime을 기반으로 하지만 우리가 사용하는 Swift와 Cocoa Touch Framework는 완벽하게 호환되서 굳이 알지 않아도 사용이 가능하다. 또한 Cocoa Touch Framework 안에 있는 모든 클래스는 모두 `NSObject`를 상속 받고 있다.

<br>

### Cocoa Framework

<img src="https://raw.githubusercontent.com/Fezravien/UploadForMarkdown/forUpload/img/img-20210701235434221.png" alt="img" style="zoom:60%;" />

Touch 만 빠진 Cocoa Framework는 뭘까?

이것은 MacOS App을 개발할 때 사용된다. 단지 `UIKit`이 아닌 `Appkit`을 가지고 있다.

<br>

### Foundation Framework

`Cocoa touch`, `Cocoa` Famework 모두에 속해 있는 Foundation Framework 부터 살펴보자 

가장 기본적인 `데이터 타입(String, Int)` 부터 `자료구조`, 각종 `구조체`나 `타이머`, `네트워크 통신`, `파일 관리` 등 기본적이나 프로그램의 중심을 담당하는 것이 포함되어 있다.

⚠️ Import Foundation 을 지워도 `String`, `Int`가 되는 이유

Foundation Framework에 있는 친구들은 `NS` 접두어가 있는 얘들이다.

`String`, `Int` 는 Swift의 기본 자료형이고 ! 

<br>



### UIKit

이제 이 질문의 답인 `UIKit`에 대해 살펴보자 

> UIKit provides many of your app’s core objects, including those that interact with the system, run the app’s main event loop, and display your content onscreen.
>
> The structure of UIKit apps is based on the Model-View-Controller (MVC) design pattern, wherein objects are divided by their purpose.
>
> UIKit provides most of the view objects, although you can define custom views for your data, as needed.
>
> The UIKit and Foundation frameworks provide many of the basic types that you use to define your app’s model objects.

**화면이나 유저 인터페이스, 앱의 동작 등 기능 구현을 주로 담당하는 프레임워크이다.** 라고 요약할 수 있을 것 같다!

화면에 표현되는 여러가지 콘텐츠를 보유하고, 화면의 구조를 만들어 관리하며, 사용자와 상호 작용(반응)을 한다.

- 스크린에 View를 출력하는 `Window`
- 화면을 그리는 `View`
- Window와 View를 연결해주는 `ViewController`

<br>

#### UIKit 계층구조 

<img src="https://raw.githubusercontent.com/Fezravien/UploadForMarkdown/forUpload/img/img-20210702000444690.png" alt="img" style="zoom:42%;" />



⚠️ `UIKit`은 `Foundation`을 가지고 있다 !

<br>

---

참고 📄

[apple documentation - UIkit](https://developer.apple.com/documentation/uikit)

[About App Development with UIKit](https://developer.apple.com/documentation/uikit/about_app_development_with_uikit)

[iOS) UIKit / Foundation / Cocoa / Cocoa Touch](https://babbab2.tistory.com/51)



