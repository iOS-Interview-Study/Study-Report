# UIWindow

![uiwindow](https://user-images.githubusercontent.com/35272802/124383545-fe8b8c80-dd07-11eb-8963-30da62c7b0b4.png)

- 앱의 뷰 계층 구조에서 최상단에 고정되어 위치하며, 앱의 화면 콘텐츠에 대한 컨테이너 역할을 합니다. 앱의 뷰, 뷰 컨트롤러와 함께 작동하여 화면에 표시되는 뷰 계층 구조에 터치 이벤트를 전달하고, 화면 방향 변경과 같은 변경 사항을 관리합니다.
- 화면에 보이는 콘텐츠는 제공하지 않습니다.
- 화면에 보이는 것은 `UIWindow` 안에 `rootViewController` 에서 제공합니다.
- 스토리보드를 사용할 때는 자동으로 만들어주지만 코드로 UI를 만들떄는 개발자가 코드를 작성해야 합니다.
- `UIView`를 상속 받습니다.
- ios13부터는 여러개의 window를 사용할 수 있습니다.
