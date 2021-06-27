# ARC

![](https://images.velog.io/images/ryan-son/post/c2003fac-61b1-4fd2-82ba-a7ba08607712/image.png)

[ARC in Swift: Basics and beyond (WWDC21)](https://developer.apple.com/videos/play/wwdc2021/10216/)

Swift는 `struct` 및 `enum` 등의 값 타입들을 지원한다. 값 타입을 사용하면 클래스와 같은 참조 타입에서 의도치 않게 값의 공유가 일어나 발생하는 문제들을 피할 수 있습니다. 
의도적으로 참조 타입을 사용하는 경우 Swift는 Automatic Reference Counting (ARC)를 통해 프로그래머 대신 참조 타입 인스턴스의 할당과 해제를 수행하여 메모리를 관리해줍니다. 하지만 Swift를 효율적으로 사용하기 위해서는 ARC가 동작하는 방식에 대해 이해하는 것이 좋습니다.

# Object lifetimes and ARC
- 객체(object)의 수명주기는 `init()` (이니셜라이즈, 초기화)와 함께 시작하며 마지막으로 사용되면 끝납니다. 
- ARC는 객체의 수명주기가 종료된 후 객체를 메모리에서 해제합니다.
- ARC는 객체의 수명주기를 참조 카운트 (`reference count`)를 통해 추적합니다.
- Swift 컴파일러는 `retain / release` 작업을 자동으로 삽입해줍니다.
- 런타임 시 retain / release에 따라 reference count가 증가하고 감소하며, reference count 가 0이 되면 객체는 메모리에서 해제됩니다.

![](https://images.velog.io/images/ryan-son/post/9c6dba55-d35b-42ca-bc59-63c0d69e0b56/image.png)

위 그림에서 `Traveler` 타입의 인스턴스인 `traveler1`의 참조가 시작되고 끝나는 지점을 알 수 있습니다. Swift 컴파일러는 아래와 같이 `release` 작업을 수행해줍니다. `retain` 작업을 명시적으로 수행하지 않는 이유는 이니셜라이즈 시점에 reference count가 1로 상승하기 때문입니다.

![](https://images.velog.io/images/ryan-son/post/fa74bf80-6ecf-4394-a58d-9da712795258/image.png)

아래 그림에서는 `traveler2` 인스턴스에 대한 참조 시작과 종료 지점을 확인할 수 있습니다.
![](https://images.velog.io/images/ryan-son/post/93043a6e-7e76-4f82-be4e-3df161d6580a/image.png)

Swift 컴파일러는 아래와 같이 `traveler2` 인스턴스에 대한 `retain / release` 작업을 수행해줍니다.

![](https://images.velog.io/images/ryan-son/post/1add336d-766a-4a32-bf68-45c96b37b31d/image.png)

코드에서 일어나는 일을 도식화해서 살펴보겠습니다.

![](https://images.velog.io/images/ryan-son/post/d03dfb59-47e9-4895-ab03-dee0a1a6d8c7/image.png)

참조 타입인 `Traveler`는 `traveler1` 인스턴스의 이니셜라이징과 함께 reference count가 1 증가합니다.

![](https://images.velog.io/images/ryan-son/post/dc5316bc-df4a-4258-8811-3e11b03215c8/image.png)

`traveler2`에 `traveler1`을 할당하였으므로 아래와 같이 reference count가 2로 상승합니다.

![](https://images.velog.io/images/ryan-son/post/594c9e50-e9f2-4782-b89a-d5e297f97fdf/image.png)

![](https://images.velog.io/images/ryan-son/post/4944dfe7-7d72-4a32-8be0-683e7b8db887/image.png)

`traveler1` 인스턴스의 사용이 완료되었으므로 아래와 같이 `release` 작업이 수행되어 reference count가 1로 감소합니다.

![](https://images.velog.io/images/ryan-son/post/a1a5a8fe-a222-4d8e-9314-739a3c228a7e/image.png)

`traveler2`의 `destination` 프로퍼티가 할당되며 `traveler2` 인스턴스가 계속 사용되고 있으므로 reference count가 1로 유지됩니다.

이로써 `traveler2` 인스턴스에 대한 사용이 종료되었으므로 reference count를 0으로 감소시키고 최종적으로 메모리에서 해제됩니다.

![](https://images.velog.io/images/ryan-son/post/e43a0a35-6c4c-4449-b59d-18bb0f3bcf3f/image.png)

Swift에서 객체의 수명주기는 사용 기반(use-based)으로 결정됩니다. 객체의 수명은 이니셜라이징과 함께 시작하고 마지막 사용 이후에 종료됩니다. 이는 코드 블럭의 끝 (closing brace)까지 객체의 수명 주기를 보장하는 C++과 같은 언어와 작동 방식이 다름을 의미합니다. 

![](https://images.velog.io/images/ryan-son/post/5a520e8d-c60f-4cfd-8497-97d61ba3e350/image.png)

![](https://images.velog.io/images/ryan-son/post/f4ce6331-f979-4d81-a67a-a97b8bd70275/image.png)

하지만 객체의 수명주기는 Swift 컴파일러가 삽입한 `retain / release` 작업에 따라 이루어지므로 실제 관측되는 객체의 수명주기는 최소한으로 보장된 수명주기와 달라질 수 있습니다. 하지만 실제 사용에 있어서는 객체의 정확한 수명주기가 중요하지 않기는 합니다.

![](https://images.velog.io/images/ryan-son/post/7bc34c71-bd44-485d-ac93-7813aa6c7ccc/image.png)

![](https://images.velog.io/images/ryan-son/post/df6a4926-5532-48ee-a533-f9cb9fed7093/image.png)

# Observable object lifetimes

`weak` 및 `unowned` 참조를 사용하거나 `Deinitializer side-effect`를 사용하면 객체의 수명주기를 관찰할 수 있지만, 관측된 객체의 수명주기에만 의존한다면 향후 버그와 같은 문제를 발생시킬 가능성이 있습니다. 구현 방식이 달라지면 수명주기가 변경되는 등의 문제가 생기는 것이죠. 혹여 한 동안은 잘 작동할 수 있지만 컴파일러 업데이트에 의해 ARC 최적화 방식이 변경되면 문제가 발생할 수도 있습니다.

## Language features
강한 참조(Strong reference) 방식인 기본 객체와 달리 `weak` 및 `unowned` 참조 객체들은 reference counting에 관여하지 않습니다. 때문에 참조 사이클을 끊는데 (break) 주로 사용되죠.

### Reference Cycle
아래 그림에서는 `Traveler`와 `Account` 타입을 확인할 수 있습니다. `Traveler` 타입은 `Account` 타입을 옵셔널 값으로 가지고 있으며, `Traveler` 타입의 인스턴스는 `account` 프로퍼티가 할당되어 있는 경우 `Account` 타입의 `points` 프로퍼티를 통해 포인트를 쌓을 수 있습니다. 구조 상으로 `Traveler` 타입은 `account` 프로퍼티를 통해 `Account` 타입의 인스턴스를 참조하고, `Account` 타입은 `traveler` 프로퍼티를 통해 `Traveler` 타입의 인스턴스를 참조합니다. 서로가 서로를 참조하고 있는 상황이죠.

![](https://images.velog.io/images/ryan-son/post/8596299b-8a48-49fa-a5a6-b126a5a5cd7e/image.png)

아래 `test()` 함수를 통해 `Traveler` 타입과 `Account` 타입의 인스턴스를 만들어보겠습니다.

![](https://images.velog.io/images/ryan-son/post/517f067d-b201-4625-9ebc-ebd7e965be9c/image.png)

코드 상에서 일어나는 일을 도식화하면 아래와 같습니다. 먼저, 이니셜라이징과 함께 객체가 heap에 할당되며 reference count가 1로 증가합니다. 

![](https://images.velog.io/images/ryan-son/post/dd36038d-a574-42eb-b01e-77ebff9ae357/image.png)

다음으로 `Account` 타입의 인스턴스가 생성되며 reference count가 1 증가합니다. `Account` 타입은 `Traveler` 타입을 참조하므로 `Traveler` 타입의 reference count가 2로 증가합니다.

![](https://images.velog.io/images/ryan-son/post/c29fe693-ec58-44c8-97a1-39a789f8430a/image.png)

계속해서 `traveler` 인스턴스에 `account` 프로퍼티를 할당하며 `Account` 타입의 reference count가 2로 증가합니다.

![](https://images.velog.io/images/ryan-son/post/d44209ad-2947-4632-928a-55b355744c31/image.png)

`Account` 타입의 마지막 사용이 종료되었으므로 reference count가 1 감소합니다.

![](https://images.velog.io/images/ryan-son/post/8e38ddfe-065b-497d-bcc9-aebc0d212d88/image.png)

`traveler` 인스턴스 또한 `printSummary()` 메서드 사용과 함께 마지막 사용이 종료되었으므로 reference count가 1 감소합니다.

![](https://images.velog.io/images/ryan-son/post/a60becb0-04d4-47df-bb4d-48c95a54b994/image.png)

이 시점에서 생성된 `traveler` 및 `account` 객체에 접근할 수단을 잃었지만 두 인스턴스의 reference count는 1이 남아있습니다. 

reference count가 0이 되지 않았으므로 두 객체는 메모리에서 해제되지 않고 메모리 누수 (memory leak)을 야기합니다.

![](https://images.velog.io/images/ryan-son/post/88d127e8-35d5-4c1e-bdd9-10cb84e33c1d/image.png)

## weak and unowned
먼저 언급하였듯이 `weak` 및 `unowned` 키워드를 통해 사용 중에 참조된 객체를 메모리에서 해제할 수 있습니다. 한 가지 주의하여야 할 점은 이와 같은 방법을 통해 메모리에서 해제된 객체에 접근하고자 한다면 `weak` 키워드가 적용된 객체는 `nil`을 반환하고, `unowned` 키워드가 적용된 객체는 참조 트랩 (reference traps, 런타임 에러)을 야기합니다.

위 예시와 같은 상황에서는 `weak` 키워드를 통해 참조 사이클을 끊을 수 있습니다. `Traveler` 타입에 대한 reference count가 0으로 감소하므로 `Account` 타입에 대한 reference count도 0이 되어 모두 메모리에서 해제될 수 있는 것이죠.

![](https://images.velog.io/images/ryan-son/post/b32876f1-80bc-4449-b764-01342fe8c149/image.png)

위에서 잠시 언급 드렸듯이 객체의 관측된 수명주기를 이용해 프로그래밍하는 것은 미래에 버그를 야기할 수 있다고 말씀드렸는데요, 예시를 살펴보시겠습니다. 이번의 예시는 `Traveler` 타입이 가지고 있던 `printSummary()` 메서드를 `Account` 타입으로 이동하였습니다.

![](https://images.velog.io/images/ryan-son/post/1224f91b-ecef-42cf-8897-67dcc4f60c78/image.png)

위의 예시에서 `account.printSummary()` 메서드를 통해 `traveler`의 이름과 포인트를 출력해줄 수도 있겠지만, 이는 우연일 뿐입니다. `traveler` 인스턴스가 가 마지막으로 사용되고난 후 바로 메모리에서 해제가 되면 `account` 인스턴스는 `traveler` 인스턴스에 더 이상 접근할 수 없기 때문입니다.

![](https://images.velog.io/images/ryan-son/post/6cf5a42a-e358-4d49-aecd-4868c8c8a3aa/image.png)

![](https://images.velog.io/images/ryan-son/post/afb2b78e-747b-42d0-94ad-b72a5fdde5f5/image.png)

이 경우 `weak` 키워드를 통해 메모리에서 해제된 인스턴스에 옵셔널 강제 해제를 통해 접근한다면 런타임 에러를 일으키게 됩니다.

![](https://images.velog.io/images/ryan-son/post/847a472a-e79e-4806-87a8-6b81adbafc39/image.png)

왜 옵셔널 바인딩을 쓰지 않고 강제 해제를 통해 문제를 일으켰느냐라고도 할 수 있지만, 실제로 옵셔널 바인딩을 사용한 경우 더 큰 문제를 야기할 수 있습니다. 프로그램이 원하는대로 동작하지 않았지만 문제를 이야기해주지 않는 것이죠.

![](https://images.velog.io/images/ryan-son/post/00ec18c6-6afd-47fa-bb67-5d1c73820641/image.png)


## Consequences and safe techniques
`weak`과 `unowned` 키워드가 적용된 인스턴스를 더 안전하게 사용할 수 있는 방법들이 있는데요, 여러 방식들이 있지만 상충되는 `upfront implementation cost`와 `continuous maintenance cost`를 고려하여 에 따라 달리 적용될 수 있습니다.

### withExtendedLifetime()
`withExtendedLifeTime()` 함수를 이용하여 원하는 메서드 혹은 프로퍼티를 사용할 때까지 객체의 수명주기를 연장시킬 수 있습니다.

![](https://images.velog.io/images/ryan-son/post/fddab0e4-ce16-472d-b8b2-ab90cdd794df/image.png)

아래와 같이 호출 순서를 변경하여도 동일한 효과를 기대할 수 있습니다.

![](https://images.velog.io/images/ryan-son/post/3b3dbc1c-6f97-4a91-b5bf-cf6f568abe3a/image.png)

더 복잡한 경우에는 `defer()` 함수를 통해 현재 코드 블럭(스코프)에서 사용할 수 있도록 요청하는 방법이 있습니다.

![](https://images.velog.io/images/ryan-son/post/d75f8efe-e7e3-4da7-a449-76b81b28bae7/image.png)

`withExtendedLifeTime()` 함수를 적용하면 객체의 수명주기와 관련된 버그를 쉽게 해결할 수 있는 것처럼 보이지만, 사실은 불안정하며 정확성에 대한 책임을 사용자에게 전가합니다.

예를 들어, `weak` 참조 타입에 대해서는 매번 `withExtendedLifetime()` 메서드를 적용하여야 하며, 사용하지 않을 경우 버그를 일으킬 가능성이 있다는 것이죠. 통제되지 않으면 `withExtendedLifeTime()` 메서드가 코드 전반에 걸쳐 등장하게 되어 유지보수 비용을 상승하게 만듭니다.

### Redesign to access via strong reference
위와 같은 경우에는 더 나은 API를 통해 클래스를 재설계하는 것이 더 정형화된 방식입니다. 아래 그림을 보시면 `printSummary()` 메서드는 다시 `Traveler` 타입으로 이동하였고, `Account` 타입의 `weak` 참조 타입인 `traveler`는 `private` 키워드를 통해 외부에서 보이지 않게끔 감추었습니다. 이를 통해 `test()` 함수가 `printSummary()` 메서드를 호출할 때 전과는 달리 강한 참조를 이용하여 프로퍼티를 불러올 수 있게 되었습니다.

![](https://images.velog.io/images/ryan-son/post/5cfea777-f9ce-488f-9be4-67c2b29c5ed1/image.png)

### Redesign to avoid weak/unowned reference
위와는 다른 근본적인 해결 방법으로는 `weak / unowned` 참조 방식을 사용하기 전에 이 것이 꼭 필요한지에 대해 생각해보는 방법이 있습니다. 참조 사이클은 때로 알고리즘을 다시 생각해보면 회피할 수 있기 때문이죠.

![](https://images.velog.io/images/ryan-son/post/43592307-a129-4619-9e7f-0e50d396b421/image.png)

생각해보면 `Account` 타입은 `Traveler`의 `name`이라는 개인 정보에만 접근하면 됩니다. 따라서 아래와 같은 구조로 개선할 수 있죠.

![](https://images.velog.io/images/ryan-son/post/b393ba01-f2ec-4334-b1fe-04363c2872bf/image.png)

이와 같이 설계하는 것은 구현 시 비용을 증가시킬 수 있지만, 객체의 수명주기로 인해 발생하는 모든 잠재적인 버그들을 제거할 수 있습니다.


## Deinitializer side-effects
달리 객체의 수명주기를 관찰할 수 있는 방법으로는 `Deinitializer side-effects`가 있는데요, `Deinitializer`는 메모리에서 해제되기 이전에 실행됩니다.

### Deinitializer
아래 그림은 첫 번째 예제에서 Deinitializer가 추가된 형태입니다. 

![](https://images.velog.io/images/ryan-son/post/1b356f18-105c-49bf-8068-517a150ad38e/image.png)

이 또한 아래와 같이 ARC 최적화 상황에 따라 출력 위치가 달라질 수 있습니다.

![](https://images.velog.io/images/ryan-son/post/abdeabbd-e264-483a-aff7-2a9578990719/image.png)

![](https://images.velog.io/images/ryan-son/post/421e4cf9-9f0e-497b-89dc-b78d603fff58/image.png)

더 복잡한 경우를 보시죠.
이제 `Traveler` 클래스에 `travelMetrics`라는 프로퍼티를 도입해보겠습니다. 이제 `destination`이 업데이트 될 때마다 `TravelMetrics` 클래스에 기록될 것입니다. 

![](https://images.velog.io/images/ryan-son/post/e6136f7e-d881-495e-9eb3-58d3afe8d6a3/image.png)

![](https://images.velog.io/images/ryan-son/post/77352e62-b387-469c-b30c-f9193a529580/image.png)

`Deinit()`이 실행될 때 `travelMetrics`가 id와 목적지, travel interest category를 publish 합니다.

![](https://images.velog.io/images/ryan-son/post/a4d2611a-2e04-4d20-9460-e222c5a13305/image.png)

![](https://images.velog.io/images/ryan-son/post/175fe9a4-2242-4156-8671-63f452e8c6bb/image.png)

![](https://images.velog.io/images/ryan-son/post/d46fa51c-4fed-4d31-9ed5-650bf12cbb8b/image.png)

![](https://images.velog.io/images/ryan-son/post/7a7a5fc1-d569-48eb-ae4c-8d6eb32928c7/image.png)

객체의 수명주기가 사용 이후 바로 종료된다면 아래와 같이 category를 `nil`로 계산할 가능성이 있습니다. 

![](https://images.velog.io/images/ryan-son/post/61b7b1bb-b4c5-4ac1-936b-1c2a5ebea0ce/image.png)

이제 이전에 언급했던 내용들을 하나씩 적용해보겠습니다.

![](https://images.velog.io/images/ryan-son/post/0f1cbe03-5aeb-4625-a151-0b1de589c685/image.png)

![](https://images.velog.io/images/ryan-son/post/f112deb0-f61e-4a3a-9106-cc96bcb98436/image.png)

아래와 같이 `defef()`가 `deinit()` 대신에 사용될 수 있습니다.

![](https://images.velog.io/images/ryan-son/post/273ca982-f5bc-4ce7-8203-6af441da31f3/image.png)

Xcode 13부터는 아래 그림과 같이 build settings에서 `Optimize Object LIfetimes` 속성을 사용할 수 있습니다. 이 속성은 객체의 사용이 완료된 직후 메모리에서 해제하는 것을 도와주어 관찰할 수 있는 객체의 수명주기와 최소한으로 보장되는 수명주기가 비슷해지도록 만들어줍니다.

![](https://images.velog.io/images/ryan-son/post/ca6c4bcb-e09a-45b0-bbe5-752c478e2a27/image.png)