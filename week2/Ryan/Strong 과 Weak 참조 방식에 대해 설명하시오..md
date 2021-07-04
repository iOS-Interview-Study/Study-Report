## Strong 과 Weak 참조 방식에 대해 설명하시오.
### strong (강한 참조):
- 해당 인스턴스의 소유권을 가진다.
- 자신이 참조하는 인스턴스의 retain count를 증가시킵니다.
- 값 지정 시점에 retain이 되고 참조가 종료되는 시점에 release 됩니다.
- 선언할 때 참조 성격을 알려주는 weak, unowned 등의 키워드를 명시하지 않으면 기본으로 strong이 적용됩니다.
```swift
class SomeClass {
    var someVariable: Int = 0
}

// retain strongReferred / retainCount == 1, 이니셜라이저가 retain을 대신하여 retainCount 상승시킴
var strongReferred = SomeClass() // strongReferred 참조 시작
strongReferred.someVariable = 1
// retain anotherStrongReferred / retainCount == 1
var anotherStrongReferred: SomeClass? = strongReferred // strongReferred 참조 종료, anotherStrongReferred 참조 시작
// release strongReferred / retainCount == 0
anotherStrongReferred = nil // anotherStrongReferred 참조 종료
// release anotherStrongReferred / retainCount == 0
```

### weak (약한 참조):
- 해당 인스턴스의 소유권을 가지지 않고 주소값만을 가지고 있는 포인터 개념입니다.
- 소유권을 가지지 않는다는 것은 참조하는 인스턴스의 retain count를 증가시키지 않는다는 의미이기도 합니다. 따라서 release도 발생하지 않습니다.
- 자신이 참조는 하지만 weak 메모리를 해제시킬 수 있는 권한은 다른 클래스에 있습니다.
- 메모리가 해제될 경우 자동으로 레퍼런스가 nil로 초기화를 해줍니다.
- 해당 개체가 nil일 수 있으므로 weak 속성을 사용하는 객체는 항상 optional 타입이어야 합니다.
```swift
class SomeClass {
    var someVariable: Int = 0
}

// retain strongReferred / retainCount == 1, 이니셜라이저가 retain을 대신하여 retainCount 상승시킴
var strongReferred = SomeClass() // strongReferred 참조 시작
strongReferred.someVariable = 1
// release ? 아래 문단 참조
weak var weakReferred: SomeClass? = strongReferred // nil? 아래 문단 참조
```

위의 예시의 경우 `strongReferred`의 참조가 종료되는 시점 (`someVariable` 변수 할당 이후) 즉시 release 된다고 생각할 수 있지만 애플의 ARC 최적화 정도에 따라 release 시점이 달라질 수 있습니다. 따라서 `weakReferred`가 `strongReferred` 인스턴스일지, nil일지 확신할 수 없습니다. [이 글](https://velog.io/@ryan-son/iOS-Automatic-Reference-Counting-ARC)에서 이러한 위험성를 세 가지 옵션을 사용해서 제거해줄 수 있다는 이야기를 했었습니다.

## 참고 자료
- [[Swift] 메모리를 참조하는 방법 (Strong, Weak, Unowned)](https://devsrkim.tistory.com/entry/Swift-%EB%A9%94%EB%AA%A8%EB%A6%AC%EB%A5%BC-%EC%B0%B8%EC%A1%B0%ED%95%98%EB%8A%94-%EB%B0%A9%EB%B2%95-Strong-Weak-Unowned) - srkim Develop Blog