# MVC 구조에 대해 블록 그림을 그리고, 각 역할과 흐름을 설명하시오.

![image](https://user-images.githubusercontent.com/35272802/132095152-40809d83-70f4-48f6-9469-44506df77f8a.png)


## 모델

- 뷰에 보여줄 데이터의 모음입니다. (예 Person, Item)
- 데이터를 처리할 비지니스 로직의 모음입니다. (예 NetworkManager, ImageLoader)

## 뷰

- 화면에 보여지는 것
- UIKit에서는 `UIView` 가 해당된다.

## 컨트롤러

- 뷰와 모델을 컨트롤 해주는 객체

## 흐름

- 사용자의 액션을 뷰가 받음
- 뷰는 컨트롤러에 해당 이벤트를 전달
    - 컨트롤러는 판단을 해서 뷰의 모얌만 변경이 되는 것이면 뷰를 업데이트 함.
    - 모델에서 데이터를 갱신해야 된다면 모델에게 요청을 함.
- 모델은 비즈니스 로직을 실행해서 데이터가 변경이 되면 컨트롤러에게 알림.
    - 컨트롤러는 변경된 데이터를 뷰에 보여줌(업데이트)

### 공부하면서 알게 된 점.

- 전통적인 MVC와 Cocoa MVC는 조금 다르다.
- 그리고 애플이 의도한 Cocoa MVC 처럼 뷰 모델 컨트롤러를 분리하기 어렵다.
- ViewController가 View도 가지고 있고 Controller의 역활을 하기 때문에 결국에는 뷰컨과 모델간에 통신을 하게 된다.  → 뷰컨에 매우 커져서 유지보수, 테스트 하기가 어렵다.
- ViewController의 역활을 줄이려는 노력으로 MVP, MVVM, VIPER 등에 다양한 아키택쳐가 나왔다.
- 이것의 역사를 집어가면서 흐름으로 공부를 하면 좋을 것 같다.

### 참고자료

[https://developer.apple.com/library/archive/documentation/General/Conceptual/DevPedia-CocoaCore/MVC.html](https://developer.apple.com/library/archive/documentation/General/Conceptual/DevPedia-CocoaCore/MVC.html)

[https://medium.com/@jang.wangsu/디자인패턴-mvc-패턴이란-1d74fac6e256](https://medium.com/@jang.wangsu/%EB%94%94%EC%9E%90%EC%9D%B8%ED%8C%A8%ED%84%B4-mvc-%ED%8C%A8%ED%84%B4%EC%9D%B4%EB%9E%80-1d74fac6e256)

[https://www.youtube.com/watch?v=ogaXW6KPc8I](https://www.youtube.com/watch?v=ogaXW6KPc8I)

[https://jiyeonlab.tistory.com/38](https://jiyeonlab.tistory.com/38)

[***https://github.com/protocorn93/iOS-Architecture***](https://github.com/protocorn93/iOS-Architecture)

[https://nightohl.tistory.com/entry/iOS-아키텍처-패턴-Apples-MVC](https://nightohl.tistory.com/entry/iOS-%EC%95%84%ED%82%A4%ED%85%8D%EC%B2%98-%ED%8C%A8%ED%84%B4-Apples-MVC)

[https://medium.com/@esung/mvc-정말제대로알고계신가요-875f1323f6c0](https://medium.com/@esung/mvc-%EC%A0%95%EB%A7%90%EC%A0%9C%EB%8C%80%EB%A1%9C%EC%95%8C%EA%B3%A0%EA%B3%84%EC%8B%A0%EA%B0%80%EC%9A%94-875f1323f6c0)
