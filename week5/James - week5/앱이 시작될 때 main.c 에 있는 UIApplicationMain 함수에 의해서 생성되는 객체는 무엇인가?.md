# 앱이 시작될 때 main.c 에 있는 UIApplicationMain 함수에 의해서 생성되는 객체는 무엇인가?

---



## UIApplicationMain

코코아 터치 프레임워크에서 앱의 life cycle을 시작하는 함수입니다.

해당 함수는 UIAppplication 객채의 인스턴스를 만들고 해당 인스턴스가 앱으로서 기능하기 위해 관련 된 모든 것에 대한 조치를 취합니다.[앱 로딩 프로세스]

#### 앱 로딩 프로세스

앱 객체사 생성되는 것을 시작으로 앱 구동에 필요한 delegate, view, view controller들을 초기화 하여 GUI 동작에 필요한 런루프를 만드는 것과 같은 많은 일을 포함합니다.



공식문서를 살펴보면...



## UIApplicationMain(_ :_ :_ :_ :)

> 애플리케이션 객체 및 애플리케이션 대리자를 만들고 이벤트 주기를 설정하는 메서드

``` swift
func UIApplicationMain(_ argc: Int32, 
                     _ argv: UnsafeMutablePointer<UnsafeMutablePointer<CChar>?>, 
                     _ principalClassName: String?, 
                     _ delegateClassName: String?) -> Int32
```

### Parameters

- `argc`

  argv의 전달인자 개수, 일반적으로 main에 해당되는 매개변수

  

- `argv`

  인수의 가변 목록, main에 해당되는 매개 변수

  

- `principalClassName`

  UI Application클래스 또는 자식 클래스의 이름. nil이라 지정하면 UIapplication이 가정됨.

  

- `delegateClassName`

  애플리케이션 델레게이트가 인스턴스화 되는 클래스 이름. 만약 `principalClassName` 가 `UIApplication` 의 서브클래스를 지정하면, 해당 서브클래스를 대리자로 지정할 수 있습니다. 그리고 하위 클래스 인스턴스는 앱으로부터 위임 메세지를 받습니다. 앱의 기본 nib file로부터 불러오는 경우 해당 parameter를 nil로 설정하세요.



### Return Value

Int32(정수 값)이 반환된다고 설명에는 명시되어 있지만 사실 해당 메서드는 아무런 값을 반환하지 않습니다. 사용자가 iOS app을 종료하면 애플리케이션이 백그라운드로 이동합니다.



### Discussion

해당 메서드는 애플리케이션 객체들을 인스턴스화 하고 지정된 클래스로가 제공하고 있는 델레게이트가 있다면 해당 델레게이트를 인스턴스화 하고 앱에 대한 대리자를 설정합니다. 해당 메서드는 앱의 run loop를 포함한  main event loop을 설정하고 이벤트 처리를 시작합니다.



## 위 메서드를 통해 만들어지는 것: UIApplication



## UIApplication

모든 iOS 앱은 정확히 하나의 UIApplication 인스턴스(매우 드물게 UIApplication을 상속 받는 하위 클래스의 인스턴스)를 가지고 있습니다. 앱이 실행되면, 시스템은 `UIApplicationMain(_ :_ :_ :_ :)` 메서드를 호출합니다. 해당 메서드는 위에서 설명한대로 여러가지 작업을 하고 그 중에서도 해당 메서드는 **싱글턴 `UIApplication` 객체를 생성합니다.**



[Apple Developer Document | UIApplicationMain(_: _: _: _:)](https://developer.apple.com/documentation/uikit/1622933-uiapplicationmain)

[Apple Developer Document | UIApplication](https://developer.apple.com/documentation/uikit/uiapplication)

