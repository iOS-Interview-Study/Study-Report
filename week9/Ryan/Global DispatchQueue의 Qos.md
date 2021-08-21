# Global DispatchQueue 의 Qos 에는 어떤 종류가 있는지, 각각 어떤 의미인지 설명하시오.

`Grand Central Dispatch Queues`는 thread들을 관리해줍니다. 시스템은 global queues를 Quality-of-Service (QoS)라는 개념으로 분류하여 제공하는데, 이 분류를 통해 작업의 실행 우선순위를 정할 수 있습니다. 높은 우선순위의 작업들은 낮은 우선순위의 작업들보다 더 많은 자원을 통해 더 빠르게 수행되기 때문에 에너지를 더 많이 소비합니다. iOS 8.0 이상의 버전부터 사용할 수 있습니다.

![](https://images.velog.io/images/ryan-son/post/1a4fa36b-916a-47f4-8bdc-e7be51e357f5/image.png)

```swift
DispatchQueue.global(qos: .userInteractive)
DispatchQueue.global(qos: .userInitiated)
DispatchQueue.global(qos: .utility)
DispatchQueue.global(qos: .background)
DispatchQueue.global() // .default qos
DispatchQueue.global(qos: .unspecified)
```

Global queue들은 main queue와 달리 concurrent하여 하나 이상의 스레드를 가질 수 있습니다. 각 Queue는 **Quality of Service (QoS)**라는 다른 우선순위 수준을 가지고 실행됩니다. Global queue에 dispatch할 때 작업은 목적을 가질 수 있으며 스케줄러로 하여금 스레드별로 실행의 우선순위를 결정할 수 있게끔 할 수 있습니다. Concurrent global queue를 타입 메서드인 `DispatchQueue.global`의 `Quality of Service` 열거형 케이스를 통해 우선순위를 알려줄 수 있습니다.

### `userInteractive`
시스템에서 가장 높은 우선 순위를 가지는 작업입니다. 사용자와 상호작용하거나 앱의 UI를 액티브하게 업데이트하는 작업에 사용할 수 있습니다. 사용자가 드래그하거나 핀칭하는 등 실질적으로 main queue만큼 빠르게 처리되어야 할 때 사용합니다. main queue에서 실행되지는 않지만 터치 제스처할 동안 main queue에 바로 반환되어야하는 경우입니다.

### `userInitiated`
시스템에서 두 번째 우선 순위를 가지는 작업으로 사용자의 행동에 따른 즉각적인 결과물을 제공할 때 사용할 수 있습니다. 버튼을 탭하는 등 사용자가 UI를 통해 작업을 시작시킨 경우, 저장된 문서를 여는 것과 같이 즉시 작업이 수행되기를 원하는 경우에 사용할 수 있습니다.

### `default`
시스템에서 `userInteractive`, `userInitiated` 보다 낮은 우선 순위를 가지는 작업입니다. `DispatchQueue.global()`에 해당하며 QoS 값을 지정하지 않으면 기본(default) queue에 할당됩니다.

### `utility`
`default` 보다 낮은 우선 순위를 가지는 작업입니다. 연산, 입출력, 네트워킹과 같이 progress indicator를 나타냄과 동시에 수행되는 긴 작업을 수행하는데 사용할 수 있습니다. 시스템은 반응성과 성능, 에너지 효율성 간의 균형을 잡으면서 작업을 수행하게 됩니다.

### `background`
가장 낮은 우선 순위를 가지는 작업입니다. 백그라운드에서 작업이 수행될 때 여기에 할당할 수 있습니다. 사용자와의 상호작용을 필요로하지 않고 수행 시간에 민감하지 않으며, prefetching, database maintenenace, 원격 서비스 또는 백업과 동기화하기와 같은 네트워킹 작업을 일부 포함할 수 있습니다. 시스템은 에너지를 효율적으로 사용하는 것을 목표로 작업을 수행합니다.

### `unspecified`
Quality-of-Service 클래스가 없는 경우를 나타냅니다.


# 참고자료
- [DispatchQoS](https://developer.apple.com/documentation/dispatch/dispatchqos) - Apple Developer
- [[Swift] Dispatch Queues](https://velog.io/@ryan-son/Swift-Dispatch-Queues) - Ryan Velog