ARC에서는 RC를 Reference Count(참조 횟수)라고 부르는데...

MRC에서는 Retain Count(보유 횟수) 라고 부름..

*그런데 Reference Count와 Retain Count은 같은 것으로 봐도 무방할 것 같음*

아예 인스턴스에 `retainCount` 라는 프로퍼티가 있음.

swift에서는 `CFGetRetainCount` 라는 함수로 인스턴스의 리테인 카운트를 알 수 있음

```swift
var instance: Person? = Person() // 1
CFGetRetainCount(instance)       // 2 아마 이 함수도 사용해서 +1이 되는 듯

var instance2 = instance
CFGetRetainCount(instance)       // 3
CFGetRetainCount(instance2)      // 3

instance = nil

CFGetRetainCount(instance2)      // 2

instance2 = nil

CFGetRetainCount(instance2)      // error: Execution was interrupted, reason: EXC_BREAKPOINT (code=1, subcode=0x180479bdc).
```

### Retain Release 방식

인스턴스가 생성되고 할당 되었을 때 자동으로 Retain Count 1증가

다른 변수에 할당을 했을 때도 RetainCount가 증가

- MRC인 경우 retain 함수를 호출해야 함.

인스턴스에 nil이 할당 되거나 중괄호 안에서 해당 인스턴스 변수에 사용이 끝나면 Release 됨

- MRC인 경우 release 함수를 호출해야 함.

### 참고자료

[https://babbab2.tistory.com/28](https://babbab2.tistory.com/28)

[https://sujinnaljin.medium.com/ios-arc-뿌시기-9b3e5dc23814](https://sujinnaljin.medium.com/ios-arc-%EB%BF%8C%EC%8B%9C%EA%B8%B0-9b3e5dc23814)

[https://medium.com/@jang.wangsu/ios-swift-rc-arc-와-mrc-란-그리고-strong-weak-unowned-는-간단하게-적어봤습니다-988a293c04ac](https://medium.com/@jang.wangsu/ios-swift-rc-arc-%EC%99%80-mrc-%EB%9E%80-%EA%B7%B8%EB%A6%AC%EA%B3%A0-strong-weak-unowned-%EB%8A%94-%EA%B0%84%EB%8B%A8%ED%95%98%EA%B2%8C-%EC%A0%81%EC%96%B4%EB%B4%A4%EC%8A%B5%EB%8B%88%EB%8B%A4-988a293c04ac)
