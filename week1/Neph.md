# Delegate vs Notification

앱에서 발생한 이벤트가 현재 화면이 아닌 다른 화면까지 영향을 주어야할 때 Delegate과 Notification 중 선택해서 사용하는 것이 대부분이다. (외부 라이브러리의 도움은 받지 않는다고 가정)

Delegate와 Notification 방식은 결과는 같지만 그 과정이 조금 다르다. 오늘은 이 두가지 방법의 공통점과 차이점에 대해 공부해보았다.



## 공통점

Delegate, Notification 모두 어떤 이벤트를 다른 화면에 전달할 수 있다는 기능을 가진다. Delegate 패턴은 다른 객체의 인스턴스를 내부적으로 보유하여 그 인스턴스를 활용하는 방식으로 운용되며, Notification은 어떤 객체를 Observing하여 그 객체의 변화에 따라 Observer들이 이벤트를 받아 처리할 수 있는 방식으로 운용된다.

일반적으로 `SomeDelegate`과 같은 꼴로 쓰이는 delegate은 전자의 경우라면 SomeDelegate을 가진 객체는 Some의 이벤트를 전달받아 이벤트를 처리하는 역할을 한다. 외부에 있는 Some 객체에서 접근할 수 없는 내부 요소의 수정, 이벤트의 시작이 이루어지는 경우 보통 사용한다.

Notification의 경우에는 어떤 값의 변화에 따라 어떤 이벤트 (프로퍼티의 변화, 함수의 trigger)가 발생해야할 때 이를 observe 하고 있다가 실행하는 구조이다. 

두 방식 모두 `값의 변화`를 포착하여 `이벤트`를 발생시킨다는 공통점을 가지고 있다.



## 차이점

가장 큰 차이점은 Notification과 다르게 Delegate의 경우에는 수신자가 발신자의 정보를 알고 있어야한다는 것이다.

SomeDelegate에서 SomeDelegate을 가진 객체는 SomeDelegate protocol의 메서드로 이벤트가 발생했을 때 처리해줄 수 있는 메서드를 요구함으로써 SomeDelegate을 가진 객체가 이벤트를 처리할 수 있도록 해준다.

즉, SomeDelegate protocol을 제작할 때 메서드의 틀을 미리 잡아놓아야하며 SomeDelegate이라는 변수를 이벤트 처리 담당 객체가 들고 있어야 한다. 하지만 Notification의 경우에는 이 과정이 무의미하다. 단순히 어떤 값의 변화를 포착하여 그에 맞는 이벤트를 발생시켜주면 된다. 

장점으로도 보이는 이 특징은 양날의 검과 같다. Delegate을 사용하여 설계하지 않고 Notification만 가지고 설계를 했다면 다른 사람이 코드를 유지보수할 때 이 Notification을 누가 Subscribe하고 있는지 알기가 어렵다. 반면 Delegate으로 설계를 했다면 해당 Delegate의 이름, 프로토콜의 메서드 등을 통해 어떤 과정에 의해 이벤트 처리 로직이 돌아가는지 파악하기 한결 수월하다.
