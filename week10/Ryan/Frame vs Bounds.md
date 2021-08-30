# Bounds 와 Frame 의 차이점을 설명하시오.
> Frame: 상위 뷰 (Superview) 좌표계를 기준으로 한 자신의 위치 및 크기 (회전하지 않고 자신을 모두 포함하는 사각형). 
> Bounds: 자신의 좌표계를 기준으로 한 자신의 위치 및 크기

![](https://images.velog.io/images/ryan-son/post/752729e5-b813-4383-9803-4286957945a4/image.png)

## Frame
`View`의 위치와 크기를 설정할 때 사용합니다.

![](https://images.velog.io/images/ryan-son/post/56b67552-5e64-4d91-9ebf-2d82693ad2e5/image.png)

![](https://images.velog.io/images/ryan-son/post/8daef15b-a317-4522-b436-b38844777985/image.png)

## Bounds
회전, 이동 등과 같은 `View`의 변형과 관계 없이 View의 크기를 얻거나, Scroll View와 같이 View 자체의 위치 (viewport)를 이동할 때 사용한다.

![](https://images.velog.io/images/ryan-son/post/b0b32d9e-82b1-4215-b4c0-677c371f8c83/image.png)


# 참고자료
- [View and Window Architecture (View Programming Guide for iOS)](https://developer.apple.com/library/archive/documentation/WindowsViews/Conceptual/ViewPG_iPhoneOS/WindowsandViews/WindowsandViews.html#//apple_ref/doc/uid/TP40009503-CH2-SW1) - Apple Developer Documentation Archive
- [Frame과 Bounds의 차이](https://velog.io/@cskim/frame-vs-bounds) - cskime 블로그