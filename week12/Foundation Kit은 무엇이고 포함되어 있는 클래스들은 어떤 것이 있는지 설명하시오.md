# Foundation Kit은 무엇이고 포함되어 있는 클래스들은 어떤 것이 있는지 설명하시오.

---



## Foundation Kit이란?

Foundation Kit은 Cocoa Touch Framework에 포함되어 있는 프레임워크 중 하나입니다. 

Foundation Kit은 원시데이터타입(String, Int, Doubles..), 컬렉션타입(Array, Dictionary, Set), 그리고 운영시스템 서비스에 접근하여 앱의 기본 기능을 제공 해 주는 프레임워크입니다. 

Foundation Kit은 **UI를 제외한** 데이터타입, 자료구조, 각종 구조체, 타이머, 네트워크 통신, 그리고 파일관리와 같은 프로그램의 중심적인 기능을 제공 해 주는 프레임워크라고 볼 수 있겠습니다.



## Cocoa Foundation Framework의 특징

- Objective-C Runtime을 기반으로 합니다.
- Objective-C의 최상위 클래스인 NSObject를 상속받습니다.



따라서 Foundation Framework 또한 Objective-C Runtime을 기반으로 하며 Objective-C의 최상위 클래스인 NSObject를 상속받습니다.





## 궁금한 점: String, NSString과 같은 Swift와 Objective-C의 원시타입간의 관계는 어떻게 되는거지? String은 구조체라서 NSString을 상속받지 못할텐데...

[Swift 깃헙](https://github.com/apple/swift/blob/becc22ff3183f980dde113409929920ca1925038/apinotes/ObjectiveC.apinotes)과 [애플 공식문서](https://developer.apple.com/documentation/swift/string)에 따르면 모든 String 인스턴스는 Objective-C 인스턴스로 변환(bridge)가 가능하다고 합니다. 변환을 할 때 `as` 타입캐스트 오퍼레이터를 사용한다고 하네요.

![Screen Shot 2021-09-11 at 7.52.48 PM](https://raw.githubusercontent.com/inwoodev/uploadedImages/uploadedFiles/20210911195255.png)

```swift
let string = "String"
let nsString = string as NSString
```

그렇다면 String과 NSString은 상속관계인건가요?



아닙니다. [BrunoRocha](https://swiftrocks.com/be-careful-with-objc-bridging-in-swift)에 따르면 NSString과 String은 상속관계가 아니고 완전히 분리된 객체라고 합니다. 그리고 여기서 사용되는 `as` 는 아래와 같은 기능을 구현하는데 사용되는 문법설탕(읽는 사람 또는 작성하는 사람이 편하게 디자인 된 문법) 이라고 설명합니다.

```swift
let string = "String"
let nsstring: NSString = string._bridgeToObjectiveC()
```

`_bridgeToObjectiveC()` 메서드는 `_ObjectiveCBridgeable` 프로토콜에서 제공하는 메서드로서 스위프트 객체를 Objective-C객체로 자동으로 변환 해 주는 기능을 제공하고 프로그래머가 `as` operator를 사용해서 동일한 변환작업을 가능하게 해 주는 거죠!

따라서 `String` 과 `NSString` 은 완벽히 분리된 관계이고 둘 이 변환 가능한 이유는 swift에서 제공 해 주는 프로토콜 덕분인 겁니다.





[참고]:

[Apple Developer Documentation | Foundation](https://developer.apple.com/documentation/foundation)

[Apple Developer Documentation | Swift](https://developer.apple.com/documentation/foundation)

[GitHub Repository | apple/swift/apinotes.Foundation.apinotes](https://github.com/apple/swift/blob/becc22ff3183f980dde113409929920ca1925038/apinotes/Foundation.apinotes#L102-L104)

[Be careful with Obj-c bridging in Swift - Bruno Rocha](https://swiftrocks.com/be-careful-with-objc-bridging-in-swift)