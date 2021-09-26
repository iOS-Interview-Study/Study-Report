# Week13 - Fezz

## Storyboard interface builder의 컴파일 과정이 어떻게 이루어지는지.



### 앱의 기본 구조

C 언어에 뿌리를 둔 모든 애플리케이션은 `main()` 함수로부터 시작된다. 

이를 `엔트리 포인트(Entry Point, 시작 진입점)` 이라 한다.

운영체제가 애플리케이션 내부에 정의된 `main()` 함수를 찾아 호출하면 

여기에 작성된 코드들이 연쇄적으로 실행되면서 우리가 작성해 둔 커스텀 코드에까지 도달하게 된다.

<br>

`Obj-C` 역시 C 언어에 기반하고 있기 때문에, 이를 이용하여 만든 iOS 앱도 `main()` 함수로부터 시작된다.

C 기반의 다른 애플리케이션과 차이점이라면 iOS 앱에서는 main() 함수를 우리가 직접 작성하지 않는다는 것 이다.

대신 Xcode 프로젝트를 생성하면 main() 함수가 자동으로 만들어지는데, 

여기에는 iOS 앱이 실행될 때 처리해야 할 내용이 작성되어있기 때문에 main() 함수를 건드릴 필요가 없다.

<br>

main() 함수가 하는 일은 단순한대, 

실행 시 시스템으로부터 전달받은 두 개의 인자값과 `AppDelegate` 클래스를 이용하여 `UIApplicationMain()` 함수를 호출하고,

그 결과로 `UIApplication` 객체를 반환한다.

생성된 `UIApplication` 객체는 `UIKit` 프레임워크에 속해 있으므로 이후의 앱 제어권은 UIKit 프레임워크로 이관된다.

<br>

`UIApplicationMainMain()` 함수가 생성하는 `UIApplication`은 앱의 본체라고 할 수 있는 객체로 사실상 앱 그 자체를 의미한다.

이 클래스를 특별한 일이 있거나 중대한 목적이 있는 경우가 아니라면 서브 클래싱 없이 그대로 사용하는데, 

굳이 서브 클래싱할 필요도 없고, 하기도 어렵기 때문이다.

그런데 달리 생각해보면 `UIApplication` 객체를 서브 클래싱하지 않고 그대로 사용하는 것에는 한계가 존재하는데,

의도와 목적에 맞게 특별히 처리해야 할 것도 있을 수 있기 때문이다.

그래서 `UIApplication` 객체는 `AppDelegate`라는 대리 객체를 내세우고 커스텀 코드를 처리할 수 있도록 약간의 권한을 부여한다.

<br>

`AppDelegate`는 `UIApplication`으로 부터 위임받은 일부 권한을 이용하여 커스텀 코드와 상호작용하는 역할을 담당하고, 

이를 통해 필요한 코드를 구현할 수 있도록 돕는다.

AppDelegate 객체는 iOS 애플리케이션 내에서 오직 하나의 인스턴스만 생성되도록 시스템적으로 보장받는다.

앱이 처음 만들어질 때 객체가 생성되고, 앱이 실행되는 동안 계속 유지되다가 앱이 종료되면 그때 함께 소멸하는 등 앱 전체의 생명 주기와 함게 한다.

<br>

#### `UIApplication` 객체와 `AppDelegate` 객체가 연관되어 앱이 실행되는 전체 과정

1. `main()` 함수가 실행된다.
2. `main()` 함수는 다시 `UIApplicationMain()` 함수를 호출한다.
3. `UIApplicationMain()` 함수는 앱의 본체에 해당하는 `UIApplication` 객체를 생성한다.
4. `UIApplication` 객체는 `Info.plist` 파일을 바탕으로 앱에 필요한 데이터와 객체를 로드한다.
5. `AppDelegate` 객체를 생성하고 `UIApplication` 객체와 연결한다.
6.  이벤트 루프를 만드는 등 실행에 필요한 준비를 진행한다.
7. 실행 완료 직전, 앱 델리게이트의 `application(_:didFinishLaunchingWithOptions:)` 메소드를 호출한다.

<img src="https://raw.githubusercontent.com/Fezravien/UploadForMarkdown/forUpload/img/8oeoizs.png" alt="Imgur" style="zoom: 67%;" />

<br>

---

📄 참고

[도서] 꼼꼼한 재은씨의 Swift 기본편