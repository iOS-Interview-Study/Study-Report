---
layout: post
title: "App thining"
image:
categories: iOS
tags: 
  - App thining
sitemap:
  changefreq: daily
  priority: 1.0
---

## App Thinning이란?

앱을 특정 디바이스에 설치할 때 굳이 들어가지 않아도 되는 파일들을 생략하고 그 디바이스에 맞게 설치하는 최적화 기술을 App Thinning이라고 한다. 앞의 문장에서 알 수 있듯 하나의 앱은 아래와 같이 여러가지 파일들이 섞여있다.

<img src="https://raw.githubusercontent.com/Neph3779/Blog-Image/forUpload/img/20210725190835.png" alt="?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FccHkki%2FbtqMNwMWH1E%2F9ECfXenzBbiitySkTphFJ0%2Fimg" style="zoom:50%;" />



### App Slicing

꼭 필요한 것만을 다운 받는 과정을 App Slicing이라고 한다.

App Slicing을 통해 필요한 것만을 다운받으면 아래와 같은 모양새가 된다.

<img src="https://raw.githubusercontent.com/Neph3779/Blog-Image/forUpload/img/20210725190924.png" alt="?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FcyHPmL%2FbtqMTgvf1NB%2F8TDe7i1f9aNbIRTs57JsIk%2Fimg" style="zoom:50%;" />

즉, 개발자가 앱의 전체 버전을 App store에 업로드하면 앱 스토어는 각 디바이스의 특성에 맞는 다양한 버전의 조각들 (별도의 .ipa 파일들)을 생성한다. 사용자가 앱을 실제로 설치할 때는 앱의 전체 버전이 아닌 slicing된 조각들 중 사용자에게 가장 적합한 조각이 설치된다. 그리고 이 App Slicing은 앱스토어가 알아서 처리해준다.

<img src="https://raw.githubusercontent.com/Neph3779/Blog-Image/forUpload/img/20210725191103.png" alt="?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fcr1BBh%2FbtqMOyQ0RXl%2FmnpwXDMpNO96k7wNCxOP0k%2Fimg" style="zoom:50%;" />



### On Demand Resource (주문형 리소스)

유저가 요청할 때 앱스토어로부터 더 많은 리소스를 가져올 수 있는 개념이다. 보통 게임에서 많이 찾아볼 수 있는데 게임 내의 sound를 유저가 선택하여 다운받을 수 있도록 해주는 경우가 많다.

주문형 리소스는 앱스토어에 IPA파일과는 별도로 저장된다. 



<img src="https://raw.githubusercontent.com/Neph3779/Blog-Image/forUpload/img/20210725191517.png" alt="?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbOsCEz%2FbtqMX9I76tB%2FLuYzKxGDVccPRsInfkp3kk%2Fimg" style="zoom:50%;" />

앱의 전체 버전에는 위와 같이 전체 리소스가 올라간다.

이 중에서 일부만을 유저가 받게하고 나머지는 필요시에만 받도록 하는 것이 ODR이다.



#### ODR의 생명주기

![img](https://raw.githubusercontent.com/Neph3779/Blog-Image/forUpload/img/20210725192612.png)



### 비트코드

기계언어로 번역되기 이전 단계의 중간표현(Intermediate Representation)을 비트코드라 한다. 프로젝트 옵션에서 사용여부를 선택할 수 있다(디폴트 on). 

비트코드를 사용하여 앱을 업로드를 하면 애플리케이션을 재컴파일하여 앱 바이너리(app binary)를 생성한다. 즉, 최신 컴파일러용에 맞게 자동으로 앱을 컴파일하고, 특정 아키텍처에 맞게 최적화한다.

반면 비트코드를 사용하지 않으면,  모든것을 실행할 수 있음 + 모든 환경이라는 조건을 갖춘 바이너리(여러개)를 생성하여 이를 하나로 합친 fat binary가 업로드된다.

비트코드를 사용하면 필요 경우에 맞게 재컴파일하게 되므로 최적화가 가능한 것이다.

 

비트코드 사용시 개발자 입장에서 단점이 생기는데 디버깅할 때 필요한 dSym 파일을 따로 받아야 한다는 것이다.

비트코드를 통해 애플은 앱을 "재컴파일"하여 유저에게 새로운 바이너리를 제공하기 때문에, XCode에서 내가 올린 바이너리와 실제 사용자가 받는 바이너리가 다르다.

따라서 개발자가 크래쉬에대한 정보를 보려면, App Store Connect에서 개발자가 올린 앱의 dysm파일을 따로 다운로드하여 크래쉬 분석 툴에 올려줘야한다.



### 참고링크

- [https://help.apple.com/xcode/mac/current/#/devbbdc5ce4f](https://help.apple.com/xcode/mac/current/#/devbbdc5ce4f)
- [https://hyunndyblog.tistory.com/151](https://hyunndyblog.tistory.com/151)
- [https://blog.naver.com/PostView.naver?isHttpsRedirect=true&blogId=horajjan&logNo=220585718693](https://blog.naver.com/PostView.naver?isHttpsRedirect=true&blogId=horajjan&logNo=220585718693)

