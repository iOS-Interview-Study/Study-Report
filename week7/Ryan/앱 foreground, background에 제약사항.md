# 앱이 foreground에 있을 때와 background에 있을 때 어떤 제약사항이 있나요?
Apple의 Article인 Preparing Your UI to Run in the Background에서는 Background에 진입하기 전에 아래와 같은 요소들을 처리할 것을 권고합니다. 반대로 말하면 Background 상태에서는 해당 작업들을 처리할 수 없거나 Foreground 상태일 때보다 까다롭다는 내용이라고도 할 수 있습니다.

## 비활성화에 대비해 앱을 조용히 만들기
큰 작업을 중지함으로써 앱을 조용히 만들고, 사용자의 데이터를 유지하도록 만듭니다.
- 사용자의 데이터를 디스크에 저장하고 열린 파일들을 모두 닫습니다.
- Dispatch 및 Operation Queue들을 중지합니다.
- 실행하기 위한 새로운 작업들을 예약하지 않습니다.
- 모든 활성화된 타이머를 무효화합니다.
- 게임 플레이를 자동으로 일시 중지합니다.
- 처리할 새로운 Metal 작업을 커밋하지 않습니다.
- 새로운 OpenGL 명령을 커밋하지 않습니다.

이 작업들은 아래와 같은 UIKit 메서드에서 작업하면 좋습니다.
- Scene based Lifecycle (iOS 13.0 ~): `sceneWillResignActive(_:) `
- App based Lifecycle (iOS 13.0 이전): `applicationWillResignActive(_:)`

## Background 진입에 대비해 리소스들을 해제하기
Background 상태로 전환됨에 따라 메모리와 같이 앱이 사용하던 공유 자원들을 해제합니다.

- 파일에서 직접 읽은 이미지 또는 미디어를 삭제합니다.
- 디스크에서 다시 만들거나 다시 로드할 수 있는 메모리 내 큰 개체를 모두 삭제합니다.
- 카메라 및 기타 공유 하드웨어 리소스에 대한 액세스를 해제합니다.
- 앱의 사용자 인터페이스에서 암호와 같은 중요한 정보를 숨깁니다.
- 경고 및 기타 임시 인터페이스를 해제합니다.
- 공유 시스템 데이터베이스에 대한 연결을 닫습니다.
- Bonjour 서비스에서 등록을 취소하고 연결된 수신 소켓을 모두 닫습니다.
- 모든 Metal 명령 버퍼가 예약되었는지 확인합니다.
- 이전에 제출한 모든 OpenGL 명령이 완료되었는지 확인합니다.

이 작업들은 아래와 같은 UIKit 메서드에서 작업하면 좋습니다.
- Scene based Lifecycle (iOS 13.0 ~): `sceneDidEnterBackground(_:)`
- App based Lifecycle (iOS 13.0 이전): `applicationDidEnterBackground(_:)`

## 앱 스냅샷을 위한 UI 준비
앱이 Background로 들어가고 Delegate 메서드가 반환되면 UIKit는 App switcher에서 활용하기 위해 앱의 현재 UI의 스냅샷을 만듭니다. 

앱 UI에는 암호나 신용카드 번호와 같은 중요한 사용자 정보가 포함되어서는 안 됩니다. 인터페이스에 이러한 정보가 포함된 경우 Background에 진입할 때 해당 정보를 View에서 제거합니다.

## Background 상태에서 할 수 있는 것: 중요 이벤트에 대한 반응
- AirPlay, or Picture in Picture video를 활용한 Audio communication
- 사용자를 위한 위치에 민감한 서비스들
- Voice over IP (VoIP)
- 외부 액세러리를 이용한 커뮤니케이션
- Bluetooth LE 액세서리 또는 이로 전환한 디바이스를 이용한 커뮤니케이션
- 서버를 통한 정기 업데이트
- Apple Push Notification service (APNs)과 관련된 지원
![](https://images.velog.io/images/ryan-son/post/a250cf92-9146-4362-9011-b52931aab9bb/image.png)

# 참고자료
- [Preparing Your UI to Run in the Background](https://developer.apple.com/documentation/uikit/app_and_environment/scenes/preparing_your_ui_to_run_in_the_background) - Apple Developer