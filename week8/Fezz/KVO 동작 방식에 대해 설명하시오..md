# Week 8 - Fezz

## KVO 동작 방식에 대해 설명하시오.



### KVO

- Key-Value Observing의 약자

- 객체의 프로퍼티의 변경사항을 다른 객체에 알리기 위해 사용하는 코코아 프로그래밍 패턴

- Model과 View와 같이 논리적으로 분리된 파트간의 변경사항을 전달하는데 유용함

- NSObjec를 상속한 클래스에서만 KVO를 사용할 수 있음.

<br>

### 구현

```swift
class Address: NSObject { 
    @objc dynamic var town: String 
    init(town: String) { self.town = town } 
}

var address = Address(town: "Fezz") 
address.observe(\.town, options: [.old, .new]) { (object, change) in 	
    print(change.oldValue, change.newValue) 
}

address.town = "fezz"
```

<br>

### 장점 

- 두 객체간의 동기화 

  Model과 View와 같이 논리적으로 분리된 파트간의 변경사항을 전달

- 객체의 구현을 변경하지 않고 내부 객체의 상태 변화에 대응할 수 있음.

  라이브러리에 들어있는 프로퍼티의 변경사항(@objc dynamic 있다는 가정)을 외부에서 옵저버를 통해 확인할 수 있다.

- 관찰된 프로퍼티의 이전값(oldValue)와 최신값(newValue)를 제공

- KeyPath를 사용하여 프로퍼티를 관찰하므로 nested 프로퍼티도 관찰 가능 

  ```swift
  class Address: NSObject { 
      @objc dynamic var town: String 
      init(town: String) { self.town = town } 
  } 
  
  class Person: NSObject { 
      @objc dynamic var address: Address 
      init(address: Address) { self.address = address } 
  }
  ```

<br>

### 단점

- NSOject 상속해야됨 == Objective-C 런타임에 의존

- 무조건 dynamic dispatch를 사용하게 됨