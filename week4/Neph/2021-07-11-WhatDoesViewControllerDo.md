---
layout: post
title: "UIViewController"
image:
categories: iOS
tags: 
  - UIResponder
sitemap:
  changefreq: daily
  priority: 1.0
---

## UIViewController의 역할

View Controller는 앱 내부 구조의 기반으로 모든 앱은 최소 하나 이상의 뷰컨트롤러를 가진다. 뷰컨트롤러는 UI, 인터페이스, 데이터간의 상호작용을 맡으며 UI간의 전환에도 도움을 준다.



### View의 계층 관리

각각의 View Controller는 View의 계층을 관리한다. 이 계층의 가장 근원이 되는 것이 root view이며 모든 view controller는 한개의 root view를 가진다. 



### View와 관련된 Notification 관리

ViewController 객체는 UIView 객체들의 생성, 소멸, 이벤트 발생 등을 관리한다. View는 나타나기 직전, 나타난 후, 사라지기 직전, 사라진 후 4단계에 걸쳐서 UIViewController에게 내용을 전달한다.



### User Interaction 처리

뷰컨트롤러는 [responder objects](https://developer.apple.com/library/content/documentation/General/Conceptual/Devpedia-CocoaApp/Responder.html#//apple_ref/doc/uid/TP40009071-CH1)로서 responder chain으로부터 내려오는 이벤트를 핸들링할 수 있지만 일반적으로 터치 이벤트를 직접 핸들링하지 않고 뷰가 대신 이벤트를 감지하여 델리게이트나 타겟 오브젝트(일반적으로 뷰컨트롤러)에 보고한다. 

따라서 대부분의 뷰컨트롤러에 대한 이벤트는 델리게이트 메서드나 액션 메서드([action methods](https://developer.apple.com/library/content/documentation/General/Conceptual/Devpedia-CocoaApp/TargetAction.html#//apple_ref/doc/uid/TP40009071-CH3)) 로 핸들링된다.



### ViewController의 두 가지 종류

View Controller는 두 가지 종류로 구분되는데 하나는 content view controller로 앱의 컨텐츠를 관리하는 기본 타입, 다른 하나는 컨테이너 뷰컨트롤러로 다른 뷰컨트롤러를 관리하는 뷰컨트롤러이다.



### View Management

| <img src="https://raw.githubusercontent.com/Neph3779/Blog-Image/forUpload/img/20210711192638.png" alt="image: ../Art/VCPG_ControllerHierarchy_fig_1-1_2x.png" style="zoom:50%;" /> | <img src="https://raw.githubusercontent.com/Neph3779/Blog-Image/forUpload/img/20210711192908.png" alt="image: ../Art/VCPG_ContainerViewController_fig_1-2_2x.png" style="zoom:50%;" /> |
| ------------------------------------------------------------ | ------------------------------------------------------------ |

뷰 컨트롤러의 가장 중요한 역할을 뷰 계층을 관리하는 것이다. 뷰컨트롤러는 모든 컨텐트를 감싸는 루트뷰를 갖고 있으며 디스플레이하고 싶은 다른 뷰를 추가할 수 있다. 뷰컨트롤러는 언제나 루트뷰의 레퍼런스를 소유하고 각 뷰는 서브뷰들의 스트롱 레퍼런스(strong references) 를 소유한다.



뷰컨트롤러의 뷰계층에서 다른 뷰에 접근하려면 아웃렛([outlets](https://developer.apple.com/library/content/documentation/General/Conceptual/Devpedia-CocoaApp/Outlet.html#//apple_ref/doc/uid/TP40009071-CH4)) 이 일반적인 방법이다. 뷰컨트롤러는 모든 뷰의 컨텐트를 관리하고 아웃렛은 뷰에 대한 레퍼런스를 저장한다. 아웃렛은 뷰가 스토리보드에 로드될 때 자동으로 실제 뷰 오브젝트에 연결된다.

컨텐트 뷰컨트롤러는 속한 모든 뷰를 직접 관리한다. 컨테이너 뷰컨트롤러는 직접 소유한 뷰와 차일드 뷰컨트롤러의 루트뷰를 관리한다. 컨테이너는 자식뷰의 컨텐트를 관리하지 않으며 오직 컨테이너의 디자인에 따라 루트뷰의 사이즈와 위치 등을 관리한다. 아래 스플릿 뷰컨트롤러(split view controller) 는 차일드뷰의 사이즈와 포지션을 관리하지만 차일드 뷰컨트롤러는 실제 컨텐츠를 관리한다.



### Data Marshing (데이터 중계)

![img](https://raw.githubusercontent.com/Neph3779/Blog-Image/forUpload/img/20210711194741.png)

뷰컨트롤러는 관리하는 뷰와 데이터간의 중계자 역할을 수행한다. [UIViewController](https://developer.apple.com/documentation/uikit/uiviewcontroller) 의 메서드와 프로프터로 앱의 비주얼을 관리할 수 있고 서브클래스의 데이터를 관리할 수 있는 변수를 추가할 수 있다. 커스텀 변수를 추가하는 것은 아래 그림과 같은 관계를 만들어 내는데, 뷰컨트롤러는 데이터와 그것을 나타내는 뷰의 레퍼런스를 소유한다.

개발자는 뷰컨트롤러와 데이터 오브젝트간의 책임을 명확하게 분리해야 한다. 뷰컨트롤러는 뷰로부터 얻는 인풋을 검증하고 데이터 오브젝트가 필요로 하는 포맷으로 패키징한다. 하지만 실제 데이터를 관리함에 있어서 뷰컨트롤러의 역할은 최소화되어야 한다.

[UIDocument](https://developer.apple.com/documentation/uikit/uidocument) 오브젝트는 데이터를 뷰컨트롤러로부터 분리하여 관리하는 방법중 한가지이다. 그것은 컨트롤러 오브젝트로서 데이터를 영구 저장소로부터 읽고 쓰는 방법을 알고 있다. 서브클래싱하면 데이터를 추출하고 전달하는 로직이나 메서드를 추가할 수 있다. 뷰컨트롤러는 뷰의 업데이트가 용이하도록 데이터의 복사본을 저장하지만 도큐먼트를 원본데이터를 소유한다.



### Resource 관리

뷰컨트롤러는 자신 위에 올라오는 모든 뷰들을 관리한다. `didReceiveMemoryWarning` 메서드를 통해서 메모리 워닝이 발생했는지를 체크하고 뷰를 삭제함으로써 메모리 공간을 확보한다.



View Controller의 

- 기본 데이터의 변경에 대한 응답으로 뷰의 콘텐츠를 업데이트
- 뷰와 사용자 상호 작용에 응답
- 뷰 크기 조정 및 전체 인터페이스의 레이아웃 관리
- 앱에서 다른 뷰 컨트롤러를 포함한 다른 객체와 조정



### Adaptivity 관리

<img src="https://raw.githubusercontent.com/Neph3779/Blog-Image/forUpload/img/20210711195332.png" alt="image: ../Art/VCPG_SizeClassChanges_fig_1-4_2x.png" style="zoom:50%;" />

뷰컨트롤러는 속한 뷰의 환경에 따라 적응을 한다. iOS앱은 아이패드나 여러가지 아이폰에서 실행이 가능하다. 여러개의 뷰컨트롤러로 개별 기기를 지원하기보다는 하나의 뷰컨트롤러만 가지고 뷰의 사이즈를 조절한다. 뷰컨트롤러의 traits, size class(compact, regular)가 바뀌면 그에 맞게 컨텐츠를 배치해준다.



## 참고한 링크

https://ahyeonlog.tistory.com/17

https://sibalja.tistory.com/25

http://www.appleofeyes.com/role-view-controllers-xcode-%EC%97%91%EC%8A%A4%EC%BD%94%EB%93%9C%EC%97%90%EC%84%9C-%EB%B7%B0%EC%BB%A8%ED%8A%B8%EB%A1%A4%EB%9F%AC%EC%9D%98-%EC%97%AD%ED%95%A0/