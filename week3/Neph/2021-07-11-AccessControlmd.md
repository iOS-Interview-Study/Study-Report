---
layout: post
title: "접근 제어자의 종류"
image:
categories: iOS
tags: 
  - 접근 제어 지정자
sitemap:
  changefreq: daily
  priority: 1.0
---

## Module이란?

접근제어에 대해 이해하기 위해서는 모듈의 개념에 대해 알아야한다.

모듈은 코드의 묶음 단위로 프레임워크, 라이브러리, 어플리케이션처럼 배포할 코드들의 묶음을 나타낸다. 즉, 하나의 프레임워크는 하나의 모듈이고 일반적으로 만드는 프로젝트도 역시 하나의 모듈이다. import를 통해 모듈을 사용할 수 있다. 프로젝트 하위에 위치한 target들도 하나의 모듈에 해당한다.



## Access Level

### open

다른 모듈에서 사용할 수 있음을 넘어 sub classing까지도 가능한 접근제어자

### public

프로젝트 내의 모든 module의 entity에 접근할 수 있는 접근제어자

### internal

default값으로 지정되어있는 접근제어자로 entity가 작성된 module에서만 접근할 수 있는 접근제어자

### fileprivate

entity가 작성된 source file내에서만 접근할 수 있는 접근제어자

### private

특정 객체에서만 사용할 수 있도록 지정한 가장 제한적인 접근제어자



## 기타

### 제한적 setter (private setter)

```swift
private(set)
fileprivate(set)
```

과 같은 문법을 적용할 수 있다.



### 접근제어자간의 충돌

public으로 선언되어있는 객체의 멤버는 open으로 선언될 수 없다.

즉, 바깥의 접근제어 수준보다 높은 수준의 내부 요소는 존재할 수 없다. 

특정 접근 제어 수준의 타입이 함수의 매개변수나 반환되는 타입일 경우 함수는 해당 값의 접근제어보다 높을 수 없다.



## 참고한 링크

https://hcn1519.github.io/articles/2018-01/Swift_AccessControl

https://baked-corn.tistory.com/80