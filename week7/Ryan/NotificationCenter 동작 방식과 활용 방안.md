# NotificationCenter 동작 방식과 활용 방안에 대해 설명하시오.
## Notification Center
> Notification 송수신을 관리하는 타입. 

- 특정 기준을 충족하는 Notification을 모든 관찰자에게 통지하며 Notification 정보는 NSNotification 객체에 캡슐화되어 있습니다.
- 게시하는 객체와 관찰하는 객체가 같을 수 있습니다.
- `userInfo` 딕셔너리를 이용해 정보를 담아 알림을 전송할 수 있습니다.

### 종류
- `NSNotificationCenter` 클래스는 하나의 프로세스 내에서의 Notification을 관리합니다. 
`- NSDistributedNotificationCenter` 클래스는 단일 시스템의 여러 프로세스에 걸친 Notification을 관리합니다.

### 동작 방식
1. 클라이언트 객체는 자신을 다른 객체가 게시(post)한 특정 알림의 관찰자로 `Notification Center`에 등록합니다.
2. 이벤트가 발생하면 객체가 적절한 알림을 `Notification Center`에 게시(post)합니다.
3. `Notification Center`는 등록된 각 관찰자에게 메시지를 발송하여 Notification을 하나의 인자로 전달합니다.

### 활용 방안
- 알림이나 정보 전달 측면에서 Delegate 방식에 비해 1:多의 관계를 맺기 용이하기에 하나의 notification에 대한 관찰자가 많은 상황에서 활용할 수 있습니다.
- 시스템에서 발생하는 이벤트는 상황이 발생하면 시스템에서 post하기 때문에 관찰자로 등록하여 사용할 수 있습니다 (e.g. 키보드 show/hide, 코어데이터 저장 등). 

# 참고자료
- [Nofication Centers](https://developer.apple.com/library/archive/documentation/Cocoa/Conceptual/Notifications/Articles/NotificationCenters.html#//apple_ref/doc/uid/20000216-BAJGDAFC) - Apple Developer Documentation Archive