---
layout: post
title: "UIApplicationDelegate 메서드"
image:
categories: iOS
tags: 
  - AppDelegate
  - UIApplicationDelegate
sitemap:
  changefreq: daily
  priority: 1.0
---

## AppDelegate이란?

`UIApplicationDelegate`의 subclass로 기본 생성되는 AppDelegate는 앱 전반적으로 공유하게 될 자원, 동작을 관리하는 역할을 수행한다. `UIApplication`의 Delegate이라는 이름에 맞게 앱의 기능들 중 일부를 대신하여 처리해준다.



### App Delegate 객체가 담당하는 일들

- 앱의 데이터 구조를 초기화한다.
- 앱의 scene을 configure한다.
- 앱 외부로부터 들어오는 notification(배터리 부족 notification 등..)들을 관리한다.
- 앱 자신의 이벤트에 답한다. (scene의 이벤트뿐만 아닌 view, view controller를 포함)
- launch time에 실행, 등록되어야 할 업무를 처리한다. (Apple Push Notification 등)



## AppDelegate의 메서드들

자주 사용하는 App launching, Scene, App LifeCycle 메서드는 링크를 달아놓았고, 그 외의 메서드는 공식문서에서 직접 확인하는 것이 좋을 것 같아 설명만 적어놓았다. 

### App 시작시점과 연관된 메서드

- [application(_:willFinishLaunchingWithOptions:)](https://developer.apple.com/documentation/uikit/uiapplicationdelegate/1623032-application)
- [application(_:didFinishLaunchingWithOptions:)](https://developer.apple.com/documentation/uikit/uiapplicationdelegate/1622921-application)

[UIApplication.LaunchOptionsKey](https://developer.apple.com/documentation/uikit/uiapplication/launchoptionskey)를 이용하여 앱의 시작시점에 launching option을 확인하는 메서드들이다.

launch 종료시에 [didFinishLaunchingNotification](https://developer.apple.com/documentation/uikit/uiapplication/1622971-didfinishlaunchingnotification)이 발송되며 이를 통해 launch 종료 여부를 확인할 수 있다.



### Scene관련 메서드

- [application(_:configurationForConnecting:options:)](https://developer.apple.com/documentation/uikit/uiapplicationdelegate/3197905-application)
- [application(_:didDiscardSceneSessions:)](https://developer.apple.com/documentation/uikit/uiapplicationdelegate/3197906-application)

scene이 생성될 때, 사라질 때 불리는 메서드들이다. scene 생성 이전에, 소멸 이후에 해줘야하는 작업들을 처리해줄 수 있다.



### 환경변화에 따른 응답 메서드

파일의 사용가능 여부에 변화가 일어나는 경우, 메모리가 부족한 경우에 notification을 받게되고 특정 메서드가 실행된다. 위의 상황들에 대처할 수 있는 코드를 적을 수 있는 메서드들이다.



### 앱의 상태를 보전/복구하는 메서드

앱의 상태를 보전해야할 때, 복구해야할 때 사용하는 메서드들이다. 



### Background Data Download 관련 메서드

백그라운드 상태에서 다운로드를 진행할 때, 다운로드 진행중의 행동, 종료시 completionHandler 등을 정의할 수 있다.ㅏ



### Remote Notification 등록 관련 메서드

외부에서 들어오는 Notification을 담당하는 메서드들이다. Notification을 받은 경우, 등록에 성공한 경우, 등록에 실패한 경우에 따라 다양한 처리를 해줄 수 있다.

### 사용자 액티비티, 퀵 액션 관련 메서드

사용자의 액티비티, 퀵 액션과 관련하여 여러 처리를 해줄 수 있는 메서드이다. 사용자 액티비티는 [https://developer.apple.com/documentation/foundation/nsuseractivity](https://developer.apple.com/documentation/foundation/nsuseractivity) 에서 확인할 수 있다.



### Watch, Health, Siri, Cloud Kit 관련 메서드

다양한 Kit들과 연관하여 앱을 사용할 수 있다. 



### URL Resource 관련 메서드

URL Resource를 열도록 지시하는 시점에 추가 작업을 해줄 수 있다.



### Geometry 관련 메서드

위치정보 등과 관련한 앱이 사용할 수 있는 메서드들이다.



### Entry Point (스토리보드용)

storyboard를 통해 진입할 때 사용하는 main 함수이다.



## App Life Cycle 관련 메서드

### Not Running

말 그대로 앱을 실행하지 않은 상태이다. 이 시점에 호출되는 메서드는 다음과 같다.

[application(_:willFinishLaunchingWithOptions:)](https://developer.apple.com/documentation/uikit/uiapplicationdelegate/1623032-application)

앱 실행을 준비하는 메서드로 필요한 주요 객체들을 생성, 앱 실행 준비가 끝나기 직전에 호출된다.

[application(_:didFinishLaunchingWithOptions:)](https://developer.apple.com/documentation/uikit/uiapplicationdelegate/1622921-application)

앱 실행을 위한 모든 준비가 끝난 후 화면이 사용자에게 보여지기 직전에 호출되는 메서드로, 초기화 코드를 이곳에 작성한다. 

이 메서드의 return value가 반환된 이후, 다른 app delegate의 메서드가 실행되어 forground state로 진입한다. 

launch option을 사용할 수 있는 마지막 장소이다.

애플에서는 다음과 같은 당부를 전달한다. (didFinish.. 메서드 대신  willFinish.. 메서드를 이용할 것을 권장한다는 내용)

> Important
>
> For app initialization, it is highly recommended that you use this method and the `application(_:willFinishLaunchingWithOptions:)` method and do not use the `applicationDidFinishLaunching(_:)` method, which is intended only for apps that run on older versions of iOS.

[applicationWillTerminate(_:)](https://developer.apple.com/documentation/uikit/uiapplicationdelegate/1623111-applicationwillterminate) 

앱이 종료되기 직전에 호출되는 메서드로 app의 task를 clean-up하거나 공유 자원의 free, user data의 저장, timer의 초기화 등의 작업을 위한 코드를 이곳에 작성한다.

다만 메모리 확보를 위해 suspended 상태에 있는 앱이 종료되는 경우, background 상태에서 사용자에 의해 종료되는 경우, 오류로 인해 앱이 종료되는 경우에 이 메서드는 호출되지 않는다.



### InActive

앱이 실행되면서 foreground에 진입하거나 foreground에서 background로 이동하는 시점. 어떠한 이벤트도 받지 않는 상태이며 다음과 같은 메서드들이 호출된다.

[sceneWillEnterForeground(_:)](https://developer.apple.com/documentation/uikit/uiscenedelegate/3197918-scenewillenterforeground) or  [applicationWillEnterForeground(_:)](https://developer.apple.com/documentation/uikit/uiapplicationdelegate/1623076-applicationwillenterforeground)

앱이 실행되며 foreground에 진입하기 직전 호출되는 메서드이다.

전자의 메서드가 호출되는 경우는 UISceneDelegate이 구현되어있는 경우이다. 만약 전자의 메서드가 호출되었다면 후자는 호출되지 않는다.

[sceneWillResignActive(_:)](https://developer.apple.com/documentation/uikit/uiscenedelegate/3197919-scenewillresignactive) or  [applicationWillResignActive(_:)](https://developer.apple.com/documentation/uikit/uiapplicationdelegate/1622950-applicationwillresignactive)

앱이 background로 진입하기 직전 호출되는 메서드이다. 위의 경우와 마찬가지로 UISceneDelegate의 구현여부에 따라 호출될 메서드가 결정된다.

홈화면으로 이동하는 경우, 화면이 앱 실행 도중 잠기는 경우, 다른 앱으로 이동한 경우 등의 상황에서 앱은 background로 진입하게 되며

멀티윈도우 상태, 알림센터나 제어센터를 보는 경우 등의 상황에서는 InActive 상태를 유지한채로 있는다.




### Active

앱이 실행중이며 foreground에 있고 이벤트를 받고 있는 상태

[sceneDidBecomeActive(_:)](https://developer.apple.com/documentation/uikit/uiscenedelegate/3197915-scenedidbecomeactive) or [applicationDidBecomeActive(_:)](https://developer.apple.com/documentation/uikit/uiapplicationdelegate/1622956-applicationdidbecomeactive)

위의 메서드에서 content를 refresh하거나 frame rate를 올리는 등의 작업을 실행해줄 수 있다. (실행될 메서드는 UISceneDelegate 구현 여부에 따라 결정됨)

### Background

앱이 background에 있으며 다른 앱으로 전환되었거나 홈 버튼을 통해 밖으로 나갔을 때의 상태

[sceneDidEnterBackground(_:)](https://developer.apple.com/documentation/uikit/uiscenedelegate/3197917-scenedidenterbackground?language=objc) or [applicationDidEnterBackground:](https://developer.apple.com/documentation/uikit/uiapplicationdelegate/1622997-applicationdidenterbackground?language=objc)