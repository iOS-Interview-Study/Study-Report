---
layout: post
title: "UIWindow의 역할"
image:
categories: 
tags: 
  - 
sitemap:
  changefreq: daily
  priority: 1.0
---

## UIWindow란?

UIWindow는 UIView의 하위 클래스로 앱의 배경과 이벤트를 View로 전달하는 객체이다.

눈에 보이는 내용은 없지만 앱의 View에 기본 컨테이너를 제공하여 하며 rootViewController에서 관리하는 하나 이상의 View를 보여주는 역할을 한다. 

또한 터치 이벤트를 View와 다른 어플리케이션 객체들에게 전달하는데, 터치 이벤트는 발생한 window로 전달되지만, 좌표값이 없는 이벤트는 키보드 이벤트, 터치와 관련되지 않은 이벤트를 받는 window인 key window로 전달된다. 

key window는 하나의 window로만 구성되며, isKeyWindow 프로퍼티를 이용해 key window인지 아닌지를 알 수 있다. 대부분 app의 메인 windonw가 key window이다. 

iOS 12 까지는 하나의 window만 사용할 수 있었지만 iOS 13부터는 Scene Delegate을 활용해 여러개의 window들을 사용할 수 있게 되었다.



Window는 foreground일 때뿐만 아니라 background일 때도 사용해야한다. foreground일때는 makeKeyAndVisible() 메서드를 통해 window를 보여주고 해당 window를 key window로 만들어준다.

![?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FGqjjH%2FbtqNF9btI9V%2FOnIOQkbfrEwDZEEu7m80Gk%2Fimg](https://raw.githubusercontent.com/Neph3779/Blog-Image/forUpload/img/20210704150620.png)



## window 객체를 만드는 경우 & 방법

### 만드는 경우

- 스토리보드를 사용하지 않는 경우에 main window를 직접 생성해야 한다.
- 외부 디스플레이를 사용하는 경우 새로운 window를 만들어서 보여줄 수 있다.



### 만드는 방법

메인 window 객체는 코드와 인터페이스 빌더 중 하나로 생성할 수 있다. 이 때 lazily create을 통해 필요할 때 생성되도록 해야한다. 

AppDelegate나 SceneDelegate에서 UIWindow의 instance(window)를 프로퍼티로 넣어준 뒤 window의 rootViewController를 설정해주면 된다.



## UIWindow를 상속받아 사용하는 경우

매우 드문일이지만 위의 메서드를 재정의해서 사용하고 싶은 경우 UIWindow의 subclass를 제작할 수 있다. becomeKey()나 resignKey()에 추가적인 동작을 명시하는 것이 대부분이다.



#### 참고한 링크

- https://wnstkdyu.github.io/2017/12/29/uiwindow/
- https://zeddios.tistory.com/283
- https://developer.apple.com/documentation/uikit/uiwindow