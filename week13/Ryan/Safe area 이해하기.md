# Safe area 이해하기
> View controller의 view를 가리는 내비게이션 바, 탭 바, 툴바, 기타 요소를 제외한 영역

`Safe area`는 모든 인터페이스의 가시적인 부분에 view를 위치시키는 것을 도와줍니다. `UIKit`이 정의한 `View controller`는 컨텐츠 위에 내비게이션 컨트롤러의 내비게이션 바와 같이 특별한 view들을 컨텐츠 위에 위치시킬 수 있습니다. 이러한 view들이 일부 투명할 수 있지만, 그래도 view 아래에 일부 컨텐츠들을 가립니다. tvOS에서는 `safe area`가 화면 베젤을 의미하는 `overscan inset`을 포함합니다.

컨텐츠의 레이아웃을 결정할 때 각 view에서 접근할 수 있는 `safeAreaLayoutGuide`를 통해 `safe area`의 도움을 받을 수 있습니다. AutoLayout을 통해 제약을 설정하는 상황이 아니라면 `safeAreaInsets` 프로퍼티를 통해 raw inset values를 얻을 수 있습니다.

![](https://images.velog.io/images/ryan-son/post/9dc18a39-6470-4f5d-af2e-5ead191872e5/image.png)

## Custom View를 포함하기 위해 safe area를 확장하기
`Container view controller`는 `embedded child view controller`의 view를 통해 컨텐츠를 보여줄 수 있습니다. 이 상황에서 `container view`의 컨텐츠 view들이 이미 커버하고 있는 영역을 `child view controller`의 `safe area`에서 제외해주기 위해 업데이트하여야 하는데, UIKit `container view controller`는 컨텐츠 view들을 고려한 `child view controller`의 `safe area`를 미리 조정합니다. 예를 들어, 내비게이션 컨트롤러는 내비게이션 바를 고려하여 `child view controller`의 `safe area`을 확장합니다.

`Embedded child view controller`의 `safe area`를 확장하려면 `additionalSafeAreaInsets` 프로퍼티를 수정하면 됩니다. 아래 그림과 같이 화면 아래와 오른쪽 끝에 커스텀 view들을 보여주겠다고 정의하였다면 `child view controller`의 컨텐츠가 커스텀 view들 아래에 위치하기 때문에 해당 view들을 고려하여 `child view controller`의 `safe area` 아래와 오른쪽의 inset을 설정하여야 합니다.

![](https://images.velog.io/images/ryan-son/post/733617d1-1d98-4a87-afed-5e974cea5b82/image.png)

아래 코드를 통해 `container view controller`의 `viewDidAppear(_:)` 메서드에서 커스텀 view들을 고려하여 `child view controller`의 `safe area`를 늘리는 방법을 확인하실 수 있습니다. view 계층에 view가 추가되기 까지 `safe area insets`가 정확하지 않으니 해당 메서드에서 수정 작업이 이루어져야 합니다.

```swift
override func viewDidAppear(_ animated: Bool) {
   var newSafeArea = UIEdgeInsets()
   // Adjust the safe area to accommodate 
   //  the width of the side view.
   if let sideViewWidth = sideView?.bounds.size.width {
      newSafeArea.right += sideViewWidth
   }
   // Adjust the safe area to accommodate 
   //  the height of the bottom view.
   if let bottomViewHeight = bottomView?.bounds.size.height {
      newSafeArea.bottom += bottomViewHeight
   }
   // Adjust the safe area insets of the 
   //  embedded child view controller.
   let child = self.childViewControllers[0]
   child.additionalSafeAreaInsets = newSafeArea
}
```

# 참고자료
- [Positioning Content Relative to the Safe Area](https://developer.apple.com/documentation/uikit/uiview/positioning_content_relative_to_the_safe_area) - Apple Developer