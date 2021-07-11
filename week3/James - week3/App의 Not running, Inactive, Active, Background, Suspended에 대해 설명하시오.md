# App의 Not running, Inactive, Active, Background, Suspended에 대해 설명하시오

---

Unattached - `scene`이 **시스템으로 부터 connection notification을 받기 전**까지는 **이 상태를 유지**한다. 따라서 메모리를 점유하고 있고, 실행 중인 상태라고 할 수 있다. scene 이 attached 상태로 돌아가는 경우는 app switcher로부터 다시 앱으로 돌아오거나 앱이 다시 리소스를 획득할 때가 있습니다.



**Not Running** - 앱이 아직 실행되지 않거나 완전히 종료된 상태다. 메모리에 올라가지 않았다.



**Suspended** - 앱이 background 상태에 있지만 코드를 실행시키고 있지 않은 상태. 앱이 background 상태로 전환된 후 더이상 작업을 수행하지 않으면 시스템에서 앱을 Suspend 상태로 바꾼다. 앱은 여전히 메모리에 존재하며 Suspend 상태가 될 당시의 상태를 저장하고 있지만, CPU나 배터리를 소모하지 않는다. Suspend 상태의 앱은 foreground 상태의 앱을 위해 메모리 부족 등의 이유로 시스템에 의해 언제든지 종료된다. 이후 앱을 실행하면 이전 상태의 화면은 나오지 않고 앱이 재시작 됩니다.



**Background** - `App`이나 `scene`이 **백그라운드에 있고 아무것도 실행되지 않는 즉, 프로세스 대기 상태**이다. 따라서 메모리는 점유하지만 대기중인 상태라고 할 수 있습니다.



[UIScene.ActivationState.unattached](https://developer.apple.com/documentation/uikit/uiscene/activationstate/unattached)

[iOS Application Life Cycle - minni](https://velog.io/@minni/iOS-Application-Life-Cycle)

