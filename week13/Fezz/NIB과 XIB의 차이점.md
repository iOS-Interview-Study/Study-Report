# Week 13 - Fezz

## nib과 xib의 차이점.



### NIB 파일이란?

`Nextstep Interface Builder`의 약자로, 화면을 구성하는 클래스들을 **바이너리 형태의 압축 파일**로 저장하고 있다.

![img](https://raw.githubusercontent.com/Fezravien/UploadForMarkdown/forUpload/img/9937AD4A5B51A1720D.png) 

<br>



### XIB 파일이란?

`Xcode Interface Builder`의 약자로, 화면을 구성하는 클래스들을 **XML 문법**에 맞춰 저장하고 있다. 

<br>

### 뷰 정보를 읽어드리는 방법

뷰 정보를 담고 있는 XIB파일을 빌드하게 되면, 접근하기 쉬운 바이너리 NIB파일로 컴파일 되고, 

앱의 번들(앱 실행에 사용되는 파일이 저장된 폴더)로 복사된 후 실행파일에서 사용된다.

 XIB를 컴파일하면 NIB가 만들어진다.

![img](https://raw.githubusercontent.com/Fezravien/UploadForMarkdown/forUpload/img/995555465B51C73B30.png) 



<br>

### Storyboard 란?

스토리 보드는 `뷰 정보`와 `뷰와 뷰 사이의 관계에 대한 정보`를 가지고 있는 파일이다. 

그렇기 때문에 뷰 정보인 XIB의 내용과 뷰 관계정보를 포함하고 있어서 XML로 이뤄져있으며, 빌드를 하게 되면 컴파일된 storyboard파일로 .storyboardc라는 파일이 생성된다.

`storyboardc` 파일은 패키지 파일로 되어있고 이 내부는 nib파일과 nib파일을 관리하는 plist 파일이 존재한다.

![img](https://raw.githubusercontent.com/Fezravien/UploadForMarkdown/forUpload/img/991E49495B51C5991B.png) 

<스토리보드 파일 내부 구조>



<br>

---

📄 참고 

[iOS의 화면을 그리기 위해선 이것이 필요합니다! XIB, NIB, Storyboard](https://dalgonakit.tistory.com/82)



