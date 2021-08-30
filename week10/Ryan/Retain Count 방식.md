# Retain Count 방식에 대해 설명하시오.
> ARC를 통해 reference count를 관리하면, 인스턴스의 이니셜라이징과 참조에 의해 사용되기 전 retain이 일어나고, 사용 종료 후 release한다.

ARC는 위와 같이 작동하는 것이 이상적이나 ARC의 최적화 수준에 따라 그렇지 않은 경우가 있을 수 있다. ARC로 인해 객체의 수명주기가 결정되는 것며 이러한 수명주기를 object lifetime이라고 한다.

상기 언급한 바와 같이 Object lifetime은 ARC의 최적화 수준에 따라 달라질 수 있다. 현재 ARC 최적화 수준에 맞추어 현재 관측할 수 있는 객체의 수명주기를 Observable object lifetime이라고 하는데, 이 Observable object lifetime에 의존하여 프로그램을 디자인하면 미래에 버그를 야기할 수 있습니다.

```swift
class Traveler {
    var name: String
    var account: Account?

    init(name: String) {
        self.name = name
    }
}

class Account {
    weak var traveler: Traveler?
    var points: Int

    init(traveler: Traveler, points: Int) {
        self.traveler = traveler
        self.points = points
    }

    func printSummary() {
        print("\(traveler!.name) has \(points) points")
    }
}

func test() {
    // Retain
    let traveler = Traveler(name: "Lily")
    let account = Account(traveler: traveler, points: 1000)
    traveler.account = account
    // Release if ARC is more optimized
    account.printSummary()
}

test()
```

위의 예시에서 `account.printSummary()`가 작동되는 것은 우연에 불과합니다. 이러한 잠재적인 버그를 예방하는 방법은 아래 세 가지 방법이 있습니다.

1. `withExtendedLifetime()` 함수를 사용
2. `printSummary()` 메서드를 `Traveler` 타입으로 이동하여 강한 참조 객체를 통해 접근
3. 참조 방식 변경을 위한 타입 재설계 (아래 참조)

```swift
class PersonalInfo {
    var name: String
}

class Traveler {
    var info: PersonalInfo
    var account: Account?
}

class Account {
    var info: PersonalInfo
    var points: Int
}
```

# 참고자료
- [ARC in Swift: Basics and beyond](https://developer.apple.com/videos/play/wwdc2021/10216/) - WWDC21
- [[iOS] Automatic Reference Counting (ARC)](https://velog.io/@ryan-son/iOS-Automatic-Reference-Counting-ARC) - Ryan blog