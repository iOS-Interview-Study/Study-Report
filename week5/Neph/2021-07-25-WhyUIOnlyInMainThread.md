---
layout: post
title: "왜 UI작업은 main thread에서 해야할까?"
image:
categories: iOS
tags: 
  - main thread
  - UI
sitemap:
  changefreq: daily
  priority: 1.0
---

## UIKit은 Nonatomic

UIKit이 만약 atomic이었다면 thread safe를 보장해주는 block 메커니즘이 필요하다. 이는 성능의 저하를 야기하게 된다. 게다가 UIKit은 거대한 프레임워크이기 때문에 thread safe하게 디자인하는 것은 현실적으로 불가능하다. 

불가능함을 설명하는 몇가지 상황들

- 만약 뷰의 속성을 비동기적으로 변경하였다면, 이 변경사항들을 모았다가 동시에 처리할지, 그때그때 스레드별로 처리할지 정할 수 없다.
- 스레드별로 view에 대한 서로 다른 처리를 지시했을 때 어떤 명령을 먼저 처리할지에 관한 문제를 해결할 수 없다.



##  Runloop와 View Drawing Cycle

UIApplication은 main thread에서 Main Run Loop라 불리는 런루프를 생성한다. 앱 내에서 발생하는 대부분의 이벤트를 관장한다. 이 Main Run Loop를 통해 스크린의 내용이 refresh될 수 있다.

view의 변경은 즉시 일어나지 않는다. 이번 RunLoop의 마지막에 redraw하여 view가 변하게 되는데, 이러한 변경을 View Drawing Cycle이라 부른다.

이러한 Run Loop는 thread마다 가지고있기 때문에 만약 background thread에서 view의 변경이 가능하다면 view가 동시에 변해야하는 상황(화면의 회전 등)에서 view들이 동시에 변하지 않는 문제가 발생할 것이다.



## Rendering Process

<img src="https://raw.githubusercontent.com/Neph3779/Blog-Image/forUpload/img/20210725181859.jpeg" alt="1*soHoOFPSdKlbR9D1KvbUhw" style="zoom:50%;" />

### UIKit

모든 종류의 컴포넌트들을 가지고 있으며 유저 이벤트를 핸들한다. 하지만 랜더링과 관련된 코드는 들고있지 않다.

### Core Animation

draw의 책임을 지고 있다. 모든 view를 display하고 animate한다.

### OpenGL ES

2D, 3D 랜더링을 진행한다.

### Core Graphics

2D 랜더링을 진행한다.

### Graphics Hardware

GPU가 있는 영역이다.



![1*MHtDsFMpROhOF7yVwYVvCA](https://raw.githubusercontent.com/Neph3779/Blog-Image/forUpload/img/20210725182122.jpeg)

코어 애니메이션은 위의 파이프라인 방식을 통해 랜더링을 진행하며 이는 4단계로 나뉜다.

### 1. Commit Transaction

view를 레이아웃하고 이미지를 디코딩하여 Render Server에 이를 전달한다.

### 2. Render Server

Commit Transaction으로부터 받은 package를 분석하고 deserialize하여 rendering tree에 보낸다. 이후에 drawing instruction들을 생성하고 VSync Signal을 기다렸다가 화면을 랜더링하기 위해 OpenGL을 호출한다.

### 3. GPU

VSync Signal이 떨어지면 OpenGL을 사용하여 랜더링을 시작한다. 랜더링이 끝난뒤에는 buffer로 내용을 전달한다.

### 4. Display

Buffer로부터 데이터를 받아서 화면에 띄워준다.



위의 파이프라인 과정이 1초당 60번 (60Hz 주사율 기준) 이루어지게 된다. 만약 백그라운드 스레드를 활용해서 view를 변경한다면 여러 스레드에서 위의 파이프라인을 시작하는 trigger를 당기게 된다. 위의 파이프라인은 굉장히 비싼 작업이기 때문에 (GPU의 메모리 낭비 극심) 빈번한 context switching은 막는 것이 좋다.



## 개선할 수 있는 방법은 없을까?

Texture, ComponenetKit 을 이용해서 일정부분을 해결할 수 있다고 한다. 

Texture의 Node는 thread safe함과 동시에 UIView를 가지고 있으므로 main thread가 아니어도 UI 작업을 처리해줄 수 있다.

ComponentKit도 마찬가지로 thread safe를 보장한다.





### 참고링크

- [https://medium.com/@duwei199714/ios-why-the-ui-need-to-be-updated-on-main-thread-fd0fef070e7f](https://medium.com/@duwei199714/ios-why-the-ui-need-to-be-updated-on-main-thread-fd0fef070e7f)