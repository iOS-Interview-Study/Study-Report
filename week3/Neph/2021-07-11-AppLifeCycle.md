---
layout: post
title: "App Life Cycle"
image:
categories: iOS
tags: 
  - app life cycle
sitemap:
  changefreq: daily
  priority: 1.0
---

## App Life Cycle

### 앱의 시작

iOS는 C언어 기반을 토대로 작동하기 때문에 앱이 시작되면 가장 처음으로 main 함수가 불린다. main 함수는 UIKit framework이 관리하며 UIApplication 객체를 생성한다. 이를 통해 개발자는 앱의 실행에 부분적으로 관여할 수 있다.



### Main Run Loop

<img src="https://raw.githubusercontent.com/Neph3779/Blog-Image/forUpload/img/20210711155954.PNG" alt="img" style="zoom:50%;" />

이벤트의 처리 과정은 다음과 같이 정리할 수 있다.

1. 유저가 이벤트를 발생시킨다.
2. 시스템을 통해 이벤트가 생성된다.
3. UIKit에 의해 생성된 Port가 이벤트를 이벤트 큐에 삽입한다.
4. 이벤트 큐의 이벤트가 하나씩 Main Run Loop에 매핑된다.
5. UIApplication instance는 이벤트를 전달받을 객체를 선정한다.



## 앱의 상태 변화

| <img src="https://raw.githubusercontent.com/Neph3779/Blog-Image/forUpload/img/20210711160214.PNG" alt="img" style="zoom:50%;" /> | <img src="https://raw.githubusercontent.com/Neph3779/Blog-Image/forUpload/img/20210711160423.png" alt="123540849-04b7c100-d77c-11eb-9b13-a08e8daf0ed9" style="zoom:50%;" /> |
| ------------------------------------------------------------ | ------------------------------------------------------------ |



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

### Suspended

background에서 특별한 작업이 없을 경우 전환되는 상태



## 참고한 링크

https://hcn1519.github.io/articles/2017-09/ios_app_lifeCycle

https://fomaios.tistory.com/entry/%EC%95%B1-%EC%83%9D%EB%AA%85%EC%A3%BC%EA%B8%B0App-LifeCycle-1

https://nsios.tistory.com/60

