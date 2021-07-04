 

###### 1 . iOS 앱을 만들고, User Interface를 구성하는 데 필수적인 프레임워크 이름은 무엇인가?

**UIKit** 

그래픽과 사용자 중심 사용자 인터페이스를 구조 및 관리하는 인터페이스. 

문자, 문자열 등 기본 객체를 사용할 때는 Foundation만 import 해도 됨.

- window 및 view 아키텍처, 멀티 터치와 같은 유형의 입력을 제공하는 이벤트 처리 인프라 제공
- 사용자, 시스템 및 앱 간의 상호 작용을 관리하는데 필요한 main run loop 제공
- 애니메이션, 문서, 그리기 및 인쇄, 현재 장치의 정보, 텍스트 관리 및 디스플레이, 검색, 접근성, 앱 확장 및 리소스 관리 지원



###### 2. 하나의 View Controller 코드에서 여러 TableView Controller 역할을 해야 할 경우 어떻게 구분해서 구현해야 하는지 설명하시오.

`viewDidLoad() 에서  `  `UITableView.register()` 를 사용해서 셀을 등록

`UITableViewDataSource` 프로토콜 내 함수를 구현하게 되면,여러 tableView가 같은 메소드를 호출하기 때문에 분기 처리를 해주어야 한다. 

분기 처리 방법 두가지

2. tableView의 종류로 구별하여 그에 맞는 cell 생성

```swift
func tableView(_ tableView: UITableView, cellForRowAt indexPath: IndexPath) -> UITableViewCell {
	if tableView == basicOneTableView {
			
	} else if tableView == basicTwoTableView {
		
	}
}
```

2. 테이블 뷰 생성 시,  tag 를 등록하여 구별



tableView 구현시, storyboard는 자동으로 셀을 등록하므로 별도의 등록 과정이 필요없다. 

Xib과 Code의 경우에는 별도로 등록해주어야 한다 .

```swift
func register(UINib?, forCellReuseIdentifier: String)
func register(AnyClass?, forCellReuseIdentifier: String)
```



###### 3. UIWindow 객체의 역할은 무엇인가?

<img width="535" alt="UIWindow" src="https://user-images.githubusercontent.com/29880961/124383740-0bf54680-dd09-11eb-966c-d1d4fb69d8e4.png">


- 앱의 시각적 콘텐츠를 담는 역할
- 뷰들과 다른 앱 객체들에게 터치 이벤트를 전달하는 역할
- 오리엔테이션 변화를 쉽게 하기 위해 앱의 뷰 컨트롤러들과 협력



xcode에서는 프로젝트가 생성될 때 `appDelegate`에 `window` 객체를 자동으로 생성하지만 Windows 객체를 새로 만들어야 하는 특별한 경우가 존재함. 

- 스토리보드를 쓰지 않는 경우는 main window를 직접 생성

- 외부 디스플레이를 지원하는 앱의 경우 새로운 window를 만들어 보여줄 수 있음

- 보통의 경우 새로운 window는 시스템에 의해 만들어지며 특정 이벤트에 대한 반응으로 만들어진다. 

  

###### 4 . 스토리보드를 이용했을때의 장단점을 설명하시오.

**장점**

- 앱의 규모가 크지 않은 기준에서 빠르게 만들고 싶을 때 
- 시각화 - 앱의 전체 흐름을 한눈에 보고 싶을 때 
- 협업하지 않고 혼자 만들 때 
- 낮은 진입장벽 - 적은 코드로 view를 구성할 수 있다

**단점** 

- 생산성 - 스토리보드 로딩 시간이 길어지면 생산성이 떨어짐

- 가독성 - 스토리보드가 많아지면 읽기가 어려워 가독성이 떨어짐

- 협업의 어려움 - xml 포맷 파일 형식으로 읽기 어렵기 때문에 merge시 처리가 어려움

- 재사용성 - 스토리보드로 만든 뷰는 재사용하기 어렵다 

- 번거로움 - 스토리보드 연결시, Identifier를 부여하여 일일이 연결해주는 것이 어려움

  

###### 5. Strong 과 Weak 참조 방식에 대해 설명하시오.

strong (강한 참조)
레퍼런스 카운트를 증가시켜 ARC로 인한 메모리 해제를 피하고, 객체를 안전하게 사용하고자 할 때 쓰인다.

- 해당 인스턴스의 소유권을 가진다 
- 자신이 참조하는 인스턴스의 retain count를 증가시킨다
- 값 지정 시점에 retain이 되고 참조가 종료되는 시점에 release가 된다 
- 선언할 때 아무것도 적어두지 않으면 default로 strong이 된다 



weak (약한 참조)
대표적으로 retain cycle에 의해 메모리가 누수되는 문제를 막기 위해 사용되며 delegate 패턴이 있다. 
약한 참조가 필요한 경우 weak 키워드만을 사용하고, guard let 

- 해당 인스턴스의 소유권을 가지지 않고, 주소값만을 가지고 있는 포인터 개념이다. 

- 자신이 참조하는 인스턴스의 retain count를 증가시키지 않는다. release도 발생하지 않는다. 

- 자신이 참조는 하지만 weak 메모리를 해제시킬 수 있는 권한은 다른 클래스에 있다. 

- 메모리가 해제될 경우 자동으로 레퍼런스가 nil로 초기화를 해준다. 

- weak 속상을 사용하는 객체는 항상 optional 타입 이어야 한다 (해당 객체가 nil일 수 있기 때문).

  
