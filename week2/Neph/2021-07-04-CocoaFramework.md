---
layout: post
title: "Cocoa Framework"
image:
categories: iOS
tags: 
  - Cocoa
sitemap:
  changefreq: daily
  priority: 1.0
---

## 프레임워크는 vs 라이브러리

> **"소프트웨어의 구체적인 부분에 해당하는 설계와 구현을 재사용이 가능하게끔 일련의 협업화된 형태로 클래스들을 제공하는 것"**
>
> by Ralph Johnson



프레임워크는는 어떤 틀이 결정되어있고 그 틀에 따라 개발을 진행하게 되지만 라이브러리는 원하는 부분을 선택해서 사용한다는 느낌이 강하다. 

라이브러리는 필요한 상황에서 메서드를 뽑아쓸 수 있도록 구성되어있지만 프레임워크는는 개발자가 자신의 어플리케이션을 작성할 수 있도록 미완성인 형태로도 정의되어있다.

프레임워크는 뼈대, 라이브러리는 살 정도로 정리할 수 있다.



## Cocoa Framework

코코아 프레임워크는 다음과 같이 구성되어있다.

![?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FYM2lL%2Fbtqt848k34Z%2FsTK8MVB0fa6YMpeKzYovc1%2Fimg](https://raw.githubusercontent.com/Neph3779/Blog-Image/forUpload/img/20210704115012.png)

하위 계층으로 갈수록 하드웨어에 가깝고 상위 계층으로 갈수록 유저에게 가깝다. 

#### Cocoa

자신보다 하위 계층의 프레임워크를 사용하여 어플리케이션을 직접 구현하는 프레임워크

UIKit, GameKit, MapKit 등이 속해있다.

#### Media

상위 계층이 코코아 터치에 그래픽, 멀티미디어 관련 서비스를 제공한다.

Core Graphics, Core Text, Core Audio, Core Animation, AVFoundation 등이 속해있다.

#### Core Services

문자열 처리, 데이터 집합 관리, 네트워크 통신, 주소록 관리, 환경 설정 등 핵심적인 서비스들을 제공한다. 또한 GPS, 나침반, 가속도 센서, 자이로스코프 센서와 같이 디바이스의 하드웨어적 특성에 기반한 서비스도 제공한다.

Foundation, Core Foundation, Core Location, Core Motion, Core Animation, Core Data 등이 속해있다.

#### Core OS

커널, 파일 시스템, 네트워크, 보안, 전원 관리, 디바이스 드라이버 등이 포함되어있다.

iOS 운영체제의 핵심이 되는 부분이다.



## Cocoa Touch Framework (Cocoa Framework)

코코아 터치 프레임워크은 다음과 같이 구성되어있다.

<img src="https://raw.githubusercontent.com/Neph3779/Blog-Image/forUpload/img/20210704110700.png" alt="img" style="zoom:50%;" />

코코아 터치 프레임워크는 앱 개발을 위해 사용하는 통합적인 것들을 칭하는 말이다. 여기에는 흔히 사용되는 UIKit과 Foundation이 포함되어있다.

Cocoa Framework의 경우에는 UIKit이 아닌 Appkit이 들어가있는데, 이는 MacOS에서 구동되는 앱을 개발하기위한 프레임워크이다.



## Foundation Framework

Foundation Framework에는 원시 자료형인 String / Int, 자료구조, 네트워크 통신, 파일 관리 등 기본적인 프로그램의 중심이 되는 것들이 들어있다. 

>  Objective-C의 자료형과는 별개로 Swift의 기본 자료형이 존재하기 때문에 Foundation을 Import하지 않더라도 Int, String등을 사용할 수 있다.

> NS는 NextStep의 약자이다.



## UIKit Framework

UIKit은 화면에 표시되는 내용들, 사용자와의 인터렉션, 화면의 구조의 관리 등을 담당한다. 스크린에 View를 출력하는 Window, 화면을 그리는 View, Window와 View를 연결하는 ViewController 등이 속해있다.

추가로 UIKit은 Foundation Framework를 내부적으로 import하고 있기 때문에 UIKit만 import해줘도 Foundation은 자동으로 딸려온다.

##### UIKit 계층도

<img src="https://raw.githubusercontent.com/Neph3779/Blog-Image/forUpload/img/20210704112245.png" alt="UIKit"  />





## Cocoa.. 이름의 유래

유명한 언어인 Java는 커피 원산지에서 이름을 따왔다. 이와 유사한 이미지를 줌과 동시에 어린아이도 할 수 있다는 뉘앙스를 풍기기 위해 "코코아"라는 단어를 사용하게 되었다고 한다.



#### 참고한 링크

- [믿고보는 소들이의 Cocoa 정리글](https://babbab2.tistory.com/51)
- https://etst.tistory.com/78
- [Framework vs Library 정리(강추)](https://jokergt.tistory.com/89)
- https://velog.io/@wan088/iOS-%ED%94%84%EB%A0%88%EC%9E%84%EC%9B%8C%ED%81%AC-CocoaTouch-Foundation-UIkit-sjjzdqmte4
- https://apple.fandom.com/wiki/Cocoa_(API)

