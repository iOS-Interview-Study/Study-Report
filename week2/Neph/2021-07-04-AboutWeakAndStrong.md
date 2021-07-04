---
layout: post
title: "Strong, Weak에 대해 알아보자"
image:
categories: iOS
tags: 
  - strong
  - weak
sitemap:
  changefreq: daily
  priority: 1.0
---

## Reference Count

Strong과 Weak에 대해 알아보려면 우선 Reference Count에 대한 지식이 있어야한다.

Swift에서는 Reference Counting을 통해 인스턴스가 살아있음을 보장한다. Reference Count가 올라가 있다는 것은 이것을 사용하고 있는 다른 인스턴스가 있다는 것이고, 이런 경우에 메모리에서 해제하면 안된다.

이는 ARC를 통해 자동으로 관리되며 프로그래머는 Retain Cycle (데드락)이 생기지 않도록 주의해서 코드를 작성해야한다.



## Strong 참조

일반적으로 참조하는 방식이 strong 참조방식이다.

해당 인스턴스의 소유권을 가지며, reference count를 증가시키는 방식으로 값을 지정하는 시점에 retain되고 참조가 종료되는 시점에 release 된다. 

참조를 종료하지 않은채로 놔둔다면 메모리 누수가 발생하게 되는데 이를 영원히 해제할 수 없는 경우도 발생한다.

## Weak 참조

해당 인스턴스의 소유권을 가지지 않고 주소값만을 가지고 있다. 참조하는 인스턴스의 retain count를 증가시키지 않으며 release를 하지도 않는다.

![img](https://raw.githubusercontent.com/Neph3779/Blog-Image/forUpload/img/20210704154353.png)

위의 그림처럼 서로가 서로를 약하게 참조하는 것들만 남아있다면 메모리에서 해제될 것이다. (retain count가 0이 되면 메모리에서 해제된다는 말을 풀어서 쓴 것) 

이렇게 retain count의 걱정 없이 안전하게 참조를 할 수 있다.

