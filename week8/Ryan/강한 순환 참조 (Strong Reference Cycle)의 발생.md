# 강한 순환 참조 (Strong Reference Cycle) 는 어떤 경우에 발생하는지 설명하시오.
## 강한 참조 (Strong Reference)
참조하는 대상에게 참조 카운트 (Reference Count)를 상승시켜 **내가 보고 있으니 없어지면 안 돼**라고 이야기 하는 방식입니다. 자신의 디이니셜라이징과 같은 방식으로 명시적으로 대상에 대한 참조를 해제하지 않으면 지속적으로 상대방에 대한 참조를 유지합니다.

## 강한 순환 참조 (Strong Reference Cycle)
강한 순환 참조는 언어의 의미 그대로 위와 같은 강한 참조를 참조 객체가 서로 수행하고 있는 상황을 떠올리시면 됩니다. Swift의 Automatic Reference Count (ARC)는 위에서 언급한 Reference Count가 0이 되어 더 이상 참조하는 객체 (나를 필요로하는 친구)가 없어질 때까지 인스턴스가 메모리에서 해제되지 않습니다. 아래의 예시를 살펴봅시다.
```swift
class Person {
    let name: String
    init(name: String) { self.name = name }
    var apartment: Apartment?
    deinit { print("\(name) is being deinitialized") }
}

class Apartment {
    let unit: String
    init(unit: String) { self.unit = unit }
    var tenant: Person?
    deinit { print("Apartment \(unit) is being deinitialized") }
}
```
참조 성격을 띠는 클래스 타입인 `Person`과 `Apartment`가 있고, `Person`과 `Apartment`가 각각 자신의 프로퍼티로 서로를 소유하는 상황입니다. 이 상황에서 아래와 같이 두 타입의 인스턴스를 만들고 원하는 상황대로 서로를 참조하도록 만들어 보겠습니다.

```swift
var john: Person?
var unit4A: Apartment?

john = Person(name: "John Appleseed")
unit4A = Apartment(unit: "4A")

john?.apartment = unit4A
unit4A?.tenant = john
```
각각의 인스턴스는 두 개의 대상으로부터 참조되고 있습니다.
1. 자신이 할당된 변수 (`john`, `unit4A`)

2. 자신을 참조하는 상대 인스턴스의 프로퍼티 (Person 내 apartment, Apartment 내 tenant) 

  ![](https://images.velog.io/images/ryan-son/post/66ee0197-2d4c-4e3d-b3a7-4e9fb5cb9a30/image.png)

이 상황에서는 아래와 같은 방법으로 할당된 변수로부터의 참조를 끊어도 서로의 인스턴스가 가진 강한 참조는 여전히 살아 있기에 서로의 인스턴스가 사라지지 않게 됩니다.

```swift
john = nil
unit4A = nil
```

말씀드렸던 것처럼 디이니셜라이저들의 `print` 문들이 실행되지 않습니다. 이는 앞서 말씀드렸듯 메모리에서 서로가 서로를 바라보는 상태에서 `john`과 `unit4A` 변수가 바라보던 인스턴스와의 연결만 끊었지 메모리상에서 서로가 바라보는 Strong Reference Cycle을 끊지 못했기 때문입니다. 이와 같은 상황은 앞으로 해결될 수 없기에 앱에서 메모리 누수 (memory leak)의 원인이 됩니다. 

![](https://images.velog.io/images/ryan-son/post/48207631-68e0-4214-82b0-5e0b46ecca5f/image.png)

이를 해결할 수 있는 방법은 참조 시 참조 카운트를 증가시키지 않는 `weak` 또는 `unowned` 키워드를 통함 참조를 생각할 수 있습니다. 이전 예시와 같은 상황에서 타입 정의 시 어느 한 쪽의 참조 방식을 `weak`이나 `unowned`를 통해 참조 카운트를 상승시키지 않도록 약하게 만들어 주는 것입니다. 여기에서 `weak`은 참조하는 대상이 존재하지 않을 수 있을 때 사용할 수 있지만, `unowned`는 참조하는 대상이 반드시 존재할 것이라고 확신하는 경우에만 사용하여야 합니다 (참조할 대상이 없는 상황에서 `unowned` 키워드를 통해 정의한 대상에 접근할 경우 런타임 에러 발생).

```swift
class Person {
    let name: String
    init(name: String) { self.name = name }
    var apartment: Apartment?
    deinit { print("\(name) is being deinitialized") }
}

class Apartment {
    let unit: String
    init(unit: String) { self.unit = unit }
    weak var tenant: Person? // weak 키워드 적용, Person 인스턴스에 대한 참조 카운트를 상승시키지 않음
    deinit { print("Apartment \(unit) is being deinitialized") }
}
```

이와 같은 상황에서 아래와 같이 서로에 대한 인스턴스를 만들고 상호 참조하게 만들어 주겠습니다.

```swift
var john: Person?
var unit4A: Apartment?

john = Person(name: "John Appleseed")
unit4A = Apartment(unit: "4A")

john!.apartment = unit4A
unit4A!.tenant = john
```

그럼 아래와 같은 그림의 관계가 된 것입니다.

![](https://images.velog.io/images/ryan-son/post/7994d53b-0048-4250-96f5-c6a2c86bbc07/image.png)

이제 `john`을 디이니셜라이즈 시키면 정상적으로 `deinit`이 작동될 것입니다.
```swift
john = nil
// Prints "John Appleseed is being deinitialized"
```

`unit4A`는 `john`이 가지고 있던 인스턴스를 참조 카운트 없이 약하게 참조하고 있었습니다. 따라서 `john`이 가지고 있던 인스턴스는 바로 디이니셜라이즈 될 수 있었던 것입니다.

![](https://images.velog.io/images/ryan-son/post/08c3657e-e490-42f4-a010-05321298e5aa/image.png)

이제 `unit4A`의 인스턴스를 필요로 하는 것은 `unit4A` 변수 자신밖에 없습니다. 변수의 인스턴스에 대한 참조를 끊어주면 이제 두 인스턴스 모두 디이니셜라이즈 된 것을 디이니셜라이저에 작성해두었던 `print`문을 통해 확인하실 수 있으실 것입니다.

![](https://images.velog.io/images/ryan-son/post/fc1bcd18-2cc0-4572-91ac-56694c073c40/image.png)

추가적으로, ARC는 강한 참조를 가진 대상이 사라지면 `weak`, `unowned` 참조를 하던 대상도 함께 참조하던 내용이 사라지므로 메모리가 garbage collection을 실행할 때 강한 참조를 가지지 않은 대상들을 메모리에서 해제하던 garbage collection의 간단한 캐싱을 구현할 수는 없습니다.

돌아다니다 흥미로운 질답이 있어 가져왔습니다. [출처](https://www.notion.so/KVO-128aee8b7b894c92ace00ba7649b6397)

**Question )** willset과 didset 그리고 KVO는 서로 비슷한 역할을 하는 것인것 같은데 그 두개의 차이는 단순히 observe를 쓰냐 안쓰냐의 차이인걸까요?

**Answer )** 우리가 타입을 만드는 경우에는 자유롭게 willSet, didSet 등을 구현해줄 수 있겠지만, **다른 사람 혹은 외부 라이브러리에서 정의한 타입이라면 내부 소스를 마음대로 변경할 수 없겠죠.**

# 참고자료
- [Strong Reference Cycles Between Class Instances](https://docs.swift.org/swift-book/LanguageGuide/AutomaticReferenceCounting.html#ID51) - The Swift Programming Language