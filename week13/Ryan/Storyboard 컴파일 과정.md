# Storyboard 컴파일 과정
> - ibtool이 storyboard 파일을 storyboardc 파일로 컴파일 (c: compiled).
> - storyboardc 파일은 view와 view의 관계 (IBAction, Segue 등)가 정의된 스토리보드를 바이너리 파일인 nib로 컴파일하며 스토리보드 내 view들의 고유 NIB 식별자 등의 정보를 Information Property List에 기록

## 스토리보드 소스 파일
Xcode를 통해 확인할 수 있는 스토리보드 파일의 구성은 아래와 같습니다.

### Interface Builder - Storyboard
스토리보드 파일을 클릭하면 볼 수 있는 흔한 화면 구성입니다. 화면과 화면 간의 관계를 시각적으로 표현하고 있어 화면 구성을 이해하기 용이합니다.

![](https://images.velog.io/images/ryan-son/post/c21395b9-595e-42f1-a9d9-c3efb8e7bde6/image.png)

### Source Code
스토리보드 파일을 우클릭하여 `Open As`, `Source Code`을 순차적으로 클릭하며 진입하면 다음과 같이 `XML` 형식으로 표현된 스토리보드를 확인하실 수 있습니다. 스토리보드의 내용을 수정하여 커밋하면 이 소스코드가 변경된다는 것을 gif diff 명령을 통해 확인하신 적이 있으실 것입니다.

![](https://images.velog.io/images/ryan-son/post/4d1e07f3-f155-4662-87d5-fbd8da5d50e7/image.png)

### 번외 - XIB
`XML Interface builder` 또는 `Xcode Interface builder`라고 불리는 XIB 파일도 마찬가지로 `XML` 형식으로 작성되어 있습니다. 스토리보드의 document type이 `"com.apple.InterfaceBuilder3.CocoaTouch.Storyboard.XIB"`, XIB 파일의 document type이 `"com.apple.InterfaceBuilder3.CocoaTouch.XIB"`으로 둘 모두 유사한 파일 형식임을 유추할 수 있습니다.

![](https://images.velog.io/images/ryan-son/post/7b83389b-896d-40fe-b29a-a1fa55e2ed2d/image.png)

## Build phase
빌드 완료 시 `XIB` 파일은 바이너리 형식으로 변환되어 Next step Interface Builder를 의미하는 `NIB` 파일로 컴파일됩니다. 바이너리 형식이라 0과 1만 확인할 수 있을 것으로 기대했는데 16진법으로 표시하고 있네요 (i.e. `8c17` = `1000110000010111`). 어찌되었든 사람이 읽고 이해할 수 있는 형태를 벗어났습니다. `NIB` 형식으로는 git과 같은 방식으로 소스 컨트롤을 하기 제한되어 `XML` 형식의 `XIB`가 도입되었다고 하네요.

![](https://images.velog.io/images/ryan-son/post/076e775b-86dd-4bd8-b3f8-cfe52a3cf2ba/image.png)

출처: [달망이 개발일기](https://dalgonakit.tistory.com/82)

빌드 후 앱 번들 패키지를 열어보시면 아래와 같이 구성되어 있는 것을 확인하실 수 있습니다. 개별 XIB로 작업한 내용은 스토리보드와 관계없이 NIB 파일로 컴파일되어 앱 번들 최상위 폴더에서 확인할 수 있네요.

![](https://images.velog.io/images/ryan-son/post/171c3096-da0e-459c-8c68-68e5a01c8faa/image.png)

다음은 메인 소스가 위치할 것으로 예상되는 `Base.lproj`를 열었을 때 확인할 수 있는 구성입니다. 스토리보드 파일이었던 `LaunchScreen`과 `Main`이 있네요.

![](https://images.velog.io/images/ryan-son/post/fafedbb8-9305-4186-b4ed-29804a57913e/image.png)

미리 말씀드렸듯이 컴파일된 `storyboardc` 패키지는 view와 view의 관계 (IBAction, Segue 등)로 이루어진 NIB 파일들을 가지고 있습니다.

![](https://images.velog.io/images/ryan-son/post/db398ff4-4c53-4f9c-8d22-03ca09c22d08/image.png)

의미 없어보이는 `BYZ-38-t0r-view-8bC-Xf-vdC`와 같은 문자열도 의미가 있습니다. 다음 코드 일부를 통해 특정 ViewController의 최상위 view의 id를 지칭하는 것임을 파악할 수 있습니다.

마찬가지로 스토리보드의 요소인 `UINavigationController`, `UIViewController`들도 모두 무작위처럼 보이는 `id`를 키값을 가지고있으며 이러한 내용이 `storyboardc` 파일에 동봉된 `info.plist` 프로퍼티 리스트를 통해 확인할 수 있습니다.

```swift
// Main.storyboard
...
<viewController id="BYZ-38-t0r" customClass="OpenMarketItemListViewController" customModule="OpenMarket" customModuleProvider="target" sceneMemberID="viewController">
    <view key="view" contentMode="scaleToFill" id="8bC-Xf-vdC">
...
```

![](https://images.velog.io/images/ryan-son/post/b95aeace-9be6-4552-86c4-b03644bac92e/image.png)


# 참고자료

- [What does ibtool do in the compiling and linking storyboards phase and how the result of linked storyboards work with executable?](https://stackoverflow.com/questions/42043047/what-does-ibtool-do-in-the-compiling-and-linking-storyboards-phase-and-how-the-r) - Stack overflow
- [iOS의 화면을 그리기 위해선 이것이 필요합니다! XIB, NIB, Storyboard](https://dalgonakit.tistory.com/82) - 달망이 개발일기
- [Nib 파일로부터 UI 관련 객체를 로딩하기](https://soooprmx.com/nib-파일로부터-ui-관련-객체를-로딩하기/#fn-7226-2-2) - Wireframe blog