# [week 1] - Fezz

## 앱이 In-Active 상태가 되는 시나리오를 설명하시오.



<img src="https://blog.kakaocdn.net/dn/HOlZG/btqLpSPYXIk/v5Cyv6iAkQ4dGSAxgrvvx0/img.png" alt="img" style="zoom: 80%;" /> 

<br/>

### Not running

앱이 실행되지 않았거나 완전히 종료되었을 때 상태

- **application(_:willFinishLaunchingWithOptions:)**

  앱 실행을 준비하는 메소드로 필요한 주요 객체들을 생성하고 앱 실행 준비가 끝나기 직전에 호출된다.

- **applicationDidFinishLaunching(_:)**

  앱 실행을 위한 모든 준비가 끝난 후 화면이 사용자에게 보여지기 직전에 호출되며, 주로 초기화 코드를 이곳에다 작성한다.

- **applicationWillTerminate(_:)**

  앱이 종료되기 직전에 호출된다.

  (하지만 메모리 확보를 위해 suspended 상태에 있는 앱이 종료될 때나 

  background 상태에서 사용자에 의해 종료될 때나 오류로 인해 앱이 종료될 때는 호출되지 않는다.)

 <br/>

### In-active(비활성화)

앱이 실행되면서 포어그라운드에 진입하지만 어떠한 이벤트도 받지 않는 상태

- **sceneWillEnterForeground(_:)**

  앱이 백그라운드나 `Not Running`에서 포어그라운드로 들어가기 직전에 호출되며, 비활성화 상태를 거쳐 활성화 상태가 된다.

<br/>

### Active(활성화)

앱이 실행 중이며 포어그라운드에 있고 이벤트를 받고 있는 상태

- **sceneDidBecomeActive**

  앱이 비활성상태에서 활성상태로 진입하고 난 직후 호출되며, 앱이 실제로 사용되기 전에 마지막으로 준비할 수 있는 코드를 작성할 수 있다.

<br/>

### Background

앱이 백그라운드에 있으며 다른 앱으로 전환되었거나 홈버튼을 눌러 밖으로 나갔을 때의 상태

- **sceneDidEnterBackground(_:)**

  앱이 백그라운드 상태로 들어갔을 때 호출되며, suspended 상태가 되기 전 중요한 데이터를 저장하는 등 종료하기 전에 필요한 작업을 한다.

<br/>

### Suspended

백그라운드에서 특별한 작업이 없을 경우 전환되는 상태

따로 호출되는 메소드는 없으며 `background`상태에서 특별한 작업이 없을 때 이 상태가 된다.



<br/>

블로그에 앱 라이프 사이클 정리했던 부분을 링크로 남겨놓을게요 !

[iOS Application Life Cycle](https://fezravien.github.io/posts/ios4)

[iOS Application Life Cycle - 2](https://fezravien.github.io/posts/ios8)

