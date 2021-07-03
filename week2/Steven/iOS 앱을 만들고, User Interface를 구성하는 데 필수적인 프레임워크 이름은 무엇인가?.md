# iOS 앱을 만들고, User Interface를 구성하는 데 필수적인 프레임워크 이름은 무엇인가?

uikit 이다! 그런데 uikit을 알기 전에 이를 포함 하는 개념인 Cocoa Touch Framework를 알아야 한다.

## Cocoa Touch Framework

- 코코아 터치 프레임워크란 ios 어플리케이션을 개발하기 위한 모든 것을 제공하는 프레임워크이다.
- 이 프레임워크 안에 우리가 자주 사용하는 `Uikit`, `Foundation` 이 있다.
- objective-c Runtime을 기반으로 하며, 모든 클래스들은 `NSobject` 로 부터 상속을 받는다.
- 참고로 mac os 어플리케이션을 위한 프레임워크는 `Cocoa Framework` 라고 한다.

![1](https://user-images.githubusercontent.com/35272802/124354762-87db8a00-dc48-11eb-816e-700d28467dcd.png)


## Uikit

- User Interface kit → 사용자와 상호작용하는 모든 것들을 제공해주는 프레임워크이다.
- 제스처 처리, 애니메이션, 그림 그리기, 이미지 처리, 텍스트 처리 등 사용자 이벤트 처리를 위한 클래스를 포함한다.
- 또한 테이블뷰, 슬라이더, 버튼, 텍스트 필드, 얼럿 창 등 애플리케이션의 화면을 구성하는 요소를 포함한다.
- UIKit 클래스 중 UIResponder에서 파생된 클래스나 사용자 인터페이스에 관련된 클래스는 애플리케이션의 메인 스레드(혹은 메인 디스패치 큐)에서만 사용해야 한다.
- UIKit은 iOS와 tvOS 플랫폼에서 사용한다.

### uikit 계층도

![2](https://user-images.githubusercontent.com/35272802/124354764-8b6f1100-dc48-11eb-9755-a7342734d27e.png)

## Foundation

- 데이터 스토리지 및 지속성, 텍스트 처리, 날짜 및 시간 계산, 정렬 및 필터링, 네트워킹을 비롯한 애플리케이션 및 프레임워크의 기본 기능 계층을 제공합니다.(공식 문서 번역)
- ui와 관련되지 않은 것들을 제공합니다.
    - swift의 기본 타입들(int, double, string, data, array 등등)
    - 날짜 및 시간 계산(date, datefomatter 등)
    - 앱 지원 기능들(Operation, NotificationCenter, Error 등)
    - 파일과 데이터 저장(FileManager, Codable)
    - 네크워킹(URLSession)
- uikit 안에 포함되어 있어서 uikit을 import 하면 따로 import하지 않아도 된다.

### 참고 자료

- [https://velog.io/@wan088/iOS-프레임워크-CocoaTouch-Foundation-UIkit-sjjzdqmte4](https://velog.io/@wan088/iOS-%ED%94%84%EB%A0%88%EC%9E%84%EC%9B%8C%ED%81%AC-CocoaTouch-Foundation-UIkit-sjjzdqmte4)
- [https://babbab2.tistory.com/51](https://babbab2.tistory.com/51)
- [https://0urtrees.tistory.com/62](https://0urtrees.tistory.com/62)
- [https://zdodev.github.io/swift/NSObject/](https://zdodev.github.io/swift/NSObject/)
- [https://kdgt-programmer.tistory.com/71](https://kdgt-programmer.tistory.com/71)
- [https://socialinnovator.tistory.com/6](https://socialinnovator.tistory.com/6)
