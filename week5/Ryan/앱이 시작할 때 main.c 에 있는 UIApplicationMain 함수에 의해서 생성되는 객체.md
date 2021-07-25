# 앱이 시작할 때 main.c 에 있는 UIApplicationMain 함수에 의해서 생성되는 객체는 무엇인가?

```swift
#import <UIKit/UIKit.h>
#import "AppDelegate.h"

int main(int argc, char * argv[]) {
    NSString * appDelegateClassName;
    @autoreleasepool {
        // Setup code that might create autoreleased objects goes here.
        appDelegateClassName = NSStringFromClass([AppDelegate class]);
    }
    return UIApplicationMain(argc, argv, nil, appDelegateClassName);
}
```

![](https://images.velog.io/images/ryan-son/post/7c8e34b2-07d5-485f-b604-a305cc9dab36/image.png)

> application 객체와 application delegate를 만들고 event cycle을 설정합니다.

## UIApplication
![](https://images.velog.io/images/ryan-son/post/a116be0e-f047-4097-8fd8-ef64f7234c53/image.png)

모든 iOS app은 정확히 하나의 `UIApplication` 인스턴스 (혹은 드물게 서브클래싱한 인스턴스)를 가집니다. 이는 앱이 실행될 때 시스템이 `UIApplicationMain(_:_:_:_:)` 함수를 호출함으로써 `shared`라는 이름의 싱글턴 UIApplication 객체를 생성합니다.

앱의 Application 객체는 들어오는 사용자 이벤트의 초기 라우팅을 처리합니다. 컨트롤 개체(UIControl 클래스의 인스턴스)가 자신에게 전달한 작업 메시지를 적절한 대상 객체로 전달합니다. Application 객체는 열려 있는 창 목록(UIWindow 개체)을 유지함으로써 이를 통해 응용 프로그램의 UIView 객체를 검색할 수 있습니다.

UIApplication 클래스는 UIApplicationDelegate 프로토콜을 준수하는 delegate를 정의하며 프로토콜의 일부 메서드를 구현해야 합니다. 애플리케이션 객체는 대리인에게 앱 실행, 메모리 부족 경고 및 앱 종료와 같은 중요한 런타임 이벤트를 알려 적절하게 대응할 수 있도록 설정할 수 있습니다.

UIApplication 객체를 통해 할 수 있는 것

- 들어오는 터치 이벤트를 일시적으로 중단(`beginIgnoringInteractionEvents()`)
- 원격 알림 등록(`registerForRemoteNotifications()`)
- 실행 취소 UI 트리거(`applicationSupportsShakeToEdit`)
- URL 스키마(`canOpenURL(_:)`)를 처리하기 위해 등록된 설치된 앱이 있는지 확인
- 백그라운드에서 작업을 완료할 수 있도록 앱 실행 확장(`beginBackgroundTask(expirationHandler:)` 및 `beginBackgroundTask(withName:expirationHandler:)`)
- 로컬 알림 예약 및 취소(`scheduleLocalNotification(_:)` 및 `cancelLocalNotification(_:)`)
- 원격 제어 이벤트의 수신 조정(`beginReducingRemoteControlEvents()` 및 `endReducingRemoteControlEvents()`)
앱 수준 상태 복원 작업 수행(State Restoration Behavior 관리 task group의 메서드들)

# 참고자료
- [UIApplicationMain(_:_:_:_:)](https://developer.apple.com/documentation/uikit/1622933-uiapplicationmain/) - Apple Developer
- [UIApplication](https://developer.apple.com/documentation/uikit/uiapplication) - Apple Developer