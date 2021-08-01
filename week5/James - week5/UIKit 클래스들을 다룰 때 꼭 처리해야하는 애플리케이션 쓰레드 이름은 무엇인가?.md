# UIKit 클래스들을 다룰 때 꼭 처리해야하는 애플리케이션 쓰레드 이름은 무엇인가?



## UI 관련된 작업은 무조건 Main thread에서 처리해야 합니다.

- 사용자이벤트를 처리하며 렌더링 코드를 포함하고 있는 UIKit의 거의 대부분의 구성 요서는 nonatomic(서로 연결된)하고 이는 Thread-safe 하지 않다는 것을 의미합니다. UIApplication은 `RunLoop`  를 메인 쓰레드에서 실행하여 `Main RunLoop`  호출합니다. Main RunLoop은 애플리케이션의 실행기간 동안 거의 모든 이벤트 처리를 담당합니다. 화면을 새로 고침할 수 있는 이유 중 하나는 Main runLoop가 실행되고 있기 때문이죠. UI 재구성이 필요하면 Main RunLoop의 한 사이클이 끝나는 시점에 화면을 다시 그려주는 방식으로 다양한 화면 변화에 대응합니다. 그런데 만약에 main thread가 아닌 다른 thread에서 UI의 변화를 준다면 MainRunLoop과 작업이 동기화가 되지 않을 것이고 그렇기 때문에 필요한 화면 변화가 재때 구현되지 않는 것입니다.
- 만약 가능하다 하더라도 main thread가 아닌 다른 thread에서 실행한 UI 작업은 view의 렌더링을 담당하는 GPU를 과부화 걸리게 하기 때문에 앱의 심각한 성능저하로 이어질 수 있습니다. 다양한 쓰레드로부터 render 정보를 GPU에게 전해주는 것은 매우 바람직하지 않습니다. 왜냐하면 rendering 자체가 매우 오버해드가 근 작업이기 때문이고 동시다발적으로 GPU에게 rendering 정보를 여러 쓰레드에서 전달하게 되면 GPU는 일을 처리하지 못하는 상황에 놓이게 되기 때문에 제때 view를 그리지 못하게 됩니다.



[참고]:

[iOS: Why the UI need to be updated on Main Thread - Dywanedu](https://medium.com/@duwei199714/ios-why-the-ui-need-to-be-updated-on-main-thread-fd0fef070e7f)