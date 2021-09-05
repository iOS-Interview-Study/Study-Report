# GCD API 동작 방식과 필요성에 대해 설명하시오.
### Concurrency는 무엇인가?
동시에 하나 이상의 작업이 수행되는 것을 이릅니다. 작업은 목적을 이루기 위한 명령의 집합으로, 이미지를 다운로드하거나 테이블을 리프레시하는 것을 예로 들 수 있습니다.

### 왜 Concurrency를 사용하는가?
User interface를 responsive하게 유지하기 위해서 사용합니다. Concurrency를 통해 table View가 이미지 다운로드나 변형 작업으로 인해 원활히 스크롤 되지 않는 문제를 해결할 수 있습니다.

### 어떻게 Concurrency를 사용할 수 있는가?
앱을 구조화하여 일부 작업이 안전하게 동시에 수행될 수 있도록 합니다. 리소스가 thread safe하지 않다면 동일한 리소스를 수정하는 작업을 동시에 수행하지 않도록 합니다. 다른 리소스를 사용하거나 프로퍼티를 읽는 작업만을 수행한다면 안전하게 작업을 동시에 수행할 수 있습니다. GCD와 Operation을 통해 어떤 작업을 수행할 것인지 시스템에게 알려줄 수 있습니다.

## GCD
Grand Central Dispatch(GCD)는 C언어로 작성된 lib dispatch library의 이름입니다. Swift3에서 일반적인 Swift class처럼 사용할 수 있게끔 문법을 업데이트하였습니다.

![](https://images.velog.io/images/ryan-son/post/87086ad3-4e73-4fcb-a341-3e5e6215a461/image.png)

GCD는 5개의 global dispatch queues를 제공합니다. GCD queues는 threads에서 작동됩니다. Programming threads는 복잡하지만 Dispatch queues는 이를 추상화하여 복잡성을 감추고 있습니다. Queue에 들어가면 선입선출 (First-In-First-Out) 원칙이 지켜집니다. 작업이 도착한 순서대로 시작된다는 것입니다. 하지만 global queues에서 수행된 작업의 종료 순서는 다를 수 있습니다. Global queues는 concurrent하기 때문입니다.

## Dispatch Queues
아래와 같이 UI와 관련되지 않은 작업을 serial queue에서 작업하면 어떤 경우에도 thread safe함을 보장할 수 있지만 이전에 할당된 작업을 끝낸 이후에 새로운 작업을 수행할 수 있을 것입니다.

![](https://images.velog.io/images/ryan-son/post/c1e299e7-02ab-41dc-8645-d04ec266fde8/image.png)

![](https://images.velog.io/images/ryan-son/post/1ce61d07-5c57-43da-8e08-7748dfd5728b/image.png)

반면에 concurrent queue에 이러한 작업을 할당한다면 concurrent queue는 즉시 별도의 스레드를 생성하여 비동기적으로 작업을 처리할 것입니다.

![](https://images.velog.io/images/ryan-son/post/28986357-207b-4c1f-8744-3bfb8ed99fc5/image.png)

위와 같이 작업을 동기적 또는 비동기적으로 처리하도록 현재 queue에서 다른 queue로 dispatch할 수 있습니다. 비동기적인 방식으로 작업을 dispatch하면 현재 queue는 지속적으로 다음 작업을 수행할 수 있는 반면, serial queue에 작업을 dispatch하면 현재 queue는 dispatch된 작업이 끝날 때까지 다른 작업을 수행할 수 없습니다. 이렇게 serial queue에 dispatch하는 경우는 한 시점에 하나의 작업만이 리소스를 변경할 수 있도록 다음 작업이 동일한 리소스에 접근할 때 적용할 수 있습니다. 하지만 현재 queue가 main queue인 경우에는 아주 나쁜 사용자 경험을 보여줄 수 있겠죠. UI가 응답하지 않게 되니까요.

![](https://images.velog.io/images/ryan-son/post/2b5c6e0f-a4d7-4b26-b764-64a8aec717e2/image.png)

하지만 concurrent queue에서 동기적인 작업을 dispatch한다면 다른 스레드에서 또 다른 작업을 수행할 수 있습니다. concurrent queue에 다른 작업이 도착하면 다른 스레드를 만들거나 쉬고 있는 스레드를 이용해서 작업을 수행하는 것이죠.

![](https://images.velog.io/images/ryan-son/post/d9008eb2-60f2-4831-bbd8-6de793d4e41e/image.png)

주의할 점은 concurrent는 비동기(asynchronous)와 다르다는 점입니다. 프로그래머는 하나의 serial 또는 concurrent queue과 또 다른 serial 또는 concurrent queue를 이용하여 작업을 수행하게 함으로써 동기적 또는 비동기적으로 작업을 수행할 수 있습니다.

동기냐 비동기냐는 현재 queue가 작업이 완료되기까지 대기하여야 하는지에 따라 결정되며 serial과 concurrent는 단지 스레드가 하나인지 아닌지를 의미합니다. 다시 말하면 동시에 작업을 수행할 수 있는지에 따라 serial과 concurrent가 결정된다는 것입니다.

![](https://images.velog.io/images/ryan-son/post/d4b28edb-d68a-44e5-b31e-4341ecf2c78d/image.png)

DispatchQueue가 가장 많이 사용되는 경우는 UI와 관련되지 않은 작업들이 concurrent queue를 통해 비동기적으로 처리되어 main queue에 결과를 반환하는 경우입니다.

serial queue를 통해 동기적으로 작업이 처리되는 흔한 경우로는 동일한 리소스에 대해 read가 이루어지는 경우가 있습니다. 이후 serial queue에 비동기적으로 dispatch하여 공유된 리소스의 값을 write합니다.

![](https://images.velog.io/images/ryan-son/post/eb13e28d-46cc-4bf3-919f-27d420c496f9/image.png)

queue에 작업들을 추가하는 가장 빠른 방법은 async 또는 sync 메서드를 통해 클로저에 작업을 제공하는 것입니다.

![](https://images.velog.io/images/ryan-son/post/850a2490-7ce7-4e85-9b80-08317a90322a/image.png)

# Summary
- 하나의 serial queue는 하나의 스레드만 가진다.
- concurrent queue는 필요할 경우 여러 개의 스레드를 만들어 사용할 수 있다.
- 작업을 현재 스레드에서 다른 스레드로 dispatch하여 비동기적으로 작업하게 하면 현재 스레드는 계속해서 작업을 수행할 수 있다. background queue에서 작업이 완료되면 결과물을 가져와 main queue에서 UI 관련 작업을 진행하면 된다.
- main queue가 아닌 serial queue에서 비동기적으로 dispatch하는 것은 리소스에 접근해 제어하는 유용한 방법이다.
- 원하는 QoS global queue에 접근하여 작업하게 함으로써 시스템에게 작업의 우선순위를 알려줄 수 있다.
- private queue를 만들 수 있으며, serial, concurrent 속성을 지정할 수 있다.
- DispatchWorkItem을 통해 간단한 dependencies를 지정하거나 작업을 취소할 수 있다.

# 참고자료
- [[Swift] Concurrency](https://velog.io/@ryan-son/Swift-Concurrency) - Ryan blog
- [[Swift] Dispatch Queues](https://velog.io/@ryan-son/Swift-Dispatch-Queues) - Ryan blog