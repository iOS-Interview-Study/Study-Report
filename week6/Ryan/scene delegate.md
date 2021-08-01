# scene delegate에 대해 설명하시오.
## Scene
> 앱 UI의 여러 인스턴스를 동시에 관리하고 적절한 UI 인스턴스로 리소스를 지정합니다.

Scene은 UI 인스턴스 하나를 표시하는 창과 뷰 컨트롤러가 포함되어 있습니다. 각 Scene은 UIKit와 앱 간의 상호 작용을 조정하는데 사용하는 `UIWindowSceneDelegate` 객체가 있습니다. Scene은 서로 동시에 실행되어 동일한 메모리 및 앱 프로세스 공간을 공유합니다. 따라서 하나의 앱에 여러 Scene과 SceneDelegate 객체가 동시에 공존할 수 있습니다. 
`UIApplicationDelegate` 객체가 새로운 Scene의 구성을 관리합니다.
![](https://images.velog.io/images/ryan-son/post/aa714381-10e0-424f-ac7b-99e2f679ae4f/image.png)

## UISceneDelegate
> Scene 내에서 발생하는 수명 주기 이벤트에 대응하는데 사용하는 핵심 메서드를 요구하는 타입

`UISceneDelegate` 객체를 사용하여 앱 사용자 인터페이스의 한 인스턴스에서 수명 주기 이벤트를 관리합니다. 이 인터페이스는 scene이 foreground로 들어가고 활성화되는 시기, 백그라운드로 들어가는 때를 포함하여 scene에 영향을 미치는 상태 전환에 응답하는 방법을 정의합니다. Delegate 메서드를 사용하면 이러한 전환이 발생할 때 적절한 동작을 제공할 수 있습니다.

### Methods
#### Scene transitioning
![](https://images.velog.io/images/ryan-son/post/a2506f62-f898-4292-86ba-07dbd5c49c61/image.png)

#### Saving and restoring App's state

![](https://images.velog.io/images/ryan-son/post/5478c451-b235-4ff9-8be3-a4dab4dd27e3/image.png)
# 참고자료
- [Scenes](https://developer.apple.com/documentation/uikit/app_and_environment/scenes) - Apple Developer
- [UISceneDelegate](https://developer.apple.com/documentation/uikit/uiscenedelegate/) - Apple Developer