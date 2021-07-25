# UIKit 클래스들을 다룰 때 꼭 처리해야하는 애플리케이션 쓰레드 이름은 무엇인가?

> *Use UIKit classes only from your app's main thread or main dispatch queue, unless otherwise indicated.* - UIKit (Apple Developer)

![](https://images.velog.io/images/ryan-son/post/b0cd0397-eda7-40a4-acab-b04bb7aa077d/image.png)

![](https://images.velog.io/images/ryan-son/post/07bc4d61-12e9-4305-b83e-4bbfb46003c3/image.png)

![](https://images.velog.io/images/ryan-son/post/eeb9ddc2-345b-4c56-b51a-2856763bdd8d/image.png)

## 왜 main thread에서 UI 작업을 하여야하는가?
### 1. UIKit은 Thread-safe 하지 않다.
Apple은 Thread-safe를 보장하는 것은 성능 측면에서 효과적이지 않기 때문에 의도적으로 UIKit을 thread-safe 하지 않도록 설계하였습니다[1]. 뷰의 속성을 비동기적으로 변경할 수 있다고 가정하면 변경사항이 스레드 자체의 RunLoop을 따르게 될 것이기에 변경 사항이 동시에 적용되는 것보다 더 많은 위험이 발생할 수 있습니다. 어떤 스레드에서 Table View의 셀을 제거하였는데 다른 스레드에서 제거된 셀에 대해 접근하려고 할 수도 있을 것이기 때문입니다.

### 2. RunLoop와 View Drawing Cycle 문제
UIApplication은 메인 스레드에서 Main RunLoop를 호출하여 RunLoop를 초기화하며, 이를 통해 유저와의 상호작용 작업과 같은 이벤트를 처리합니다. Main RunLoop의 반복 작업을 통해 우리는 화면을 새로  고칠 수 있는 것입니다. 화면을 새로 고칠 때도 변경 사항은 즉시 반영되지 않고 현재 RunLoop 말미에서 다시 그려지는데, 이를 통해 앱의 모든 뷰에 대한 변경 사항을 처리할 수 있고 결국 모든 변경 사항이 동시에 이루어짐을 보장할 수 있습니다. 이를 View Drawing Cycle이라 합니다. 만약 백그라운드 스레드에서 UI 업데이트가 가능하다면 각 스레드마다 자체적인 RunLoop가 존재하여 레이아웃을 동시에 다시 그리는 작업을 보장할 수 없게 되고, 결국 화면의 변경이 발생할 때 변경된 요소와 미처 변경되지 못한 요소들이 공존하는 상황을 보게 될 것입니다.

# 참고자료
1. [Thread-Safe Class Design](https://www.objc.io/issues/2-concurrency/thread-safe-class-design/) - objc.io
2. [iOS: Why the UI need to be updated on Main Thread](https://medium.com/@duwei199714/ios-why-the-ui-need-to-be-updated-on-main-thread-fd0fef070e7f) - Dywanedu blog