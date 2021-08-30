#### Bounds 와 Frame 의 차이점을 설명하시오.

- **frame**은 <u>superview의 좌표계 기준</u>, **bounds**는 <u>자기 자신의 좌표계를 기준으로</u> 뷰의 크기와 위치(origin과 size)를 결정한다.

- **frame**의 좌표 값은 subview(상위뷰)로부터 얼만큼 떨어져 있는지를 나타낸다. 

  ```swift
  var frame: CGRect { get set }
  ```

   

- **bounds**는 superview의 좌표 시스템을 따르지 않으며, <u>자기 자신이 좌표계의 기준이 된다</u>. 기본 경계 원점은 (0,0)이며 크기는 frame 속성의 위치 정보가 없으며 view의 크기만 변경이 가능하다. 사각형의 크기를 변경하면 중심점 기준으로 view가 커지거나 줄어든다. 

  ```swift
  var bounds: CGRect { get set }
  ```

  

  <img src="/Users/jieunkim/Desktop/스크린샷 2021-08-30 오전 11.21.47.png" alt="스크린샷 2021-08-30 오전 11.21.47" style="zoom:50%;" />



<img src="/Users/jieunkim/Desktop/스크린샷 2021-08-30 오전 11.21.55.png" alt="스크린샷 2021-08-30 오전 11.21.55" style="zoom:50%;" />



frame은 주로 view의 위치나 크기를 설정할 때 사용하고, bounds는 view 내부에 그림을 그리거나 변형한 후에 view의 크기를 알고 싶을 때나 하위 view를 정렬하는 것과 같이 내부적으로 변경하는 경우에 사용한다.



#### 

