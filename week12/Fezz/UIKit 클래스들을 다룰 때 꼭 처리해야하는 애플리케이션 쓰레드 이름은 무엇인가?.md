# Week 12 - Fezz

## UIKit 클래스들을 다룰 때 꼭 처리해야하는 애플리케이션 쓰레드 이름은 무엇인가?



### Main Thread

`Main Thread`는 앱에 단 한개만 존재하며, 나머지는 `Background Thread`이다.

`UIApplication`은 앱을 시작할 때 인스턴스화 되는 앱의 첫번째 부분인데, 앱의 run loop를 포함하여 main event loop를 설정한다. 그리고 일반적으로 작성하는 대부분의 코드는 `Main Thread` 에서 실행된다. 또한 Event 처리를 시작하며 앱의 main event loop는 touch, gesture 등의 모든 UI event를 수신한다. 

`Main Thread` 는 Interface Thread 라고도 불리며 유저의 Event는 Main Thread로 전달되고 앱은 작성된 코드에 의해 반응을 하게 된다. 즉 모든 인터페이스에 관련된 코드(UI) 는 반드시 Main Thread에서 작성되야 함을 의미한다.

<br>

#### Main Thread에서만 UI를 업데이트하는 이유

**"UIKit은 Thread-safe하지 않다"**

UIKit의 대부분의 구성 요소는 서로 연결되어 있으며, 이는 `Thread-Safe` 하지 않다는 것을 의미한다. UIKit의 모든 속성을 Thread-Safe하게 디자인 하는 것은 너무 방대한 프레임워크이기 때문에 비현실적이며 성능 측면에서 효과적이지 못해 오히려 더 느리게 동작할 것이다.

Thread-Safe 프레임 워크를 설계하는 것은 `nonatomic` -> `atomic`으로 변경하거나 `NSLock` 을 통한 방법이 있을 수있다. 

UIKit이 Thread-Safe하지 않는 것은 Apple의 의도적인 디자인으로 이러한 문제점은 시리얼큐(직렬 대기열)에서 처리함으로 해결할 수 있다.