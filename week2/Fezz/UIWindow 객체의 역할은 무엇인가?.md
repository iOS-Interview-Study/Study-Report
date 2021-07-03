# Week 2 - Fezz

## UIWindow 객체의 역할은 무엇인가?

<img src="/Users/yjaewoongnaver.com/Desktop/스크린샷 2021-07-04 오전 1.10.08.png" alt="스크린샷 2021-07-04 오전 1.10.08" style="zoom:50%;" /> 

<img src="https://raw.githubusercontent.com/Fezravien/UploadForMarkdown/forUpload/img/c05af6a2-c616-482b-8f65-98013d40bb05.png" alt="window" style="zoom:55%;" /> 

<br> 

#### 앱의 UI 배경과 이벤트를 뷰로 전달하는 객체

Window는 뷰 컨트롤러와 함께 이벤트를 처리하고 앱 작동에 필수적인 많은 작업을 수행한다.

Window는 시각적 모양은 없고 Root View Controller에서 관리하는 하나 이상의 뷰를 보여주는 역할을 합니다.

<img src="https://raw.githubusercontent.com/Fezravien/UploadForMarkdown/forUpload/img/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202021-07-04%20%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB%201.16.18.png" alt="스크린샷 2021-07-04 오전 1.16.18" style="zoom:60%;" /> 

> **UIWindow는 UIView의 하위클래스이다.** 

<br>

#### Window를 사용하는 경우

- 앱의 콘텐츠를 표시할 기본 window를 제공합니다.
- 추가 콘텐츠를 표시할 때 필요한 경우 추가 window를 만듭니다.

<br>

#### UIWindow 객체는 아래의 작업에도 사용할 수 있다.

- Window의 가시성에 영향을 주는 z 좌표를 설정한다. 이는 다른 Window와 관련된 Window 가시성(visibility)에 영향을 미친다. 
- Window를 표시하고 키보드 이벤트의 대상으로 만든다.
- Window의 좌표계 간에 좌표값을 변환한다.
- Window의 root view controller 변경한다.
- Window에 표시되는 화면 변경한다.

<br>

---

## ⚠️ 정리

### UIWindow

UIWindow는 자체적으로 보이는 컨텐츠를 제공하진 않지만 모든 iOS앱이 기본적으로 가지고 있으며 앱의 화면 컨텐츠에 대한 컨테이너 역할을 한다. 대부분의 앱에는 한개의 `window` 만 필요하다. 스토리보드를 사용한다면 자동으로 기본 window를 포함한 템플릿을 제공한다. 하지만 앱에서 스토리 보드를 사용하지 않고 프로그래밍 방식을 사용한다면 window를 직접 만들어야 한다. 그리고 window의 rootViewCotroller속성을 사용해 직접 루트 뷰 컨트롤러를 지정해야 한다.

대부분의 앱은 기기의 메인 스크린에 컨텐츠를 표시할 하나의 window만 필요하다. 추가적으로 만들 순 있지만 보통은 연결된 또다른 외부의 스크린에 컨텐츠를 표시해야할 때에 만든다. window는 크게 다음과 같은 역할을 한다.

1. 터치 이벤트를 올바른 대상으로 전달한다. (좌표값이 없는 경우에는 키윈도우로 이벤트 전달)
2. 뷰의 컨테이너 역할을 해준다. 따라서 뷰는 윈도우에 추가되어야 나타난다.
3. 화면을 전환할 때에는 새로운 윈도우를 생성하는 것이 아닌 윈도우에 추가 되어 있는 뷰를 교체하는 방식으로 사용한다.

<br>

### UIView

화면의 직사각형 영역에 대한 컨텐츠를 관리하는 개체

1. 자신에게 주어진 프레임 내의 시각적인 요소들을 출력한다.
2. 프레임 내에서 발생한 이벤트를 처리한다.
3. 자신에게 포함된 뷰(Subview)를 관리한다.

뷰를 다른 뷰 안에 포함하여 계층 구조를 만들 수 있다. 뷰를 중첩하면 자식 뷰(서브 뷰)와 부모 뷰(슈퍼 뷰) 간에 부모-자식 관계가 생성된다. 슈퍼 뷰에서는 여러 서브 뷰가 포함될 수 있지만 서브 뷰는 슈퍼 뷰가 하나만 있다. 기본적으로 서브 뷰의 영역이 슈퍼 뷰의 경계 밖으로 나갔을 때 서브 뷰의 내용이 잘리지 않는다. 이런 동작을 변경하려면 `clipsToBounds` 속성을 사용한다.

각 뷰는 `frame`과 `bounds`로 정의된다. `frame`은 슈퍼 뷰의 좌표 계에서 현재 뷰의 위치를 정의한다. `bound`는 뷰의 내부 좌표를 정의한다. 또한 뷰의 좌표와 크기를 묶어서 `frame` 이라고 한다. 뷰에서 컨텐츠를 표기할 때에는 뷰의 지역좌표, 뷰의 위치를 지정할 때에는 슈퍼뷰의 지역좌표를 기준으로 그린다.

<br>

---

- CloudNote 예시

```swift
// CloudNote
// SceneDelegate
func scene(_ scene: UIScene, 
           willConnectTo session: UISceneSession, 
           options connectionOptions: UIScene.ConnectionOptions) {
    
        guard let windowScene = (scene as? UIWindowScene) else { return }
        window = UIWindow(windowScene: windowScene)
        let mainViewController = NoteSplitViewController(style: .doubleColumn)
        
        window?.rootViewController = mainViewController
        window?.makeKeyAndVisible()
}
```

<br>

---

참고 

[UIView와 UIWindow](https://ahyeonlog.tistory.com/16)

[UIWindow](https://developer.apple.com/documentation/uikit/uiwindow)

