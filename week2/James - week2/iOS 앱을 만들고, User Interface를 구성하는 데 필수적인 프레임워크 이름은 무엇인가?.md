# iOS 앱을 만들고, User Interface를 구성하는 데 필수적인 프레임워크 이름은 무엇인가?

---

정답은????

#### UIKit

#### UIKit

#### UIKit





## UIKit 이란?

> iOS 또는 tvOS 앱을 위해서 그래픽과 이벤트 기반 유저인터페이스를 그리고 관리하는 프레임워크



UIKit 프레임워크는 iOS 또는 tvOS 앱을 그리는데 필요한 기반을 제공해주는 프레임워크입니다.

- UIKit은 인터페이스를 구성하는데 필요한 window와 view architecture를 제공합니다.

- UIKit은 멀티터치와 같은 이벤트 처리를 위한 기반을 제공합니다.

- UIKit은 유저, 시스템, 그리고 앱 간의 상호작용을 조율해주는 main-run을 제공합니다.

그외 UIKit은 애니메이션 지원, 문서지원, 그림 그리고 출력 지원, 현재 기기에 대한 정보, 텍스트관리와 출력, 검색지원, 손쉬운사용 지원, 앱 확장 기능, 그리고 리소스 관리와 같은 기능을 합니다.



[중요]

UIKit class는 특별한 지시사항이 없는한 무조건 앱의 메인쓰레드에서 사용 해 주세요. 이 제한사항은 특별히 UIResponder를 상속하는 클래스 그리고 유저의 앱의 인터페이스를 관장하는 class에게 적용됩니다.



[UIKit | Apple Developer Document](https://developer.apple.com/documentation/uikit)

