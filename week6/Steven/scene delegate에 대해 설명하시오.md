# scene delegate에 대해 설명하시오.

![1](https://user-images.githubusercontent.com/35272802/127769794-af628331-defa-4b5c-bb33-6331314391d9.png)

## Scene 이란

- ios 12 이전까지의 window의 개념을 대체한다.
- UI 하나의 인스턴스를 나타내는 windows와 viewControllers가 들어있다.
- 앱 하나에 여러개가 생길 수 있다.

## Scene Delegate

- 한개의 Scene에 대한 라이프 사이클을 담당한다.

### Scene Session

- scene의 고유의 런타임 인스턴스를 관리한다.
- AppDelegate는 SceneSession을 통해서 scene에 대한 정보를 업데이트 받는다.
- 이 Session의 라이프 사이클을 AppDelegate가 한다.

## Scene Delegate 메소드

- `scene(_: willConnectTo: options:)` : Scene이 앱에 추가 될 때 호출
- `sceneDidDisconnect(_:)`: scene의 연결이 해제될 떄 호출
- `sceneDidBecomeActive(_:)` :  app switcher에서 선택되는 등 scene과의 상호작용이 시작될때 호출
- `sceneWillResignActive(_:)` : 사용자가 scene과의 상호작용을 중지할 때 호출(다른 화면으로의 전환과 같은 경우)
- `sceneWillEnterForeground(_:)` : scene이 foreground로 진입할 때 호출
- `sceneDidBackground(_:)`: scene이 background로 진입할 때 호출

### ios13부터의 라이프 싸이클

앱 클릭 → didFinishLaunchingWithOptions → configurationForSession → willConnectToSession → scene(_:willConnectTo) → 화면에 앱 등장 → willResignActive,didEnterBackground → didDisconnect → didDiscardSceneSessions

### 참고자료

[https://velog.io/@dev-lena/iOS-AppDelegate와-SceneDelegate](https://velog.io/@dev-lena/iOS-AppDelegate%EC%99%80-SceneDelegate)

[https://ios-development.tistory.com/53](https://ios-development.tistory.com/53)

[https://eunjin3786.tistory.com/164](https://eunjin3786.tistory.com/164)

[https://purple-log.tistory.com/28](https://purple-log.tistory.com/28)
