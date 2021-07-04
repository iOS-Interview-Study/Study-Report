## UIWindow 객체의 역할은 무엇인가?
앱의 시각적 콘텐츠를 담는다.
뷰들과 다른 어플리케이션 객체들에게 터치 이벤트를 전달하는 중요한 역할을 한다.
오리엔테이션 변화를 쉽게 하기 위해 앱의 뷰 컨트롤러들과 협력한다.
window 객체는 뷰들을 담는 컨테이너라고 생각하면 된다. 스크린에 표시되는 뷰의 계층 구조에서 최상위 뷰의 역할을 할 고정적인 객체의 역할을 하는 것이다. 그래서 앱의 생명 주기 동안 특별한 경우(밑에서 설명)를 제외하면 단 하나의 window 객체만 생성하고 사용하며 표시한 뷰가 바뀌어야 되는 경우 가장 앞단에 있는 rootViewController의 교체를 통해 뷰를 바꾸는 방법을 사용한다. 그래서 보통 Xcode는 프로젝트가 생성될 경우 자동으로 AppDelegate에 window 객체를 생성을 하여 개발자가 생성 또는 상속받는 일은 잘 없다.

특별한 경우
다음은 window 객체를 새로 만들거나 만들어지는 경우이다.

스토리보드를 쓰지 않을 경우에는 메인 window를 직접 생성해야 한다.
외부 디스플레이를 지원하는 앱의 경우 새로운 Window를 만들어 보여줄 수 있다.
보통의 경우 새로운 window는 시스템에 의해 만들어지며 특정 이벤트에 대한 반응으로 만들어진다.(예: 전화가 오는 경우)

- 메인 Window 객체를 만드는 방법
메인 window 객체는 프로그래밍 / 인터페이스 빌더 두 가지 방법으로 생성과 설정을 할 수 있고 AppDelegate 객체가 이것을 잡고 있어야 한다. 그리고 이 객체는 백그라운드나 포그라운드에 상관없이 앱이 실행될 때는 항상 생성해야 한다. 그러나 추가적인 window 객체는 그것이 필요할 때 생성되도록 해야 한다(lazily created).

코드로 window 객체를 생성하고 뷰 컨트롤러에 연결하는 경우

Main으로 되어 있었던 Info.plist에 항목을 지워준다. 프로젝트를 생성할 경우 Xcode는 자동으로 Main.storyboard를 통해 메인 window 객체를 만들고 rootViewController를 결정한다. 코드로 만드는 경우에는 지우면 된다.

```swift
@UIApplicationMain
class AppDelegate: UIResponder, UIApplicationDelegate {

    var window: UIWindow?
    ...
}
```

그 다음 AppDelegate로 가보면 AppDelegate 객체가 strong으로 잡고 있는 옵셔널한 window 객체가 있다. 자동으로 storyboard가 연결이 되어 있는 경우라면 스토리보드와 연결되어 window 객체가 생성되어 AppDelegate 객체가 잡고 있는 이 window에 넣어진다. 그러나 스토리보드를 사용하지 않는 경우기 때문에 직접 넣어 줄 것이다. 이 작업은 UIApplicationDelegate 메소드인 didFinishLaunchingWithOptions에서 해줄 것이다.

```swift
func application(_ application: UIApplication, didFinishLaunchingWithOptions launchOptions: [UIApplicationLaunchOptionsKey: Any]?) -> Bool {
        // Override point for customization after application launch.
        window = UIWindow(frame: UIScreen.main.bounds)
        ...

        return true
}
```

위에서 언급했듯이 window 객체는 그저 뷰를 보여주는 컨테이너 역할이기 때문에 `rootViewController`를 무조건 가져야 한다. 그래서…

```swift
func application(_ application: UIApplication, didFinishLaunchingWithOptions launchOptions: [UIApplicationLaunchOptionsKey: Any]?) -> Bool {
        // Override point for customization after application launch.
        guard let window = window else { return true }
        window.backgroundColor = UIColor.cyan

        let myViewController: UIViewController = UIViewController()
        window.rootViewController = myViewController
        myViewController.view.backgroundColor = UIColor.yellow

        let myLabel: UILabel = {
            let label: UILabel = UILabel()
            label.text = "프로그래밍을 이용한 경우"
            label.frame.size = label.intrinsicContentSize

            return label
        }()
        myViewController.view.addSubview(myLabel)
        myLabel.center = myViewController.view.center
        ...

        return true
}
```

UIViewController를 생성해 윈도우의 rootViewController를 지정해준 뒤 실행해보면 앱이 죽지는 않지만 역시나 검은 화면만 나오는 것을 볼 수 있다. 우리가 지정한 window가 보이지 않는다는 의미인데 이것은 우리의 window가 keyWindow가 아니라는 의미이다.(.isKeyWindow로 확인 가능하다.)

여기서 keyWindow란, ‘키보드 이벤트와 터치와 관련되지 않는 이벤트를 받는 window’를 말한다. 터치 이벤트는 터치가 일어난 바로 그 윈도우에게 전달되지만 좌표값이 없는 이벤트는 앱의 keyWindow에 전달이 된다. (하지만 대부분의 경우 메인 윈도우가 keyWindow일 것이다.)

```swift
window.makeKeyAndVisible()
```

우리의 window 객체가 keyWindow가 되고 보이게 하기 위해 이것을 실행해주면 노란화면에 UILabel 하나가 나오는 것을 볼 수 있다.(window의 파란색은 당연히 나오지 않는다.) 만약 위에서 AppDelegate가 strong으로 잡지 않는다면 어떻게 될까? 역시나 검은 화면이 나올 뿐이다.(실험을 통해서 확인해보기 바란다.)

스토리보드는 때에 따라서 버그도 많고 뷰가 많아질 경우 매우 느려지는 경우를 볼 수 있는데 이렇게 처음부터 코드로 하는 방법은 후에 가서도 쓰일 수 있을 것이다.

## 참고자료
- [UIWindow 공부](https://wnstkdyu.github.io/2017/12/29/uiwindow/) - PACALOG blog