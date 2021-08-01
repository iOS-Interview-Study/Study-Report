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



------





### 앱의 콘텐츠나 데이터 자체를 저장/보관하는 특별한 객체를 무엇이라고 하는가?

<u>**Core Data (Framework)**</u>

디바이스에 영구적인 데이터 저장이 가능하며, 단일 디바이스에서 데이터를 유지, 캐싱, 실행 취소 하는 것을 지원한다.

CloudKit을 사용하여 여러 장치에 데이터를 동기화한다. 



오프라인에서의 사용을 위해 앱에 영구 데이터를 저장, 임시 데이터 캐싱, 단일 디바이스에서 앱의 실행 취소 기능을 추가한다. 

단일 iCloud 계정의 여러 기기에서 데이터를 동기화 하기 위해 CoreData는 스키마를 CloudKit 컨테이너에 자동으로 미러링한다. 



생성 방법은 데이터 모델 편집기를 통해 데이터의 유형과 관계를 정의하고, 각각의 클래스를 정의한다. 

간단한 정보 저장은 `UserDefaults`로 하지만, 크고 복잡한 데이터를 저장할 때 좋다. 



##### Persistence
<img width="274" alt="persistant" src="https://user-images.githubusercontent.com/29880961/127758842-a9851bcf-db41-4ff1-b8d5-e1bf4c1bfe52.png">

Core Data는 개체를 저장소에 매핑하는 세부 정보를 추상화하여 데이터베이스를 직접 관리하지 않고도 쉽게 저장할 수 있도록 한다. 



##### Undo and Redo of Individual or Batched Changes

<img width="538" alt="스크린샷 2021-08-01 오후 12 42 50" src="https://user-images.githubusercontent.com/29880961/127758847-ef5197a9-f2ec-4e97-b6e9-0f7214521347.png">


Core Data의 실행 취소 관리자는 변경 사항을 추적하고 개별 or 그룹 or 한 번에 모두 롤백할 수 있으므로 앱에 실행 취소 및 다시 실행 지원을 쉽게 추가할 수 있다. 



##### Background Data Tasks

<img width="312" alt="스크린샷 2021-08-01 오후 12 44 10" src="https://user-images.githubusercontent.com/29880961/127758850-c52801e0-6333-4d68-9bbc-0127a9d83b64.png">


백그라운드에서 JSON을 개체로 구문 분석하는 것과 같은 잠재적인 UI 차단 데이터 작업을 수행한다. 그 후 결과를 캐시하거나 저장하여 네트워크 통신을 줄일 수 있다. 



##### View Synchronization

Core Data는 테이블 뷰 및 컬렉션 뷰에 대한 데이터 원본을 제공하여 뷰와 데이터를 동기화 된 상태로 유지하는 데 도움이 된다. 



##### Versioning and Migration

Core Data는 데이터 모델의 버전을 관리하고, 앱을 개발할 때마다 사용자의 데이터를 마이그레이션하는 메커니즘이 포함되어있다. 



------



### Delegate 패턴을 활용하는 경우를 예를 들어 설명하시오.

기능을 처리할 객체를 delegate로 설정하고, 특정 이벤트가 발생할 때 이를 delegate에 의해 위임되어져있는 본래의 객체로 전달해주는 것이다. 프로토콜을 사용하기 때문에 재사용할 수 있는 코드 작성이 가능하다.

`TableView`의 delegate 를 채택하면 어떤 이벤트에 대한 코드를 부여하면 동작하도록 하는 프로토콜이다. 
기본 값이 Optional 으로 되어있기 때문에 기본적으로 동작이 되지만, 필요한 경우 함수의 내부를 채워넣거나 적절한 값을 return 해주면 작동한다.

<img width="927" alt="스크린샷 2021-08-01 오후 6 06 57" src="https://user-images.githubusercontent.com/29880961/127765889-1db2f3c0-bb58-4b8a-ae58-afaaf9e886cd.png">


------


### Swift Standard Library의 map, filter, reduce, compactMap, flatMap에 대하여 설명하시오.

고차함수 : 함수를 param으로 받아 함수를 리턴하는 함수. 함수를 변수에 대입 가능한 것이 특징.

#### Map (맵핑)

```swift
let numbers = [1, 2, 3, 4]

let mapTarget = numbers.map { number in
    return number + 1
}

print(mapTarget) // [2, 3, 4, 5]
```



#### filter (추출)

컨테이너 내부의 값을 걸러 새로운 컨테이너 값으로 추출한다.

`map`과 차이가 있다면, `map`은 기존의 data를 변형하여 새로운 컨테이너에 담아 반환하지만, `filter`는 기존의 data를 그대로 가져서 변환을 한다. 

```swift
let numbers = [1, 2, 3, 4]

let filterTarget = numbers.filter { number in
    return number % 2 == 0
}

print(filterTarget) // [2, 4]

```



#### reduce (결합, 차원을 줄여주는 것)

`reduce`는 사용할 때 초기값을 주어야 함. 컨테이너 내부의 콘텐츠를 하나로 통합한다. 

```swift
let numbers = [0, 1, 2, 3, 4]
let reduceTarget = numbers.reduce(0, { (first: Int, second: Int) -> Int in
    return first + second
})
print(reduceTarget) //10
```



#### compactMap

기존 `map`과는 달리 `nil`이 제거된 결과를 확인할 수 있다. 

```
let numbers: [Int?] = [0, 1, 2, nil, 4]
let compactMapTarget = numbers.compactMap { number in
    return number
}
print(compactMapTarget) // [0, 1, 2, 4]
```



#### flatMap

기존 `map`, `compactMap`보다 더 추가된 기능인 

다차원 배열을 1차원 배열로 평평하게 만들어 주고, nil 제거, 옵셔널 바인딩 까지의 역할을 수행한다. 

```swift
// for compactMap
let numbers: [[Int?]] = [[0, 1, 2, nil, 4], [5, 6, 7, nil, 8]]

var compactMapTarget: [[Int]] = []
for number in numbers {
    compactMapTarget.append(number.compactMap{ n in
        return n
    })
}
print(compactMapTarget) // [[0, 1, 2, 4], [5, 6, 7, 8]]

// for flatMap
let numbers: [[Int?]] = [[0, 1, 2, nil, 4], [5, 6, 7, nil, 8]]
let flatMapTarget = numbers.flatMap{ $0 }.compactMap{ $0 }
print(flatMapTarget) // [0, 1, 2, 4, 5, 6, 7, 8]

```

