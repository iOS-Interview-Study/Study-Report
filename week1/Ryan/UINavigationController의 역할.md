# UINavigationController 의 역할이 무엇인지 설명하시오.

![](https://images.velog.io/images/ryan-son/post/7c40e0d1-024c-4597-8a1c-5f581aec60b0/image.png)

## 역할
- 하나 또는 복수의 child view controllers를 관리
  - navigation stack라는 ordered array를 통해 child view controllers 관리
  - 마지막 view controller는 stack의 topmost item이 되며, 현재 보여지는 view controller를 나타냄
- app의 hierarchy level에 적합한 내비게이션 인터페이스 제공
![](https://images.velog.io/images/ryan-son/post/f94f4cd5-0193-463e-bf4e-1dcee252b2d0/image.png)
- navigation bar 제어
- `isToolBarHidden` 프로퍼티의 상태에 따라 topmost view controller가 제공하는 툴바 업데이트
- left, middle, right item, title 구성
- toolbarItems 구성

![](https://images.velog.io/images/ryan-son/post/8ba7ca02-469e-4709-8acb-78afcfc521f3/image.png)

![](https://images.velog.io/images/ryan-son/post/9d93be0e-b429-4904-a7bc-57b72b14e9c8/image.png)


## 특징
- 한 번에 하나의 child view controller만 보여줄 수 있음
- view controller의 item을 선택하여 새로운 view controller로 push 방식으로 이동할 수 있으며 이전 view controller를 현재 화면 뒤로 숨김.
- back button이나 left-edge swipe gesture 통해 숨겨진(이전) view controller로 돌아갈 수 있음