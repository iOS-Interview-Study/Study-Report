# Week3 - Fezz

## App의 Not running, Inactive, Active, Background, Suspended에 대해 설명하시오.

<br>

### Respond to Scene-Based Life-Cycle Event && Respond to App-Based Life-Cycle Events

<img src="https://raw.githubusercontent.com/Fezravien/UploadForMarkdown/forUpload/img/024b99c5-4ab6-4ee0-bb41-6e6426ec6a64.png" alt="An illustration showing the state transitions for a scene-based app. Scenes start in the unattached state and move to the foreground-active or background state. The foreground-inactive state acts as a transition state." style="zoom:50%;" /><img src="https://raw.githubusercontent.com/Fezravien/UploadForMarkdown/forUpload/img/4d403429-fa30-4706-863f-5e3617ee21d0-20210710185050073.png" alt="An illustration showing the state transitions for an app without scenes. The app launches into the active or background state. An app transitions through the inactive state. " style="zoom:50%;" />



#### State

```swift
@available(iOS 4.0, *)
public enum State : Int {
      case active = 0
      case inactive = 1
      case background = 2
}
```

- `Active`

  앱이 `Foreground`에서 실행되고 있으며, 사용자에게 이벤트를 받을 수 있다.

- `In-Active`

  앱이 `Foreground`에서 실행이 되고 있지만, 이벤트를 받지 않는 상태이다.

  또한, 인터럽트로 인해 발생하거나 `Background`에서 active로 상태 변환을 할 때 이 상태를 통과하게 된다.

- `Background`

  앱이 `Background`에서 실행되고 있다.

- `Suspended`

  이 상태는 실제 case에는 존재하지 않는다.

  앱이 `Background` 상태에 있으며, 아무 실행을 하지 않는 상태를 의미한다.

<br>

---

### Life Cycle에서 메모리와 프로세스

- `Unattached`

  `scene`이 **시스템으로 부터 connection notification을 받기 전**까지는 **이 상태를 유지**한다. 따라서 메모리를 점유하고 있고, 실행 중인 상태라고 할 수 있다.

- `Suspended`

  `App`이나 `scene`이 **백그라운드에 있고 아무것도 실행되지 않는 즉, 프로세스 대기 상태**이다. 따라서 메모리는 점유하지만 대기중인 상태라고 할 수 있다.

- `Not Running`

  아예 `App`이 **실행되지 않았거나, 실행이 되었었지만 시스템에 의해서 종료된 상태**이다. 따라서 메모리에도 없고, 프로세스의 관점에서도 아무 것도 실행되지 않는다.

<br>

참고 📄

[Fezz blog](https://fezravien.github.io/posts/ios8)

[Managing Your App’s Life Cycle](https://developer.apple.com/documentation/uikit/app_and_environment/managing_your_app_s_life_cycle)

[iOS Application Life Cycle - minni](https://velog.io/@minni/iOS-Application-Life-Cycle#respond-to-app-based-life-cycle-events)