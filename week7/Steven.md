# 오토레이아웃을 코드로 작성하는 방법은 무엇인가? (3가지)

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/26d760c9-5947-4661-93f7-16f6787a0bb6/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/26d760c9-5947-4661-93f7-16f6787a0bb6/Untitled.png)

> 공식 문서 예시에서 파란뷰와 빨간 뷰 사이에 8인 간격을 다양한 방법으로 구현해 보려고 한다.

### NSLayoutAnchor

```swift
blueView.trailingAnchor.constraint(equalTo: redView.leadingAnchor, constant: 8.0).isActive = true
```

### NSLayoutConstraint

```swift
NSLayoutConstraint.init(item: blueView, 
												attribute: .trailing, 
												relatedBy: .equal, 
												toItem: redView, 
												attribute: .leading, 
												multiplier: 1.0, 
												constant: 8).isActive = true
```

### Visual Format

```swift
let views: [String: Any] = [
	"blue": blueView,
	"red": redView
]

let format = "H:|-[blue]-8-[red]-|"

var constraint = NSLayoutConstraint.constraints(withVisualFormat: format,
																								options: [],
																								metrics: nil,
																								views: views)

view.addConstraints(constraint)
```

### 참고자료

- [https://velog.io/@wonhee010/Code로-AutoLayout을-줘보자](https://velog.io/@wonhee010/Code%EB%A1%9C-AutoLayout%EC%9D%84-%EC%A4%98%EB%B3%B4%EC%9E%90)
- [https://nsios.tistory.com/99](https://nsios.tistory.com/99)
- [https://www.raywenderlich.com/277-auto-layout-visual-format-language-tutorial](https://www.raywenderlich.com/277-auto-layout-visual-format-language-tutorial)

---

# Singleton 패턴을 활용하는 경우를 예를 들어 설명하시오.

- 프로젝트 전역에서 해당 인스턴스를 사용할 때
- 많이 또 자주 사용해야 하는 기능일 때,
- 그 기능이 다른 인스턴스에 의존하지 않고 독립적일 때 사용한다.

### UIKit에서 제공하는 싱글톤 객체들

```swift
let screen = UIScreen.main
let userDefault = UserDefaults.standard
let application = UIApplication.shared
let fileManager = FileManager.default
let notification = NotificationCenter.default
```

### 내가 프로젝트를 사용하면서 예시들

- Date formatting 을 할 때 앱에 날짜 관련 정책을 세우고 DateFormatter를 싱글톤으로 선언해서 사용
    - 추가적으로 DateFormatter를 생성하고 연산하는 작업이 무거운 작업이라서 인스턴스를 한번만 생성하는 것이 효율적 이었다.
- Json encoder나 decoder를 서버 api 기준에 맞게 커스텀하고 전역에서 사용
- 네트워크를 통신하는 클래스를 만들어서 전역에서 사용

### 추가적으로 안 내용

- Swift에서 싱글톤을 선언하기에는 쉽고 또 강력하다.
    - 다른 언어에서는 Thread Safe 하지 않아서 개발자가 처리해줘야 할 것이 많다.
    - Swift에서는 알아서 해준다.. (자세한 설명은 소들이 블로그..)

### 참고자료

- [https://babbab2.tistory.com/66](https://babbab2.tistory.com/66)

---

# NotificationCenter 동작 방식과 활용 방안에 대해 설명하시오.

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/17bb0c25-75dd-4e18-b733-c5a59016f380/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/17bb0c25-75dd-4e18-b733-c5a59016f380/Untitled.png)

### 참고자료

- [https://www.slideshare.net/Jintin1018/ios-notificationcenter-tutorial](https://www.slideshare.net/Jintin1018/ios-notificationcenter-tutorial)

 

---

# 앱이 foreground에 있을 때와 background에 있을 때 어떤 제약사항이 있나요?

### Foreground 시

- Foreground 진입시에 App active 상태를 차단하는 코드를 실행하지 마세요
- UI작업이 마무리 되었는지, 사용자에게 보여줄 데이터는 잘 업데이트 됐는지를 확인하세요.
- 앱이 사용자에게 보여졌을 때는 다른 뷰컨을 표시하거나 사용자 인터페이스를 크게 변경하지 마세요.

### Background 시

- 최대한 메모리를 적게 사용하고 왠만해서는 무엇을 하지 마세요.
- 예시들
    - 유저 데이터들을 저장하고 열린 파일이 있다면 닫으세요
    - dispatch, operation Queue를 중지 시키세요
    - 새 작업을 예약하지 마세요
    - 타이머를 중단하세요
- 앱이 공유하고 있는 자원들을 해제하세요.
- Foreground앱에 자원이 많이 할당 되기 떄문에 Background 상태에 있는 앱들은 최악의 경우에는 종료될 수 있어요. 이에 대해 대비하세요.

### Background에서 응답해야하는 중요한 이벤트들

일반적으로 App이 background 상태에 들어가면 실행할 시간이 주어지지 않지만 다음과 같은 예시는 예외의 경우이다.

- Airplay 혹은 pip 모드 사용시 오디오 통신
- 위치 감지 서비스를 사용하는 경우
- ip를 사용한 음성 통화(일단 전화 통화는 아닌듯.. 카톡 보이스톡 등)
- 외부 악세서리와 통신
- 블루투스 통신
- 서버에서 정기적으로 업데이트를 해야하는 경우
- APN 애플 푸시 알림 서비스 지원

### 참고자료

- [https://developer.apple.com/documentation/uikit/app_and_environment/scenes/preparing_your_ui_to_run_in_the_background](https://developer.apple.com/documentation/uikit/app_and_environment/scenes/preparing_your_ui_to_run_in_the_background)
