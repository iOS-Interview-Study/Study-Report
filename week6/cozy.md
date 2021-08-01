### Scene delegate에 대해 설명하시오.

**iOS 12** 까지는 대부분 하나의 앱에 하나의 window 였지만,

iOS 13 부터는 window의 개념이 `scene `으로 대체되어 <u>하나의 앱에 여러개의 `scene`을 가질 수 있</u><u>게 됨</u>.



**AppDelegate**에서 하고 있던 UI의 상태를 알 수 있는 UILifeCycle의 대부분을 **SceneDelegate**가 수행하며, 대신 AppDelegate에선  `Scene Session`을 통해 scene에 대한 모든 정보를 업데이트 받고, 관리하는 역할이 추가되었다. 

​	

**Scene Session**

고유의 런타임 인스턴스를 관리. 사용자가 앱에 scene을 추가하거나, 요청하면 시스템에서 scene에 대항하는 session 객체를 생성한다.  session의 정보로는 식별 id, scene의 구성 세부사항이 들어있다. 

UIKit에서는 session의 정보를 scene life time 동안 유지하고, app switcher에서 사용자가 해당 scene을 클로징하는 것에 대응하여 session을 파괴한다.

Session 객체는 직접 생성하는게 아닌, UIKit가 앱의 사용자 인터페이스에 대응하여 생성한다. 



 **SceneDelegate 함수** 

```swift
func scene(_ scene: UIScene, willConnectTo session: UISceneSession, options connectionOptions: UIScene.ConnectionOptions) {
        guard let _ = (scene as? UIWindowScene) else { return }
    }
```

scene이 앱에 추가되어질 때 호출



```swift
 func sceneDidDisconnect(_ scene: UIScene) {
	     // Called as the scene is being released by the system.
        // This occurs shortly after the scene enters the background, or when its session is discarded.
        // Release any resources associated with this scene that can be re-created the next time the scene connects.
        // The scene may re-connect later, as its session was not necessarily discarded (see `application:didDiscardSceneSessions` instead).
    }
```

scene의 연결이 해제될 때 호출



```swift
 func sceneDidBecomeActive(_ scene: UIScene) {
        // Called when the scene has moved from an inactive state to an active state.
        // Use this method to restart any tasks that were paused (or not yet started) when the scene was inactive.
    }
```

`app switcher`에서 선택되는 것과 같이 scene과의 상호작용이 시작될 때 호출



```swift
func sceneWillResignActive(_ scene: UIScene) {
        // Called when the scene will move from an active state to an inactive state.
        // This may occur due to temporary interruptions (ex. an incoming phone call).
    }
```

사용자가 scene과의 상호작용을 중지할 때 호출 (다른 scene으로의 전환)



```swift
func sceneWillEnterForeground(_ scene: UIScene) {
        // Called as the scene transitions from the background to the foreground.
        // Use this method to undo the changes made on entering the background.
    }
```

scene이 포그라운드로 진입할 때 호출

foreground 상태의 앱: 사용자가 보고 있는 화면으로, CPU를 비롯한 시스템 자원의 우선순위가 높을 때이다. 



```swift
 func sceneDidEnterBackground(_ scene: UIScene) {
        // Called as the scene transitions from the foreground to the background.
        // Use this method to save data, release shared resources, and store enough scene-specific state information
        // to restore the scene back to its current state.
    }
```

scene이 백그라운드로 진입할 때 호출

background 상태의 앱: 홈 화면에 들어가서 사용자에게 보이지 않는 상태. 		

(음악 앱을 사용하더라도 노래가 멈추지 않게 하기 위한 작동이나, 타이머 앱을 실행 후 다른 작업을 해야할 때)



 **AppDelegate 함수** 

> 1. 앱의 중요 데이터 구조를 초기화 시켜줌
> 2. 앱의 scene의 환경설정 
> 3. 앱 밖에서 발생한 알림(배터리 부족, 다운로드 알 등)에 대응
> 4. 앱 자체의 타겟 이벤트에 대응
> 5. 애플 push 알림과 같이 실행시 요구되는 모든 서비스 등록 



```swift
    // MARK: UISceneSession Lifecycle
    func application(_ application: UIApplication, configurationForConnecting connectingSceneSession: UISceneSession, options: UIScene.ConnectionOptions) -> UISceneConfiguration {
        // Called when a new scene session is being created.
        // Use this method to select a configuration to create the new scene with.
        return UISceneConfiguration(name: "Default Configuration", sessionRole: connectingSceneSession.role)
    }
```

scene을 만들 때 구성 객체를 반환시켜준다.



```swift
    func application(_ application: UIApplication, didDiscardSceneSessions sceneSessions: Set<UISceneSession>) {
        // Called when the user discards a scene session.
        // If any sessions were discarded while the application was not running, this will be called shortly after application:didFinishLaunchingWithOptions.
        // Use this method to release any resources that were specific to the discarded scenes, as they will not return.
    }
```

사용자가 app switcher를 통해 scene 을 닫을 때, 호출





### 앱의 콘텐츠나 데이터 자체를 저장/보관하는 특별한 객체를 무엇이라고 하는가?

<u>**Core Data (Framework)**</u>

디바이스에 영구적인 데이터 저장이 가능하며, 단일 디바이스에서 데이터를 유지, 캐싱, 실행 취소 하는 것을 지원한다.

CloudKit을 사용하여 여러 장치에 데이터를 동기화한다. 



오프라인에서의 사용을 위해 앱에 영구 데이터를 저장, 임시 데이터 캐싱, 단일 디바이스에서 앱의 실행 취소 기능을 추가한다. 

단일 iCloud 계정의 여러 기기에서 데이터를 동기화 하기 위해 CoreData는 스키마를 CloudKit 컨테이너에 자동으로 미러링한다. 



생성 방법은 데이터 모델 편집기를 통해 데이터의 유형과 관계를 정의하고, 각각의 클래스를 정의한다. 

간단한 정보 저장은 `UserDefaults`로 하지만, 크고 복잡한 데이터를 저장할 때 좋다. 



##### Persistence

Core Data는 개체를 저장소에 매핑하는 세부 정보를 추상화하여 데이터베이스를 직접 관리하지 않고도 쉽게 저장할 수 있도록 한다. 

![스크린샷 2021-08-01 오후 12.40.45](/Users/jieunkim/Desktop/persistant.png)

##### Undo and Redo of Individual or Batched Changes

![스크린샷 2021-08-01 오후 12.42.20](/Users/jieunkim/Library/Application Support/typora-user-images/스크린샷 2021-08-01 오후 12.42.20.png)

Core Data의 실행 취소 관리자는 변경 사항을 추적하고 개별 or 그룹 or 한 번에 모두 롤백할 수 있으므로 앱에 실행 취소 및 다시 실행 지원을 쉽게 추가할 수 있다. 



##### Background Data Tasks

<img src="/Users/jieunkim/Library/Application Support/typora-user-images/스크린샷 2021-08-01 오후 12.44.00.png" alt="스크린샷 2021-08-01 오후 12.44.00" style="zoom:50%;" />

백그라운드에서 JSON을 개체로 구문 분석하는 것과 같은 잠재적인 UI 차단 데이터 작업을 수행한다. 그 후 결과를 캐시하거나 저장하여 네트워크 통신을 줄일 수 있다. 



##### View Synchronization

Core Data는 테이블 뷰 및 컬렉션 뷰에 대한 데이터 원본을 제공하여 뷰와 데이터를 동기화 된 상태로 유지하는 데 도움이 된다. 



##### Versioning and Migration

Core Data는 데이터 모델의 버전을 관리하고, 앱을 개발할 때마다 사용자의 데이터를 마이그레이션하는 메커니즘이 포함되어있다. 



### Delegate 패턴을 활용하는 경우를 예를 들어 설명하시오.

기능을 처리할 객체를 delegate로 설정하고, 특정 이벤트가 발생할 때 이를 delegate에 의해 위임되어져있는 본래의 객체로 전달해주는 것이다. 프로토콜을 사용하기 때문에 재사용할 수 있는 코드 작성이 가능하다.











#### Swift Standard Library의 map, filter, reduce, compactMap, flatMap에 대하여 설명하시오.