# [week 1] - Fezz

## UINavigationController 의 역할이 무엇인지 설명하시오.



<img src="https://www.zehye.kr/assets/post-img/iOS/100.png" alt="img" style="zoom:67%;" /> 

- 계층구조로 구성된 content를 순차적으로 보여주는 container view controller
- stack 구조로 구현되어 있다. -  navigation stack
- 계층 구조 탐색으로 앱 content를 보여주기 적절하다.
- 한번에 한 child view controller의 content만 보여준다.

<br/>

<img src="https://github.com/daheenallwhite/swift-photoframe/blob/photoframe-step6/images/container-view-controller-3.jpg?raw=true" alt="img" style="zoom:40%;" />



이는 트리 구조처럼 상위 카테고리에서 점차 하위 카테고리로 넓어져 가는 구조를 표현하며 상위 카테고리로 돌아가기 위해서는 가장 최근에 보여진 뷰컨트롤부터 역순으로 거쳐 가야한다. 따라서 스택구조가 이를 구현하기에 적절하다. 이러한 네비게이션 뷰 컨트롤러는 `pop/push`메소드를 사용하여 보여지는 child view controller를 변경이 가능하다.



참고 !

[흰 블로그](https://daheenallwhite.github.io/ios/2019/07/25/Navigation-Controller/)