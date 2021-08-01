# Week6 - Fezz

## scene delegate에 대해 설명하시오.

### SceneDelegate

#### Scene?

> UIKit는 UIWindowScene 객체를 사용하는 앱 UI의 각 인스턴스를 관리합니다. **Scene에는 UI의 하나의 인스턴스를 나타내는 windows와 view controllers가 들어있습니다.** 또한 각 **scene에 해당하는 UIWindowSceneDelegate 객체**를 가지고 있고, 이 객체는 **UIKit과 앱 간의 상호 작용을 조정**하는 데 사용합니다. Scene들은 같은 메모리와 앱 프로세스 공간을 공유하면서 서로 동시에 실행됩니다. **결과적으로 하나의 앱은 여러 scene과 scene delegate 객체를 동시에 활성화**할 수 있습니다.
> (Scenes - Apple Developer Document 참고)

UI의 상태를 알 수 있는 UILifeCycle에 대한 역할을 `SceneDelegate`가 하게 됐고, 역할이 분리된 대신 `AppDelegate`에서 Scene Session을 통해서 scene에 대한 정보를 업데이트 받는다. 

#### Scene Session?

> **UISceneSession 객체**는 scene의 고유의 런타임 인스턴스를 관리합니다. 사용자가 앱에 새로운 scene을 추가하거나 프로그래밍적으로 scene을 요청하면, 시스탬은 그 scene을 추적하는 session 객체를 생성합니다. **그 session에는 고유한 식별자와 scene의 구성 세부사항(configuration details)이 들어있습니다.** UIKit는 session 정보를 그 scene 자체의 생애(life time)동안 유지하고 app switcher에서 사용자가 그 scene을 클로즈하는 것에 대응하여 그 session을 제거합니다. session 객체는 직접 생성하지않고 UIKit이 앱의 사용자 인터페이스에 대응하여 생성합니다. 
> (UISceneSession - Apple Developer Document 참고)

