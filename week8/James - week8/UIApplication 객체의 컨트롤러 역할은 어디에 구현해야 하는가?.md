# UIApplication 객체의 컨트롤러 역할은 어디에 구현해야 하는가?

---



`UIApplicationDelegate` 를 채택하고 있는 `AppDelegate` 클래스에서 `UIApplication` 의 컨트롤러 역할이 구현되어야 한다고 생각합니다.

왜냐하면 [공식문서](https://developer.apple.com/documentation/uikit/uiapplication) 에서 볼 수 있듯이 UIApplication 클래스는 `UIApplicationDelegate` 를 채택하고 있는 delegate를 정의한다고 적혀있고 해당 delegate는 앱 실행, 메모리부족 경고, 앱 종료 등과 같은 이벤트를 처리하게 됩니다. 

이 delegate는

- 앱의 기반 데이터 구조를 초기화 시키고
- 앱의 scene 설정을 해주고
- 메모리부족 경고, 다운로드완료와 같은 앱 밖으로부터 오는 메세지에 반응하고
- 앱을 타겟하는 이벤트에 반응하고
- Push notification과 같은 앱 시작시 발생하는 서비스를 등록 해주는 작업을 하게됩니다.



이는 애플에서 정의하는 [MVC 패턴](https://developer.apple.com/library/archive/documentation/General/Conceptual/DevPedia-CocoaCore/MVC.html) 에서 말하는 controller의 역할이지 않을까 싶습니다. UIApplication을 대신해서 앱의 전반적인 이벤트 처리를 해주는게 `UIApplicationDelegate` 를 채택하고 있는 `AppDelegate` 클래스이기 때문에 저는 UIApplication 객체의 컨트롤러 역할은 `AppDelegate` 이고 위에 언급된 이벤트에 대한 처리를 해당 클래스에서 해야한다고 생각합니다.



[참고]:

[Apple Developer Document | UIApplication](https://developer.apple.com/documentation/uikit/uiapplication)
