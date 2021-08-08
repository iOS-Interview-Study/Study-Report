# Scene Delegate에 대해 설명하시오

---

Xcode 프로젝트를 생성한 뒤 프로젝트 파일 중 `SceneDelegate.swift` 와 `AppDelegate.swift` 파일들을 확인할 수 잇습니다. 더 나아가서 `Info.plist` 파일을 살펴보면 아래와 같이 `Application Scene Manifest`라는 key를 확인할 수 있습니다.

![Screen Shot 2021-07-31 at 6.57.35 PM](https://raw.githubusercontent.com/inwoodev/uploadedImages/uploadedFiles/20210731185740.png)

Application Scene Manifest를 자세히 살펴보면 Scene의 설정과 멀티 윈도우 설정을 할 수 있는 것을 볼 수 있습니다.



iOS 13 이전에는 scene이라는 개념이 없었고 app이 실행되고 app의 lifecycle를 appDelegate가 모두 관리하였습니다.

![Screen Shot 2021-07-31 at 6.39.29 PM](https://raw.githubusercontent.com/inwoodev/uploadedImages/uploadedFiles/20210731183948.png)

![Screen Shot 2021-07-31 at 6.39.45 PM](https://raw.githubusercontent.com/inwoodev/uploadedImages/uploadedFiles/20210731183958.png)

iOS 13 이후 앱의 Process LifeCycle과 UILifeCycle이 분리가 되었고 appDelegate가 Process LifeCycle을 관리하고 UILifeCycle에 대한 부분을 SceneDelegate가 담당하게 되었습니다.



구체적으로 scene이라는 개념이 app의 window를 대체하게 되었습니다. 이제 `UISceneSession` 객체가 관리하는 Scene이 유저가 보는 화면을 뜻하게 됩니다.

이제 애플리케이션 인터페이스와 앱 컨텐츠를 관리하는 scene을 하나 이상 갖을 수 있게 되었습니다. 그렇기 때문에 SceneDelegate가 화면에 디스플레이되는 UI와 data를 책임지게 되었습니다.



## SceneDelegate Methods

``` swift
1. scene(_:willConnectTo:options:)
2. sceneDidDisconnect(_:)
3. sceneDidBecomeActive(_:)
4. sceneWillResignActive(_:)
5. sceneWillEnterForeground(_:)
6. sceneDidEnterBackground(_:)
```



#### `scene(_:willConnectTo:options:)`

UISceneSession의 생명주기에서 첫 번째로 호출되는 메서드 입니다. 해당 메서드는 새로운 `UIWindow` 객체를 생성하고, root view controller를 세팅하고 생성된 window를 key window로 설정합니다. 더불어 해당 메서드를 통해서 scene의 UI를 복구할 수도 있습니다. 



#### `sceneWillEnterForeground(_:)`

해당 메서드는 앱이 background에서 foreground로 진입할 때 호출되는 메서드 입니다.



#### `sceneDidBecomeActive(_:)`

해당 메서드는 scen이 active한 상태가 되었을 때 호출됩니다. 이제 scene이 화면에 보여지고 사용준비가 된  것 입니다.



#### `sceneWillResignActive(_:)`

scene이 in-active한 상태로 들어갈 때 호출 됩니다.



#### `sceneDidEnterBackground(_:)`

scene이 background 상태가 된 후 호출됩니다.



#### `sceneDidDisconnect(_:)`

scene이 background로 보내질 때 iOS는 scene을 아예 제거하여서 사용되었던 리소스를 해제할 수도 있습니다. 그렇다고 해서 앱 자체가 죽거나 not running 상태가 되는 것은 아닙니다. 그저 scene의 연결이 해제되는 것을 뜻합니다. 사용자가 만약 특정한 scene를 다시 호출하게되면 iOS는 연결이 해제된 scene을 다시 연결할 수도 있습니다.



위 내용을 종합 해 보자면...

SceneDelegate가 추가된 가장 주된 이유는 iOS13 이후 멀티 윈도우 애플리케이션을 지원하기 위해 추가가 되었다고 볼 수 있습니다. iOS 13 이후 AppDelegate가 애플리케이션 단계의 이벤트 처리를 담당하게 되었고 SceneDelegate는 사용자에게 보여질 scene의 생명주기(scene의 생성, scene의 제거, 그리고 scene의 SceneSession의 상태 보존 등등)를 관리하게 되었습니다.



[참고]:

[Understanding Scene Delegate & App Delegate - KayanKumar P](https://medium.com/@kalyan.parise/understanding-scene-delegate-app-delegate-7503d48c5445)

[Understanding the iOS 13 Scene Delegate](https://www.donnywals.com/understanding-the-ios-13-scene-delegate/)