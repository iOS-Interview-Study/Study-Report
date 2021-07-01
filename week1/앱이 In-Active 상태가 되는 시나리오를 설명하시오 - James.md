#### App이 in-active 상태가 되는 시나리오를 알기 위해서는 먼저 앱의 생명주기를 먼저 알아야 합니다.



# Managing Your App's Life Cycle

---



## AppDelegate  vs  Scene Delegate

점선: 시스템의 action

실선: 사용자의 action

![Screen Shot 2021-06-24 at 12.22.25 PM](https://raw.githubusercontent.com/inwoodev/uploadedImages/uploadedFiles/20210624122230.png)

![Screen Shot 2021-06-24 at 12.26.50 PM](https://raw.githubusercontent.com/inwoodev/uploadedImages/uploadedFiles/20210624122655.png)



## App States

1. Not running

   - 앱이 완전히 종료되었거나 실행중이긴 하지만 시스템에 의해서 종료된 상태

2. In-active

   ![Screen Shot 2021-06-24 at 11.33.53 AM](https://raw.githubusercontent.com/inwoodev/uploadedImages/uploadedFiles/20210624113400.png)

   -  [공식문서](https://developer.apple.com/documentation/uikit/uiapplication/state/inactive)에 따르면 앱이 foreground에서 돌아가긴 하지만 어떠한 이벤트도 받지 않는 상태를 얘기합니다. (background로 전환시 거치는 단계)

3. Active

   - 앱이 실행 중이며 foreground에 있고 이벤트를 받고 있는 상태를 뜻합니다.

4. Background

   - 앱이 background에 있으면 다른 앱으로 전환되었거나 홈버튼이나 드래그를 동해 앱 밖으로 나갔을 때 상태를 얘기합니다. (Suspend되기 전 상태)

5. Suspended

   - 아무 코드도 실행되지 않는 상태이고 시스템에서 임의로 resource를 해제할 수 있는 상태입니다. 따로 호출되는 메서드가 없습니다. background 상태에서 특별한 작업이 없을 때 해당 상태가 됩니다.



## App 상태전환 flow

Deployment Target이 iOS 13 미만 app과 iOS13 이후 버전인 scene-based app 모두 appDelegate 프로토콜을 채택하는 app delegate object가 앱의 실행과 종료를 관리합니다. 앱 실행시 UIKit은 자동으로 UIApplication object와 app delegate object를 생성하여 아래에 명시된 메서드를 실행합니다.

```swift
class AppDelegate: UIResponder, UIApplicationDelegate {

	/* 필요한 객체를 생성하고 앱 실행 준비가 끝나기 직전에 호출된다
	(not running 상태)
	*/
  func application(_ application: UIApplication, willFinishLaunchingWithOptions launchOptions: [UIApplication.LaunchOptionsKey : Any]? = nil) -> Bool {
    print("앱 실행 준비 완료!")
    return true
  }
  
  /* 앱 실행 준비가 모두 끝난 후 화면이 사용자게에 보여지기 직전에 호출되는 메서드
  주로 초기화 코드를 이 곳에 작성한다. (not running 상태)
  */
  func application(_ application: UIApplication, didFinishLaunchingWithOptions launchOptions: [UIApplication.LaunchOptionsKey: Any]?) -> Bool {
        // Override point for customization after application launch.
        return true
    }
}
```



Deployment Target이 iOS 13미만인 경우 

앱이 appDelegate가 app의 window를 관리하며 앱이 시작된 후 종료하는 과정에서의 호출은 아래와 같이 이뤄집니다.

```swift
/*앱이 background 또는 not-running 상테에서 Foreground로 들어가기 직전에 호출됩니다.
in-active 상태를 거쳐 active 상태가 됩니다.
*/
func applicationWillEnterForeground(_ application: UIApplication) {
  print("Foreground로 진입할겁니다!!")
}

func applicationDidBecomeActive(_ application: UIApplication) {
  print("앱이 active상태입니다.")
} 

func applicationWillResignActive(_ application: UIApplication) {
  print("앱이 active 상태에서 inactive 상태로 전환시 직전에 실행됩니다.")
}

/*앱이 background 상태로 들어갔을 때 호출 됩니다.
suspended 상태가 되기 전 중요한 데이터를 저장하는 것과 같이 종료하기 전 필요한 작업을 합니다.
특별한 처리가 없으면 바로 suspended 상태로 전환됩니다.
*/
func applicationDidEnterBackground(_ application: UIApplication) {
  print("앱이 BackGround상태입니다..")
}

/*
메모리 확보를 위한 suspended 상태에 있는 앱이 종료될 때 그리고 background 상태에서 사용자에 의해 종료될 때 또는 오류로 인해 앱이 종료될 때는 호출 되지 않습니다.
*/
func applicationWillTerminate(_ application: UIApplication) {
  print("앱이 종료됩니다.")
}
```



![img](https://raw.githubusercontent.com/inwoodev/uploadedImages/uploadedFiles/20210624152156.png)

iOS 13이후 Scene-based app인 경우 Scene Delegate가 앱의 나머지 생명주기를 관리하게 됩니다. 이 과정에서 window의 개념이 scene 개념으로 대체 됩니다. 이제 앱에서 두 개 이상의 scene이 있을 수 있고 scene이 앱의 사용자 인터페이스 및 콘텐츠의 배경으로 사용됩니다.

Scene과 SceneSession 그리고 Scene Delegate에 대한 자세한 내용은 [해당](https://lena-chamna.netlify.app/post/appdelegate_and_scenedelegate/) 블로그를 통해 확인 해 주세요.

Scene Delegate는 scene을 관리하며 앱이 시작된 후 UILife Cycle의 시작과 종료하는 과정은 appDelegate와 굉장히 유사합니다.

애플리케이션이 최소 실행되고 실행된 직후 사용자의 화면에 보여지기 직전에 위와 동일하게 아래 메서드들이 호출된 후

```swift
	앱 실행 전 상태

class AppDelegate: UIResponder, UIApplicationDelegate {
  
  func application(_ application: UIApplication, willFinishLaunchingWithOptions launchOptions: [UIApplication.LaunchOptionsKey : Any]? = nil) -> Bool {
  
}

	func application(_ application: UIApplication, didFinishLaunchingWithOptions launchOptions: [UIApplicationLaunchOptionsKey: Any]?) -> Bool {
  
	}
}
```

Scene이 application에 추가된 후 기존 appDelegate의 앱 생명주기와 비슷한 맥락으로 생명주기가 만들어집니다. 다만 not running 상태가 아니라 unattached 상태부터 시작됩니다.

appDelegate는 아래의 메서드를 활용하여 Scene Session을 연결합니다.

``` swift
class AppDelegate: UIResponder, UIApplicationDelegate {

  // MARK: UISceneSession Lifecycle
  
	Unattached 상태

    func application(_ application: UIApplication, configurationForConnecting connectingSceneSession: UISceneSession, options: UIScene.ConnectionOptions) -> UISceneConfiguration {
      // AppDelegate에서 Scene Session을 연결하는 동작입니다.
      // 새로운 scene session이 생성될 때 호출 됩니다.
      // 새로운 scene의 환경설정을 하기 위해 해당 메서드를 사용합니다.
      // Scene Session이 생성될 때 AppDelegate에게 해당 사항을 알려주는 메서드입니다.
        return UISceneConfiguration(name: "Default Configuration", sessionRole: connectingSceneSession.role)
    }

}
```



``` swift
func scene(_ scene: UIScene, willConnectTo session: UISceneSession, options connectionOptions: UIScene.ConnectionOptions) {
  // Scene이 앱에 추가될 때 호출 됩니다.
  // ViewController와 같은 class 객체를 만들어 사용해야하는 시점입니다.
  // ViewDidLoad()가 호출되지 않습니다.
  
}

	unattached 상태
			|
			|
			V
	In-active 상태

func sceneWillEnterForeground(_ scene: UIScene) {
  // 앱이 background나 unattached 상태에서 foreground로 들어가기 직전에 호출됩니다.
  // 비활성화 상태를 거쳐 활성화 상태가 됩니다.
}
	In-active 상태
			| 
			|
			V
	Active 상태

func sceneDidBecomeActive(_ scene: UIScene) {
  // inactive 상태에서 active 상태로 전환된 상태입니다.
  // 앱이 실제로 사용되기 전 마지막으로 준비할 수 있는 코드를 작성할 수 있습니다.
  // app switcher를 활용하여 현재 실행중인 app들이 보이는 화면일 때 호출됩니다.
    }

func sceneWillResignActive(_ scene: UIScene) {
  // active 상태에서 inactive 상태로 전환될 때 호출되는 메서드입니다.
  // 앱 실행 중 갑자기 걸려온 전화와 같은 잠시 방해받는 과정에서 호출 됩니다.
  // 또한 다른 화면으로 전환될 때도 호출 됩니다.
    }

		In-active 상태
				|
				|
				V

		Background 상태

func sceneDidEnterBackground(_ scene: UIScene) {
  // Scene이 foreground에서 background로 전환되었을 때 호출 됩니다.
  // 해당 메서드를 활용하여 데이터를 저장하거나 리소스를 해제하세요
    }
```

scene의 종료는 아래 메서드들을 통해 이뤄집니다.

``` swift

		Background 상태
					|
					|
					V
		Suspended 상태
class SceneDelegate: UIResponder, UIWindowSceneDelegate {
   func sceneDidDisconnect(_ scene: UIScene) {
        // 시스템이 scene을 해체할 때 호출되는 메서드입니다.
        // 해당 메서드는 scene이 background상태로 들어가거나 아니면 session이 discard 될 때 호출됩니다.
        // 해당 scene과 관련된 모든 리소스를 해제합니다.
        // 해당 메서드를 호출한다고 해서 무조건적으로 scene이 discard 된 것이 아닙니다. 해당 scene은 추후에 다시 연결 될 수 있습니다.
    }
 }

		unattached 상태
```



```swift


class AppDelegate: UIResponder, UIApplicationDelegate {
	func application(_ application: UIApplication, didDiscardSceneSessions sceneSessions: Set<UISceneSession>) {
        // 사용자가 임의로 scene session을 discard 할 때 호출되는 메서드입니다.
        // 만약 session이 앱이 실행되지 않는 상황에서 discard 된다면 이 메서드는 application:didFinishLaunchingWithOptions 가 호출 된 잠시 후 호출됩니다.
        // 해당 메서드를 활용하여 discard된 scene의 리소스를 해제합니다..
    }
}

func applicationWillTerminate(_ application: UIApplication) {
  
}

앱이 종료된 상태
```




## 그럼 app in-active 상태가 되는 시나리오는 어떻게 되는가?

iOS 13 이후 버전 기준으로 앱이 실행되면 `application(_:willFinishLaunchingWithOptions) ` 그리고  `application(_:didFinishLaunchingWithOptions:)` 메서드가 호출 된 뒤  `sceneWillEnterForeground()`메서드가 호출 되어서 앱의 foreground를 진입하게 되고 scene이 in-active 상태를 거쳐서 active 상태로 됩니다.

또한 유저의 행동에 의해 background로 넘어가기 전 즉 `sceneWillResignActive(_: UIScene)` 메서드가 호출 되면 app이 active 상태에서 inactive 상태로 전환되어서 background 상태로 바뀌게 됩니다.



앱이 **background로** 넘어가는 경우는 아래와 같은 경우가 있습니다:

- 홈화면으로 갈 때

- 잠금 버튼을 눌러서 잠금화면 일 때

- 다른앱으로 이동해서 다른앱이 켜져있을 때

 

background로 넘어가지 않고 **Inactive상태만**을 유지하는 경우는 아래와 같은 경우가 있습니다:

- 멀티윈도우상태(최소화상태)

- 앱실행중 전화가 올 때

- 문자를 받은 뒤 해당 문자를 길게 클릭해서 확대해서 볼 때

- 앱 실행 중 왼쪽상단에서 스크롤해서 알림센터 볼 때

- 앱 실행 중 오른쪽상단에서 스크롤해서 제어센터 볼 때



[참고]:

[Article | Responding to the Launch of Your App](https://developer.apple.com/documentation/uikit/app_and_environment/responding_to_the_launch_of_your_app)

[[iOS 면접질문]앱 생명주기(App LifeCycle) (1) - 개념(앱이 In-Active 상태가 되는 시나리오를 설명하시오.)](https://fomaios.tistory.com/entry/앱-생명주기App-LifeCycle-1)

[SceneDelegate란](https://zetal.tistory.com/entry/swiftUI-튜토리얼2-SceneDelegate)

[[Swift\] App Life Cycle] - Nam의 iOS일기](https://nsios.tistory.com/60)