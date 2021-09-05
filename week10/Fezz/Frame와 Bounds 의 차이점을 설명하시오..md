# Week 10 - Fezz

## Frame와 Bounds 의 차이점을 설명하시오.



### Frame

<img src="https://raw.githubusercontent.com/Fezravien/UploadForMarkdown/forUpload/img/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202021-08-29%20%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE%205.35.51.png" alt="스크린샷 2021-08-29 오후 5.35.51" style="zoom:50%;" />

**View의 위치와 크기를 `superview`의 좌표계 기준으로 한다 !**

> Super View?
>
> - 최상위 View인 루트 View가 아니라, 나의 한칸 윗 계층 View를 말한다

<br>

#### Frame Point

<img src="https://blog.kakaocdn.net/dn/PPd9G/btqL2PkesNl/46dwzhgECMK18mM8G2CCKK/img.png" alt="img" width="50%" /><img src="https://blog.kakaocdn.net/dn/brWnxX/btqLZKxdAgw/S08euXvkx7nPHCUAag5Kdk/img.png" alt="img" width="42%" />

위 그림을 보면 **Super View의 원점에서** **얼마큼 떨어져 있느냐** 를 나타내는 것이다.

그러니까, 자신보다 바로 위의 상위뷰를 기준으로한 좌표이다.

<br>

#### Frame Siez

![img](https://blog.kakaocdn.net/dn/PPEBm/btqLXSQVU2L/8eapc4lmWdKVqCsTnQI1E0/img.png)

frame의 속성은 **View 영역을 모두 감싸는 사각형으로 나타낸다.** 

정의에 따라 **frame의 origin 값도 변경될 수 있다** 

<br>



### Bounds

![스크린샷 2021-08-29 오후 6.01.08](https://raw.githubusercontent.com/Fezravien/UploadForMarkdown/forUpload/img/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202021-08-29%20%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE%206.01.08.png)

**Frame과 다르게 View의 위치와 크기를 `own`의 좌표계를 기준으로 한다. 즉 자기 자신의 좌표계를 기준 !**

<br>

#### Bounds Point

![img](https://blog.kakaocdn.net/dn/bFO3QZ/btqL2vzyvWF/osL8uwpiPZtWwWLNmnqqrk/img.png)

bounds의 origin은 자신의 좌표계를 기준으로 하고, 초기에 (0, 0)로 초기화된다.

<br>

![img](https://blog.kakaocdn.net/dn/SwlAd/btqL8VEG0jj/kgrQtFERYLNKpEI6FgvLnK/img.png)

frame의 origin 값을 변경하면 **SubView도 그만큼 **같이 움직인다



<br>

#### Bounds Size

![img](https://blog.kakaocdn.net/dn/MJsGQ/btqL39KcHXP/1MJPghdI4qk7Pq6gvuAW4k/img.png)

View가 회전하든 안하든 **자신의 원점이 곧 좌표의 시작점**이 된다.

<br>

![img](https://blog.kakaocdn.net/dn/cVTcp5/btqL8nDenAl/zy9TEknzD7W6JBvu0vkv51/img.png)

`secondView`를 이동하게 되면 `thirdView`가 반대로 이동하는 것 처럼 보인다

<br>

![img](https://blog.kakaocdn.net/dn/MrBVB/btqL39LMa05/mECeGJsHPlIFtXrVqYJmnk/img.png)

![img](https://blog.kakaocdn.net/dn/pmqMf/btqMaEC1Q16/Il8PETB18b1BwDEbVlkw10/img.png)

기준 뷰의 `bounds`를 옮기는 것은 

그 속의 `Sub View`를 보여주는 `창(ViewPort)`를 옮긴다는 소리이다.

즉, 속 안에 뷰를 어떻게 보여줄 것인가 이다.

<br>

## 참고 

[소들이 - iOS) Frame vs Bounds 제대로 이해하기](https://babbab2.tistory.com/44?category=831129)

[Apple - Frame](https://developer.apple.com/documentation/uikit/uiview/1622621-frame)

[Apple - Bounds](https://developer.apple.com/documentation/uikit/uiview/1622580-bounds)

