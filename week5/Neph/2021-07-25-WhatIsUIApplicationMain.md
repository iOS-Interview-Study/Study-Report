---
layout: post
title: "UIApplicationMain은 뭐하는 함수일까?"
image:
categories: 
tags: 
  - 
sitemap:
  changefreq: daily
  priority: 1.0
---

## UIApplication

UIApplication의 singleton instance는 앱의 시작시에 생성된다. 이 instance를 제작하는 함수가 바로 UIApplicationMain 함수이다. 이 외에도 App Delegate의 제작과 event cycle의 설정을 담당하는 역할을 가진다.



## UIApplicationMain

```swift
func UIApplicationMain(_ argc: Int32, 
                     _ argv: UnsafeMutablePointer<UnsafeMutablePointer<CChar>?>, 
                     _ principalClassName: String?, 
                     _ delegateClassName: String?) -> Int32
```

함수는 다음과 같이 생겼다. Swift는 C언어 기반의 언어이기 때문에 C의 main함수와 유사한 모양을 띄고 있는 것을 볼 수 있다. 

argc는 argument value의 수, argv는 argument value이다.

principalClassName은 UIApplication or 이를 상속받은 하위 클래스의 이름이다. singleton으로 생성될 클래스의 이름을 Stirng으로 받아서 사용한다.

delegateClassName은 App Delegate 클래스의 이름이다. pincipalClassName에서 UIApplication의 하위 클래스를 넘겨주었다면 여기서도 하위 클래스를 지정해줄 수 있다.

C의 main 함수처럼 반환값은 무의미하다. 

주요 클래스의 인스턴스를 생성하거나 주어진 클래스나 app delegate을 통해 delegate 인스턴스를 생성한다. 또한 메인 이벤트 루프, 어플리케이션 실행 루프, 프로세스 이벤트의 시작점을 설정한다. info.plist 파일에 NSMainNibFile Key나 값이 유효한 nib 파일 이름이 포함된 로드하려는 특정 nib파일을 지정한다면 이 함수를 통해 nib 파일을 로드된다.



### 참고링크

- https://developer.apple.com/documentation/uikit/1622933-uiapplicationmain/[](https://developer.apple.com/documentation/uikit/1622933-uiapplicationmain/)