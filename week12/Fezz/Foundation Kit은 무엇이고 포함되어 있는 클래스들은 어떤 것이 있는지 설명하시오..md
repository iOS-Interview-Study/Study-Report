# Week 12 - Fezz

## Foundation Kit은 무엇이고 포함되어 있는 클래스들은 어떤 것이 있는지 설명하시오.



`Foundation`은 원시 데이터 타입(String, Int, Double), 컬렉션 타입(Array, Dictionary, Set) 및 운영체제 서비스를 사용해 애플리케이션의 기본적인 기능을 관리한다.

- Foundation에는 데이터 타입, 날짜 및 시간, 필터 및 정렬, 네트워킹 등의 기본 기능을 제공한다.
- Foundation 프레임워크에서 정의한 클래스, 프로토콜 및 데이터 타입은 iOD, MacOS, tvOS 등 모든 애플의 SDK에서 쓰인다.

 

### 기능

- Number : Data, String : 원시 데이터 타입 사용

- Collection : Array, Dictionary, Set 등과 같은 컬렉션 타입 사용

- Date and Time : 날짜와 시간을 계산하거나 비교하는 작업

- Unit and Measuerment : 물리적 차원을 숫자로 표현 및 관련 단위간 변환 기능

- Data Formatting : 숫자, 날짜, 측정값 등을 문자열로 변환 또는 반대 작업

- Filter and Sorting : 컬렉션의 요소를 정렬하거나 검사

 <br>

**애플리케이션 지원**

- Resource : 애플리케이션의 에셋과 번들 데이터 접근 지원

- Notification : 정보를 퍼트리거나 받아들이는 작업

- App Extension : 확장 애플리케이션과의 상호작용 지원

- Error and Extension : API 상호작용에서 발생할 수 있는 예외 처리

<br> 

**파일 및 데이터 관리**

- File System : 파일 또는 폴더를 생성하고 읽고 쓰는 관리

- Archive and Serialization : 속성, 목록, JSON, 바이너리 파일들을 객체로 변환 혹은 반대

- iCloud : 사용자의 iCloud 계정을 이용해 데이터를 관리

 <br>

**네트워킹**

- URL Loading System : 표준 인터넷 프로토콜을 통해 URL과 상호 작용하고 서버와 통신

- Bonjour : 로컬 네트워크를 위한 작업

<br>

---

### UIKit

**"UIKit은 Foundation을 포함하고 있다."**

UIKit은 iOS 어플리케이션 사용자 인터페이스를 구현하고 이벤트를 관리하는 프레임워크입니다.

- UIKit 프레임워크는 제스처 처리, 애니메이션, 그림 그리기, 이미지 처리, 텍스트 처리 등 사용자 이벤트 처리를 위한 클래스를 포함합니다.

- 테이블뷰, 슬라이더, 버튼, 텍스트 필드, 얼럿 창 등 애플리케이션의 화면을 구성하는 요소를 포함합니다.

- UIKit 클래스 중 UIResponder에서 파생된 클래스나 사용자 인터페이스에 관련된 클래스는 애플리케이션의 메인 스레드(혹은 메인 디스패치 큐)에서만 사용.

 <br>

#### 구성

**사용자 인터페이스**

- View and Control : 화면에 콘텐츠 표시

- View Controller : 사용자 인터페이스 관리

- Animation and Haptics : 애니메이션과 햅틱을 통한 피드백 제공

- Window and Screen : 뷰 계층을 위한 윈도우 제공

<br> 

**사용자 액션**

- Touch, Press, Gesture: 제스처 인식기를 통한 이벤트 처리 로직

- Drag and Drop: 화면 위에서 드래그 앤 드롭 기능

- Peek and Pop: 3D 터치에 대응한 미리 보기 기능

- Keyboard and Menu: 키보드 입력을 처리 및 사용자 정의 메뉴 표시