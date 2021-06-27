# UINavigation Controller

---

> A container view controller that defines a stack-based scheme for navigating hierarchical content
>
> [UINavigation Controller](https://developer.apple.com/documentation/uikit/uinavigationcontroller)

Navigation Controller는 컨텐츠를 계층적으로(stack-based) 관리하는 **Container View Controller** 입니다.



#### Container View Controller란?

- 특정 ViewController가 다른 ViewController를 **Contain**하게 해 주는 컨트롤러라고 볼 수 있습니다.

  - **Navigation Controller**

    ![Screen Shot 2021-05-25 at 4.22.36 PM](https://raw.githubusercontent.com/inwoodev/uploadedImages/uploadedFiles/20210525162257.png)

    - Container View Controller를 활용하여 Navigation Controller의 Stack을 쌓을 수 있다.

  - TabBar Controller

    ![Screen Shot 2021-05-25 at 4.21.57 PM](https://raw.githubusercontent.com/inwoodev/uploadedImages/uploadedFiles/20210525162221.png)

    - 여러 개의 ViewController가 TabBar Controller에 연결 될 수 있고 이는 Container View Controller을 활용하기에 가능합니다.

  - PageViewController![ClfJI](https://raw.githubusercontent.com/inwoodev/uploadedImages/uploadedFiles/20210625130704.png)

Navigation Controller는 하나 또는 그 이상의 자식 view controller를 **Navigation Interface**에서 관리합니다.

#### Navigation Interface

> iOS에서 내비게이션 인터페이스는 주로 계층적 구조의 화면전환을 위해 사용되는 드릴 다운 인터페이스(drill-down interface)입니다. 드릴 다운 인터페이스란 아래 그림과 같이 각 선택할 수 있는 항목에 대한 세부항목이 존재하는 인터페이스입니다.
>
> 출처: boostcourse - iOS앱프로그래밍

![68_0](https://raw.githubusercontent.com/inwoodev/uploadedImages/uploadedFiles/20210625132225.png)



#### Contents View Controller

네비게이션 컨트롤러는 Navigation Stack을 사용하여 자식 컨트롤러들을 관리하고 해당 stack에 담겨서 contents를 보여주게 되는 View Controller를 Content View Controller라고 부릅니다.

![68_1](https://raw.githubusercontent.com/inwoodev/uploadedImages/uploadedFiles/20210625143306.png)



#### Navigation Stack

Navigation Controller에 의해 관리되는 Navigation Stack은 View Controller를 담을 수 있는 stack 배열입니다.

Navigation Stack 은 push / pop을 통하여 View Controller들을 관리합니다.

해당 stack에 있는 가장 상위에 있는 View Controller를 **최상위 View Controller**라 부르고 해당 View Controller로의 View가 화면에 보이게 됩니다.

가장 하단에 있는 View Controller를 **Root View controller**라 부르고 해당 View Controller는 Pop 되지 않습니다.

![68_2](https://cphinf.pstatic.net/mooc/20171230_38/1514570281875SWKpk_PNG/68_2.png)



#### Navigation Bar

![3abba22e-4aef-47dd-b4e2-a9965c424338](https://raw.githubusercontent.com/inwoodev/uploadedImages/uploadedFiles/20210625145948.png)

Navigation Bar는 Navigation Controller에 의해 생성됩니다. Navigation Bar는 Navigation Controller의 모든 View Controller 상단에 표시 됩니다.

최상위 View Controller가 변경 될 때 마다 Navigation Controller는 Navigation Bar를 업데이트 해줍니다.

#### Navigation Bar의 구조

- Navigation Bar는 Navigation Interface의 상단에 표시됩니다.

- Navigation Bar는 Navigation Item들을 가질 수 있습니다.

![68_7](https://raw.githubusercontent.com/inwoodev/uploadedImages/uploadedFiles/20210625145917.png)

View Controller가 바뀔 때 마다 Navigation Item들이 바뀌지만 Navigation Bar 자체는 Navigation Controller가 관리하는 하나의 공통 객체입니다.



# UINavigationController의 역할은 무엇인가?

---

위 내용을 종합 해 보자면 

- Navigation Controller는 컨텐츠를 계층적으로(stack-based) 관리하는 **Container View Controller** 입니다.

- Navigation Controller는 하나 또는 그 이상의 자식 view controller를 **Navigation Interface** 또는 drill-down Interface에서 관리합니다.
- Navigation Controller에 의해 관리되는 Navigation Stack에 담긴 자식 View Controller를 push, pop 하여 사용자에게 다양한 콘텐츠를 나타낼 수 있습니다.

[참고]:

[1)내비게이션 인터페이스란- iOS 앱 프로그래밍](https://www.boostcourse.org/mo326/lecture/16857)

[UINavigation Controller | Apple Developer Documentation](https://developer.apple.com/documentation/uikit/uinavigationcontroller)

[UINavigationBar | Apple Developer Documentation](https://developer.apple.com/documentation/uikit/uinavigationbar)

[Creating a Custom Container View Controller | Apple Developer Documentation](https://developer.apple.com/documentation/uikit/view_controllers/creating_a_custom_container_view_controller)