---
layout: post
title: "App Bundle의 구조와 역할"
image:
categories: iOS
tags: 
  - iOS
sitemap:
  changefreq: daily
  priority: 1.0
---

## Bundle vs Package

### 번들

알려진 것들로 이루어진 디렉토리, 실행 가능한 코드와 그 코드가 사용하는 자원들을 포함하고 있다.

코드와 자원을 모으는 구조를 제공하여 개발자 경험을 향상시키는 것을 가장 우선시 하는데, 이 구조는 코드나 자원의 예측 가능한 로딩뿐만 아니라 지역화 같은 시스템 차원의 기능도 허용한다. 



번들은 크게 `앱 번들`, `프레임워크 번들`,  `Loadable 번들`로 나눌 수 있다.

`앱 번들`은 실행될 수 있는 executable과 그 executable을 설명하는 Info.plist 파일 그리고 executable에서 사용되는 launch 이미지를 포함한 asset과 자원, 인터페이스 파일, string 파일, 데이터 파일로 이루어져 있다.

`프레임워크 번들`은 dynamic shared library 라 불리는 동적 공유 라이브러리에서 사용되는 코드와 자원을 포함하고 있다.

`Loadable 번들`은 앱의 기능성을 확장시켜주는 실행 가능한 코드와 자원을 포함하고 있고 플러그인이 대표적인 예시이다.



번들의 컨텐츠는 `Bundle.main`을 사용해서 접근할 수 있으며 대부분의 경우 `url(forResource:withExtension:)`메서드를 사용하여 특정 자원의 위치를 알아낼 수 있다. 

```swift
Bundle.main.url(forResource: "Photo", withExtension: "jpg")
```



모든 앱 번들은 앱에 대한 정보가 담긴 Info.plist 파일을 가지며 bundleURL과 bundleIdentifier를 포함한 몇몇 메타 데이터는  아래와 같이도 접근할 수 있다.

```swift
let bundle = Bundle.main

bundle.bundleURL        // 앱의 저장위치
bundle.bundleIdentifier // identifier
```





### 패키지

파인더를 통해 봤을 때 파일처럼 보이는 디렉토리이다. 패키지는 관련있는 자원들을 하나의 유닛으로 압축시키고 연결시키는 작업을 통해 사용자 경험을 향상하기 위해 만들어졌다.



- 디렉토리에 .app, .playground, .plugin과 같은 특별한 확장자를 가지고 있는 파일이 있는 경우
- 디렉토리에 document 타입으로 등록된 앱의 확장자가 존재하는 경우
- 디렉토리에 그 자체를 패키지로 보여지게 하는 확장된 attribute가 존재하는 경우

위 세 가지의 경우에 해당 디렉토리를 패키지라고 생각할 수 있다.



### 정리

패키지는 누군가 봉인해둔 하나의 박스라면 번들은 백팩과 같은 느낌이다. 



<img src="https://raw.githubusercontent.com/Neph3779/Blog-Image/forUpload/img/20210711145044.svg" alt="img" style="zoom:50%;" />

출처: https://nshipster.co.kr/bundles-and-packages/



## App Bundle이란?

앱 번들은 개발자가 생성하는 가장 일반적인 유형의 번들로, 어플리케이션의 성공적인 작동에 필요한 모든 것들을 저장한다. 구조는 어플리케이션 플랫폼이 iOS냐 MacOS냐에 따라 다르지만, 사용하는 방법은 동일하다.



## 앱 번들의 구성요소

앱 번들에는 Info.plist, 실행파일, 리소스 파일, 기타 서포트 파일 등이 포함되어있는데 각각 아래와 같은 역할을 수행한다.

### Info.plist

어플리케이션에 대한 구성 정보가 들어있는 구조화된 파일로, 시스템은 이 파일에 의존하여 어플리케이션 및 파일에 대한 관련 정보를 식별한다.

### 실행파일

모든 응용 프로그램에 존재하는 실행파일로 어플리케이션의 메인 entry point와 어플리케이션 타겟에 정적으로 연결된 모든 코드가 포함되어 있다.

### 리소스 파일

리소스는 어플리케이션의 실행 파일 외부에 있는 데이터 파일이다. 리소스는 일반적으로 이미지, 아이콘, 소리, nib파일, 문자열 파일, 설정 파일 및 데이터 파일 등으로 구성된다. 대부분 localized될 수 있다.

### 기타 서포트 파일

커스텀 데이터 리소스 등이 포함 되어있다.



iOS의 번들 구조를 이해하면 사용자 지정 파일을 저장할 위치를 결정하는데 도움이 된다. iOS 어플의 번들 구조는 모바일 장치에 적합하게 이루어져 있다. 외부 디렉토리가 거의 없는 평평한 구조를 사용하여 디스크 공간을 절약하고 파일에 대한 접근과정을 간소화한다.





## 참고한 링크

https://nshipster.co.kr/bundles-and-packages/

https://melod-it.gitbook.io/sagwa/documentation-archive/bundle-programming-guide/bundle-structures

https://ios-development.tistory.com/339

https://hcn1519.github.io/articles/2018-12/bundle

https://developer.apple.com/library/archive/documentation/CoreFoundation/Conceptual/CFBundles/AboutBundles/AboutBundles.html#//apple_ref/doc/uid/10000123i-CH100-SW1

https://woongsios.tistory.com/92

https://developer.apple.com/documentation/foundation/bundle

https://www.notion.so/App-Bundle-650ff04e917c4ef4980237d4f68f09a3

https://github.com/jwonyLee/TIL/blob/master/iOS/Interview/AppBundle.md