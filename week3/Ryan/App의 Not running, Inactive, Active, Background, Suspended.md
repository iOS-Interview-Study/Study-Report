#  App의 Not running, Inactive, Active, Background, Suspended에 대해 설명하시오.



## Respond to App-Based Life-Cycle Events

<img src="https://docs-assets.developer.apple.com/published/c63cd35863/4d403429-fa30-4706-863f-5e3617ee21d0.png" alt="An illustration showing the state transitions for an app without scenes. The app launches into the active or background state. An app transitions through the inactive state. " style="zoom:50%;" />

### Not Running

: 앱이 아예 실행되지 않았거나 시스템에 의해 종료되었을 때의 상황입니다.

### Inactive

: 앱이 foreground 상태이기는 하나 이벤트를 받지 못한 상태입니다.

### Active

: 앱이 foreground에서 실행 중이며 이벤트를 받았을 때의 상황입니다.

아이패드에서 분할화면으로 되어 있을 때 둘다 active 상태, 시간도 동일하게 표시됨

### Background

: 앱이 background에 있으며 코드를 실행하고 있는 상태입니다.

### Suspended

: 앱이 background이며 앱이 메모리에 남아 있긴 하나 코드를 실행하고 있지 않은 상태입니다.

<img src="https://www.notion.so/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2Ff0cd5d32-2e76-4b88-a126-e621a9ef73b1%2FUntitled.png?table=block&id=176dca8e-1ce2-4385-83a1-d9556a5897c9&spaceId=fd0602f0-5c84-47c9-931f-7b59490cd6f6&width=770&userId=af69fa72-3202-4872-a481-9206b854df18&cache=v2" alt="img" style="zoom:50%;" />

- 대부분의 앱의 상태 변화는 delegate 메서드에 대한 호출이 동반

→ 앱의 상태 변화에 대해 적절한 방식으로 대응할 수 있는 메소드들이 있음


![img](https://www.notion.so/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2Fa5cdcb89-0cdc-447c-b9a2-2d0708ddd3ef%2FUntitled.png?table=block&id=7e5f99b8-da30-4950-9ec1-5f78cbf4ee5e&spaceId=fd0602f0-5c84-47c9-931f-7b59490cd6f6&width=1150&userId=af69fa72-3202-4872-a481-9206b854df18&cache=v2)

앱이 실행 될 때 app delegate의 `application:willFinishLaunchingWithOptions:` 과 `application:didFinishLaunchingWithOptions:` 메서드를 이용하여 다음과 같은 것을 하는 것이 가능함

1. 왜 앱이 실행되었는지에 대한 실행 옵션에 대한 내용을 체크하고 이에 적절하게 대응할 수 있음
2. 앱의 중요한 데이터 구조를 초기화 할 수 있음
3. 앱이 보일 window와 뷰를 준비하는 것이 가능

→ 앱이 실행될 때 소요되는 시간을 줄이기 위해 가벼울 수록 좋음

### App Launch Cycle



![img](https://www.notion.so/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2F0620f37b-38d7-4fee-bdbc-9aaf00e6dc66%2FUntitled.png?table=block&id=34ae3104-8638-46fa-95df-429787910604&spaceId=fd0602f0-5c84-47c9-931f-7b59490cd6f6&width=860&userId=af69fa72-3202-4872-a481-9206b854df18&cache=v2)

(앱이 실행되고 foreground 에서의 상태 변화)

- 앱이 실행되면 시스템은 앱의 메인 함수를 메인 스레드에서 호출하기 위하여 프로세스와 메인 스레드를 생성
- Xcode 프로젝트의 메인 함수는 즉시 UIKit framework를 제어할 수 있게 해줍니다!!

## Respond to Scene-Based Life-Cycle Events

<img src="https://docs-assets.developer.apple.com/published/61283402a3/024b99c5-4ab6-4ee0-bb41-6e6426ec6a64.png" alt="An illustration showing the state transitions for a scene-based app. Scenes start in the unattached state and move to the foreground-active or background state. The foreground-inactive state acts as a transition state." style="zoom:50%;" />

사용자나 시스템이 우리의 앱에 새로운 씬을 요구한다면, UIKit 은 그것을 생성하고 그것을 ***`unattached`\*** 상태로 만듭니다.

*이때는 씬이 어떤 앱과 연결되어 있지 않은 순수한 씬 상태입니다.*

 

*사용자 요청 씬은* 빠르게 화면 상에 나타나게 되는 ***`foreground`\***상태로 이동합니다.

 

시스템 요청 씬은 `background`로 이동하여 이벤트를 처리하게 됩니다.

예를 들어, 시스템은 보통 location event 를 처리하기 위해 씬을 **`background`**에서 시작합니다.

 

사용자가 앱 UI 를 화면에서 없애버렸을 때 ( dismiss ) , UIKit은 관련 씬을 **`background`** 상태로 바꾸고 결국에는 **`suspended`** 상태에 놓습니다.

# 참고자료

- [Managing Your App's Life Cycle](https://developer.apple.com/documentation/uikit/app_and_environment/managing_your_app_s_life_cycle) - Apple Developer
- [Managing Your App's Life Cycle (공식문서 정리)](zoom.us/j/97604281364?pwd=djI5TTEyaWVScU85UmFqQjYvNkxjZz09#success) - 우짱의 iOS 블로그