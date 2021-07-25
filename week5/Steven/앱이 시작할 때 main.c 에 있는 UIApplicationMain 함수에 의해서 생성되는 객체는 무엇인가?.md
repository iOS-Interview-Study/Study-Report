## 앱이 시작할 때 main.c 에 있는 UIApplicationMain 함수에 의해서 생성되는 객체는 무엇인가?
Appdelegate 파일 맨 상단에 `@main` 을 말한다.

```swift
func UIApplicationMain(_ argc: Int32, 
                     _ argv: UnsafeMutablePointer<UnsafeMutablePointer<CChar>?>, 
                     _ principalClassName: String?, 
                     _ delegateClassName: String?) -> Int32
```

`principalClassName` - UIApplication 클래스의 이름을 지정합니다. nil일때는 UIApplication이 됩니다.

`delegateClassName` - App Delegate의 이름을 지정합니다.

### UIApplication

- ios 앱의 중앙 제어와 조정하는 곳(공식문서)
- 이 인스턴스는 한개의 앱당 하나만 있다.(싱글톤으로 만들어 진다)

### 참고자료

[https://zeddios.tistory.com/539](https://zeddios.tistory.com/539)
