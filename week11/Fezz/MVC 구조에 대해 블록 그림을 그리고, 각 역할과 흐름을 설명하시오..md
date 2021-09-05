# Week 11 - Fezz

## MVC 구조에 대해 블록 그림을 그리고, 각 역할과 흐름을 설명하시오.



### 아키텍처를 왜 생각해야 될까?

- 아키텍처가 없다면 프로젝트가 확장되었을 때 가독성이 떨어져 `디버깅`이 힘들어진다.
- 아키텍처를 사용하면 협업에서 `의사소통`, `유지보수`, `확장` 등의 비용이 적어진다.

<br>

### 좋은 아키텍처란 무엇일까?

1. Balanced **distribution of responsibilities** among entities with strict roles.
   - 각각의 객체들의 역할이 명확해야 하며 `SOLID` 원칙의 Single Responsibility Principli(단일 책임 원칙)을 의미한다.
   - 하나의 객체에 하나의 역할만 하도록 하여 복잡도를 낮춘다.
2. **Testability** usually comes from the first feature.
   - 테스트 가능 
   - 1 번과 연관이 있는데, 단일 책임 원칙을 지키지 않다면 테스트가 어려워진다.
3. Ease of use and a low maintenance cost.
   - 사용성과 유지보수성의 비용

이 세가지 조건이 각각 다른 조건이 아니라 결국 하나의 의미가 된다.

**단일 책임 <--> 테스트 <--> 사용성, 유지보수** 

<br>

### 아키텍처를 알아보자 

가장 많이 사용되는 아키텍처인 `MVC`, `MVP`, `MVVM`를 알아보도록 한다.

그 중 `MVC`, `MVP`, `MVVM`에는 공통적으로 가지고 있는게 있는데,

`M - Model` 

데이터를 다루는 부분 (형식, 가공 ... 등)

`V - View`

 화면에 보여지는 부분 (View 멍청해야지 ㅎㅎ)

`C, P, VM - Controller/Presenter/ViewModel`

Model과 View를 연결하는 부분 

View로 받은 이벤트로 인해 Model이 변경되고, 변경된 Model로 View를 갱신한다.

<br>

---

#### MVC

##### 전통적 MVC

먼저 전통적인 `MVC` 패턴에 대해 살펴본다.

<img src="https://raw.githubusercontent.com/Fezravien/UploadForMarkdown/forUpload/img/img-20210905171344004.png" alt="img" width="60%;" />

전통적인 `MVC` 는  각각의 Model, VIew, Controller가 밀접하게 연관되어있다.

이 부분은 좋은 아키텍처의 `단일 책임 원칙`에 위배되는 구조가 된다.

<br>

##### Apple's MVC

<img src="https://raw.githubusercontent.com/Fezravien/UploadForMarkdown/forUpload/img/img-20210905171338697.png" alt="img" width="45%;" /> <img src="https://blog.kakaocdn.net/dn/bByoZL/btqCjSrUUCD/KDt3GJk3os713GCn1xK9BK/img.png" alt="img" width="54%;" />

애플의 `MVC`는 Controller가 View와 Model의 중재자 역할로 View와 Model에 독립성을 부여했다. 

특징에 대해서 살펴보자.

- Model과 View는 서로를 모르게 독립되었지만, View와 Controller가 밀접한 것을 볼 수있으며, 이로인해 책임이 모호해질 수 있다.
- 책임의 모호함(로직 + UI요소) 으로 인해 Model만 테스트 가능한 구조가 된다.
- 다른 아키텍처에 비해 구현이 쉽다.
- 다른 아키텍처에 비해 구조가 복잡하지 않지만 그만큼 Controller의 코드가 밀집되어 커지게 된다.

<br>

---

#### MVP

<img src="https://blog.kakaocdn.net/dn/lWAb8/btqCl8HDdmc/oPKDEckMOLlcB9LoAqIniK/img.png" alt="img" width="80%;" />

`MVC` 패턴과 매우 비슷한 구조를 보여준다.

`MVC`와 다르게 UIView와 UIViewController가 Passive View에 속한다. 

따라서, Controller에 해당하는 Presenter에 View와 관련된 것이 포함되지 않는다. (UI와 비즈니스 로직이 분리됨)

 이로 인해 각각의 객체의 독립성이 `MVC` 보다 좋아졌고, 테스트가 용의해졌다.

<br>

특징에 대해 살펴보면,

- `MVC`와 다르게 View, Controller의 역할이 분리되면서 View를 갱신하기 위한 작업을 따로 해줘야 하는 이유로 비용이 증가한다.
- 각각의 역할이 분리됨에 따라 테스트가 가능하다.
- `MVC` 보다 코드의 양이 증가하지만 각각의 역할이 명확해졌다.
- **Presenter와 View는 1:1 관계를 가진다.**

<br>

---

#### MVVM

<img src="https://raw.githubusercontent.com/Fezravien/UploadForMarkdown/forUpload/img/1*uhPpTHYzTmHGrAZy8hiM7w.png" alt="img" width="85%;" />

Model, Controller, View 각각 독립성을 가지고 있으며, View, Controller와 Model 사이에서 View Model이 중재자 역할을 수행한다.

`MVVM`의 가장 큰 차이이자 중요한 부분은 View Model 부분인데 

- View를 통해 사용자로부터 이벤트를 받으면 Model에 적용한다
- Model의 변화가 생기면  View에 갱신한다.

이 부분을 보면 `MVP`와 같다고 생각할 수 있지만, **결론부터 말하면 다른다 !!**

그 이유는 `MVVM`의 View Model은 View와 두 가지 패턴으로 엮여있다.

- `Command` :  View로 부터 받은 이벤트를 ViewModel을 통해 Model을 업데이트 한다.
- `Data Binding` : Model이 업데이트 됨에 따라 View를 **자동**갱신한다. 

> Data Binding이 `MVP` 처럼 "Model이 업데이트 되면 View를 갱신한다." 와 비슷해보지만, "**자.동.**"이게 중요한 포인트이다. 따로 업데이트 해주는 로직을 작성한다기 보다 Model이 변경됨을 알려줘서 즉각적 반영(?) 이라고 생각하면 될 것 같다. 

이를 이용하여 View와 ViewModel 사이의 의존성을 제거한다.

<br>

---

📑 참고 

[iOS Architecture Patterns](https://medium.com/ios-os-x-development/ios-architecture-patterns-ecba4c38de52)

[[iOS 아키텍처 패턴] MVC](https://jiyeonlab.tistory.com/38?category=818842)

