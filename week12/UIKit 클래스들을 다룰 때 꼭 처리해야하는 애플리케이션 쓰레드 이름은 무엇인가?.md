# UIKit 클래스들을 다룰 때 꼭 처리해야하는 애플리케이션 쓰레드 이름은 무엇인가?

---



## Main Thread

앱의 유저인터페이스와 관련되거나 `UIResponder` 로부터 파생되는 클래스와 관련된 작업은 앱의 main thread에서만 실행되어야 합니다.



[Apple Developer Document | UIKit](https://developer.apple.com/documentation/uikit/)

