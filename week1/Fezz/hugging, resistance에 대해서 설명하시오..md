# [week 1] - Fezz

## hugging, resistance에 대해서 설명하시오.



![img](https://raw.githubusercontent.com/Fezravien/UploadForMarkdown/forUpload/img/img.png)



- Content hugging : **최대 크기**에 대한 제한
  - 주어진 크기보다 **작아질 수 있다.**
- Compression resistance: **최소 크기**에 대한 제한
  - 주어진 크기보다 **커질 수 있다.**

<br/>

---

**1. Content Hugging**



![img](https://raw.githubusercontent.com/Fezravien/UploadForMarkdown/forUpload/img/img-20210627201256634.png)



각 Label의 content hugging이 251일 때는 서로 priority가 같아, 같은 width를 유지
하지만 여기서 **빨간색 Label의 content hugging을 250**으로 바꾸면 아래와 같이 변한다.



![img](https://raw.githubusercontent.com/Fezravien/UploadForMarkdown/forUpload/img/img-20210627201301548.png)



<br/>

**2. Compression Resistance**



![img](https://raw.githubusercontent.com/Fezravien/UploadForMarkdown/forUpload/img/img-20210627201304025.png)



`width`의 `Constraint`와 `Compression resistance`가 둘 다 1000
둘 다 Priority가 같지만 둘의 width 제약이 44(UIButton의 기본 width)를 가리키므로 버튼 타이틀이 다 보이지 않는다
여기서 **width의 Constraint만 999**로 바꾸게 되면 아래와 같이 전부 보여지는 것을 확인할 수 있다.



![img](https://raw.githubusercontent.com/Fezravien/UploadForMarkdown/forUpload/img/img-20210627201308054.png)

<br/>

---

참고 

[Content hugging vs Compression resistance 차이점 알기! ](https://ontheswift.tistory.com/21)

