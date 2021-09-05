# MVC 구조에 대해 블록 그림을 그리고, 각 역할과 흐름을 설명하시오.

## MVC

> UI, 데이터 및 로직 제어를 구현하는데 널리 사용되는 소프트웨어 디자인 패턴. 소프트웨어의 비즈니스 로직과 화면을 구분하는데 중점을 두고 있습니다.

![](https://images.velog.io/images/ryan-son/post/961fa068-4dd2-4420-afcc-0181829c10be/image.png)

### View

데이터를 사용자에게 보여주는 방식인 UI를 정의합니다. 표시할 데이터를 모델로부터 받습니다.
ex. 버튼 탭

### Controller

사용자로부터 받은 입력에 대한 응답으로 모델 또는 뷰를 업데이트하는 로직을 가지고 있습니다. 모델에 대한 조작이 필요없을 경우에는 바로 뷰를 업데이트할 수 있습니다.

### Model

앱이 가진 데이터를 정의합니다. 데이터의 상태가 변경되면 뷰에 전달되어 뷰가 업데이트 됩니다.

# 참고자료

- [MVC](https://developer.mozilla.org/ko/docs/Glossary/MVC) - MDN Web Docs