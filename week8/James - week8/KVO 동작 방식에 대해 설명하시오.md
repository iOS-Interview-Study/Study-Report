# KVO 동작 방식에 대해 설명하시오.

---



## KVO(Key-Value-Observing)란?

class 객체의 특정 값이 변경되었을 경우 이를 UI에 반영하기 위해서 컨트롤러 객체에 `Observing` 을 도입하여 delegate에게 특정한 메세지를 보내 처리할 수 있도록 하는 기능입니다. 위 방식은 model과 view와 같이 로직이 분리된 객체간의 소통시 유용하게 사용될 수 있습니다.



## 동작 방식

- 감지하고자 하는 모델이 `NSObject` 를 상속받게 해야 합니다.
- 감지하고자 하는 값 앞에 `@objc dynamic var` 를 붙여야 합니다.

``` swift
class StreamTalkMessage: NSObject {
    @objc dynamic var content: String = "19금 내용. 잘못 올렸네요"
    
    func deleteContent() {
        content = "삭제된 메세지입니다."
    }
}
```



- 감시하는 객체 토한 `NSObject` 를 상속받아야 합니다.
- `NSKeyValueObservedChange` 타입의 객체를 통해서 값의 old value와 new value를 알 수 있습니다.
- `observe(_:options:changeHandler:)` 메서드를 호출하여 원하는 값을 감시할 수 있게 됩니다.

``` swift
class StreamTalkMessageObserver: NSObject {
    @objc var streamTalkToObserve: StreamTalkMessage
    var observation: NSKeyValueObservation?
    
    init(streamTalkToObserve: StreamTalkMessage) {
        self.streamTalkToObserve = streamTalkToObserve
        super.init()
        
        observation = observe(\.streamTalkToObserve.content, options: [.old, .new], changeHandler: { object, change in
            print("message changed from: \(change.oldValue!), updated to: \(change.newValue!)")
        })
    }
}
```



#### 활용

감시자를 초기할 때 감지하고자 하는 객체를 주입시켜주면 Key Value Observing이 가능해 집니다.

```swift
let observedMessage = StreamTalkMessage()
let streamMessageObserver = StreamTalkMessageObserver(streamTalkToObserve: observedMessage)
```

```swift
observedMessage.deleteContent // 감시자의 changeHandler를 호출하게됩니다.
```

