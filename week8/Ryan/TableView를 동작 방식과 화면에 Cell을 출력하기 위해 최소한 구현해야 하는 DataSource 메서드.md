# TableView를 동작 방식과 화면에 Cell을 출력하기 위해 최소한 구현해야 하는 DataSource 메서드를 설명하시오.

## TableView 동작 방식
### 요약
1. `UITableView`는 딜리게이트인 `UITableViewDataSource`에게 섹션 개수와 셀에 적용될 데이터를 재사용 큐에 있는 `UITableViewCell`에 적용하라는 임무를 줍니다. (이 때 재사용 큐에서 셀을 꺼내주는 주체는 `UITableView`입니다.(`tableView.dequeueReusableCell(withIdentifier:for:)`) - [동작 트리거는 `UITableViewDataSource` 객체]
2.  TableView가 처음 로드되는 상황과 같이 재사용 큐에 재사용할 수 있는 셀이 없는 경우에는 1에서 언급한 메서드가 셀을 만들고, 그렇지 않은 경우에는 재사용 큐에서 꺼내어 사용합니다.
3. `UITableView`는 `UITableViewDataSource`와 같이 또 한 명의 동업자로 `UITableViewDelegate`입니다. 이 딜리게이트는 주로 셀 선택, swipe 액션 등 사용자의 입력에 응답하거나 셀, 섹션 헤더와 푸터의 높이를 계산하는 등의 일을 수행합니다. 하지만 TableView를 로드하는데에는 필수적인 요소가 아니므로 `UITableViewDataSource`처럼 테이블 뷰를 나타내기 위해 반드시 채택하여야 하거나 구현해야하는 메서드는 없습니다.

### 긴 이야기
`UITableView`는 함께 일하는 동료들을 여럿 데리고 있는데, 그 중 가장 핵심적인 기능을 수행하는 타입이 `UITableViewDataSource`와 `UITableViewCell`입니다. 이들이 수행하는 핵심적인 역할 중의 일부는 TableView이 Header와 Footer 포함하여 제공할 셀들의 데이터를 제공해주고, 각 셀이 어떠한 모양 (레이블, 상세 레이블, 이미지 등)을 가질지를 결정해주는 것입니다. `UITableViewDataSource`는 단순히 자격요건을 표시한 프로토콜로 자체적인 구현이 없기 때문에 누군가 이를 채택한 후 구현해주어야 하는데요, 주로 TableView를 가진 ViewController에서 수행하는 경우가 많습니다. `UITableViewDataSource`의 자격을 가지기 위해서는 반드시 아래 두 메서드를 구현해주어야 합니다.  

```swift
func tableView(_ tableView: UITableView, numberOfRowsInSection section: Int) -> Int
func tableView(_ tableView: UITableView,
               cellForRowAt indexPath: IndexPath) -> UITableViewCell
```
먼저, 첫 번째 메서드는 테이블뷰가 보여줄 섹션의 개수를 알려주는 것입니다. 알기 쉽게 반환값이 `Int` 타입으로 표현되어 있죠. 섹션 (Section)은 아래 그림과 같이 여러 행(Row)을 가질 수 있는 대상이고, Row는 섹션에 포함된 하나의 셀을 의미합니다. 섹션은 자신의 시작과 끝을 구분해주는 섹션 헤더(Header)와 푸터(Footer)를 가질 수 있습니다.
![](https://images.velog.io/images/ryan-son/post/1570bfcc-ab64-4117-93be-8fa3dcdf0482/image.png)

다음으로 아래 메서드는 각 행에 나타날 셀들에 들어갈 내용들을 입력해주는 메서드입니다. 반환 타입은 예상하시듯 `UITableViewCell`입니다. 이 메서드를 통해 각 셀이 TableView 내에서의 위치(`indexPath`)에 있을 때 가져야 할 내용을 입력해줄 수 있습니다 (`tableView.dequeueReuseableCell(withIdentifier:for:)`). [Apple 공식 문서](https://developer.apple.com/documentation/uikit/uitableview/1614878-dequeuereusablecell)에 따르면 괄호 안의 메서드는 반드시 `tableView(_:cellForRowAt:)` 메서드 내부에서 호출하여 사용하여야 합니다. 이렇게 보여질 내용들을 모두 구현하고 어떤 셀을 재사용할 것인지 `func register(AnyClass?, forCellReuseIdentifier: String)`과 같은 메서드를 사용해 등록해주면 비로소 TableView의 로드가 가능합니다.
등록은 아래 방법 중 자신이 구현하는 방향에 맞추어 하나의 방법을 택하여 사용하시면 됩니다.
- `func register(AnyClass?, forCellReuseIdentifier: String)`
- `func register(UINib?, forCellReuseIdentifier: String)`
- Interface Builder에서 Cell을 선택하면 Attribute inspector에서 확인할 수 있는 ReuseIdentifier에 입력
![](https://images.velog.io/images/ryan-son/post/f4bebdc8-0b18-4fea-88eb-c877aa6bf1d6/image.png)

추가로 여기에서 셀, 섹션의 헤더와 푸터 높이 또는 사용자의 셀 선택, swipe 액션과 같은 이벤트에 응답하려면 또 `UITableView`의 또 하나의 딜리게이트인 `UITableViewDelegate`를 구현한 타입과 함께 일하면 됩니다.

# 참고자료
- [UITableView](https://developer.apple.com/documentation/uikit/uitableview) - Apple Developer
- [UITableViewDataSource](https://developer.apple.com/documentation/uikit/uitableviewdatasource) - Apple Developer
- [UITableViewDelegate](https://developer.apple.com/documentation/uikit/uitableviewdelegate) - Apple Developer
- [tableView.dequeueReuseableCell(withIdentifier:for:)](https://developer.apple.com/documentation/uikit/uitableview/1614878-dequeuereusablecell) - Apple Developer
