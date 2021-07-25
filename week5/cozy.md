1. #### UIKit 클래스들을 다룰 때 꼭 처리해야하는 애플리케이션 쓰레드 이름은 무엇인가?

   : Main Thread 또는 Main Dispatch Queue

   메인 큐는 한개만 존재하며 main thread에서 동작한다.  

   

   **Non-Atomic, Thread-Safe 하지 않은 UIKit**

   UIKIT은 자체적으로 큰 framework 이기 때문에, 성능적인 이슈를 고려하여 다른 프로세스나 쓰레드에 의해 interrupt가 되어지는 Non-Atomic하고, Thread-Safe 하지 않다는 특징을 갖고 있다. 이러한 특징으로,  serial한 처리를 위해 Main Thread에서 synchronously 한 동작을 하게 되었다. 

   

   **앱의 UI event 과정** (responder chain(->)을 따라 UIResponder로 전달)

   UIApplication -> UIWindow -> UIViewController -> UIView -> subviews 

   

   Responder는 버튼을 누르거나, 탭, 확대/축소를 하며 변경되는 UI 이벤트를 처리하며, 이러한 event chain이 UIKit의 main thread에서 작동하게 된다. **UI에 관련된 모든 이벤트는 main thread에 붙기 때문에 반드시 main에서 해주어야 한다.** 

   

   다른 Queue에서 작업하다가도, 화면에 표시해야 하는 작업은 반드시 Main Queue로 보내서 작업해야한다. 

   ```
   DispatchQueue.global().async {
       // (비동기 네트워크 작업) 이미지 다운로드
       ...
       
       DispatchQueue.main.async {
           // (UI관련) 다운로드한 이미지를 화면에 표시
       {
   }
   ```

   

2. #### 앱이 시작할 때 main.c 에 있는 UIApplicationMain 함수에 의해서 생성되는 객체는 무엇인가?

   

```swift
public func UIApplicationMain(_ argc: Int32,_ argv: UnsafeMutablePointer<UnsafeMutablePointer<Int8>>!,_ principalClassName: String?,_ delegateClassName: String?) -> Int32
```

`argc`(argv의 갯수) ,`argv`(argument의 변수 목록): main 함수로 받는 것으로 쉘에서 프로그램을 실행할 때 프로그램 실행 명령어와 함께 인자로 들어오는 값을 넣어준다.

`principalClassName`: UIApplication 클래스를 상속한 경우 해당 클래스 이름을 전달한다. 

`delegateClassName`: 앱 delegate 클래스 이름. 

UIApplicationMain 함수에서는 앱의 주요 객체 생성, UI 로딩, 앱의 초기 셋팅값(info.plist)을 불러오고, 앱을 run loop에 올려놓으며 함수를 진행시킨다. 



3. #### App thinning에 대해서 설명하시오.

   

   ** App Thinning**

   앱이 디바이스에 설치될 때 앱 스토어와 운영체제가 그 디바이스의 특성에 맞게 설치하도록 하는 설치 최적화 기술로 앱의 설치 용량을 최소화하고 다운로드 속도를 향상시킬 수 있다.

    

   **App Thinning 구성요소** 

   1) **bitCode**: 디바이스에 맞게 앱을 최적화하여 재컴파일을 하여 바이너리를 새로 만들어 제공.

   앱스토어가 앱을 최적화하려면, 비트코드를 켜서 Archive하고 앱스토어에 올려야 한다.

    

   2) **on-demand resource** : 유저가 요청할 때에만 앱스토어에서 더 많은 리소스를 가져올 수 있는 것

   

4. #### 멀티 쓰레드로 동작하는 앱을 작성하고 싶을 때 고려할 수 있는 방식들을 설명하시오.

   

   멀티 쓰레딩: 여러개의 스레드가 동시에 진행된다. 하나의 프로세스 내에 여러개의 스레드가 존재하고, 스레드들 간의 자원은 공유하지만 독립적으로 실행되는 구조이다. 

   별도의 자원 공유가 아닌 전역 변수의 공간 또는 동적으로 할당된 공간(Heap영역)을 이용하여 데이터를 주고 받으므로 스레드간의 통신 방법이 프로세스간 통신 방법에 비해 간단하다. 

   

   *atomic operation: 해당 명령을 수행하는 도중 다른 명령이 끼어들 수 없는 명령어

   

   1) *mutual Exclusion* :

   - Immutable한 인스턴스는 스레드에 안전하다. 

   - 스레드에 안전하지 않은 Mutable한 경우에는 해당 thread에 lock이나 semaphore를 걸어 공유자원에는 하나의 thread만 접근 가능하게 한다.

   

   2) *Thread Local Storage* : 

   - 특정 thread에만 접근 가능한 저장소를 만든다. Swift에서는 GCD를 통해 처리를 하여, 클로저 블록 안에 있는 특정 작업을 큐에 올려 해당 큐를 특정 스레드에 실행시키는 방식을 사용한다. 

     

   3) *ReEntrancy* : thread에서 동작하는 코드가 동일 thread에서 다시 수행되거나, 다른 thread에서 해당 코드를 동시에 수행해도 동일한 결과값을 얻을 수 있도록 코드를 작성한다. 이는 thread 진입시에 local state를 저장하고 이를 atomic 하게 구현할 수 있다. 

   

   4) *Atomic Operation* : 데이터 변경시 atomic하게 데이터에 접근하도록 프로퍼티의 속성을 설정한다.  

   

   5) *Immutable Object* : 객체 생성 이후에 값을 변경할 수 없는 값타입(구조체)을 전달 할 시, 스레드에 보다 안전합니다. 

   

------

1번 질문 참고

https://developer.apple.com/documentation/uikit

https://tngusmiso.tistory.com/49

https://zeddios.tistory.com/516

https://velog.io/@yongchul/iOSThread의-기본개념



2번 질문 참고 

https://jinshine.github.io/2018/05/28/iOS/앱의%20생명주기(App%20Life%20Cycle)와%20앱의%20구조(App%20Structure)/



5번 질문 참고 

https://minosaekki.tistory.com/50

https://gwangyonglee.tistory.com/47

