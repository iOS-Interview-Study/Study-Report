# KVO 동작 방식에 대해 설명하시오.
## Key-Value Observing
다른 객체의 프로퍼티의 변화를 알려주는 Cocoa 프로그래밍 패턴으로 Model과 View와 같이 논리적으로 분리된 객체들 간 소통이 필요할 때 사용할 수 있는 방식입니다. KVO를 통해 관찰하려면 대상이 반드시 `NSObject`를 상속하여야 합니다.

## KVO 적용 방법
### 1. 관찰할 대상인 프로퍼티를 KVO 대상이라고 표시
관찰할 대상을 `@objc`와 `dynamic` 키워드를 통해 KVO 대상이라고 표시합니다.
```swift
class MyObjectToObserve: NSObject {
    @objc dynamic var myDate = NSDate(timeIntervalSince1970: 0) // 1970
    func updateDate() {
        myDate = myDate.addingTimeInterval(Double(2 << 30)) // Adds about 68 years.
    }
}
```

### 2. 관찰자(Observer) 정의
관찰자 클래스의 인스턴스는 하나 이상의 프로퍼티에서 생긴 변화에 대한 정보를 관리합니다. 관찰자를 만들면 관찰하기를 원ㄴ하는 프로퍼티를 참조하는 key path를 `observe(_:options:changeHandler:)` 메서드에 입력하여 호출함으로써 관찰을 시작할 수 있습니다.

```swift
class MyObserver: NSObject {
    @objc var objectToObserve: MyObjectToObserve
    var observation: NSKeyValueObservation?
    
    init(object: MyObjectToObserve) {
        objectToObserve = object
        super.init()
        
        observation = observe(
            \.objectToObserve.myDate,
            options: [.old, .new]
        ) { object, change in
            print("myDate changed from: \(change.oldValue!), updated to: \(change.newValue!)")
        }
    }
}
```

그렇다면 이제 `NSKeyValueObservedChange` 인스턴스인 `oldValue`와 `newValue`를 가지고 관찰 대상인 프로퍼티가 어떻게 변화하는지 관찰할 수 있습니다.

만약 프로퍼티가 어떻게 변화하는지 알지 않아도 된다면 `options` 파라미터를 생략하셔도 됩니다. `options` 파라미터를 생략하면 새 프로퍼티 값과 이전 프로퍼티 값들을 저장하지 않으므로 `oldValue`와 `newValue` 프로퍼티를 `nil`로 만들어 둡니다.

### 3. 관찰할 인스턴스를 관찰자에게 전달
관찰자에게 어떤 인스턴스를 관찰하면 될지 알려주기 위해 관찰 대상인 인스턴스를 관찰자의 이니셜라이저를 통해 넘겨줍니다.
```swift
let observed = MyObjectToObserve()
let observer = MyObserver(object: observed)
```

### 4. 프로퍼티 변화에 대응하는 방법
앞서 살펴보신 방식과 같이 설정해주신 이후부터는 관찰 대상인 인스턴스의 프로퍼티가 변경될 때 관찰 대상이 관찰자에게 값의 변경이 일어났음을 알려줍니다. 아래 예시와 같이 관찰 대상인 `myDate` 프로퍼티를 변경시키면 즉시 관찰자의 change handler를 작동시킵니다.
```swift
observed.updateDate() // Triggers the observer's change handler.
// Prints "myDate changed from: 1970-01-01 00:00:00 +0000, updated to: 2038-01-19 03:14:08 +0000"
```

# 참고자료
- [Using Key-Value Observing in Swift](https://developer.apple.com/documentation/swift/cocoa_design_patterns/using_key-value_observing_in_swift/) - Apple Developer