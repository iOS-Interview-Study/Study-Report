# TableView를 동작 방식과 화면에 Cell을 출력하기 위해 최소한 구현해야 하는 DataSource 메서드를 설명하시오.

---



#### TableView는 다양한 객체의 협력을 통해서 구현됩니다.

- Cell: Cell은 각 테이블 뷰 행의 컨텐츠를 보여줍니다.
- Controller: 통상적으로 UIViewController 또는 UITableViewController 객체를 통해서 table view 관리를 합니다.
- data source: 해당 객체는 `UITableViewDataSource` 프로토콜을 채택한 객체로서 테이블 뷰에 데이터를 제공해 줍니다.
- delegate: 해당 객체는 `UITableViewDelegate` 프로토콜을 채택한 객체이고 유저와 테이블뷰의 컨텐츠간의 상호작용을 관리합니다.



#### TableView는 MVC패턴으로 작동합니다.

TableView 자체는 오직 데이터를 사용자에게 보여주는 것만 관리를 하고 데이터 자체를 관리하지 않습니다. 해당 데이터를 관리하기 위해서 tableView에게 data source 객체를 제공 해 줍니다. 해당 객체는 `UITableViewDataSource` 프로토콜을 채택한 객체이고 많은 경우 ViewController가 datasource 역할을 하게됩니다.

``` swift
class ViewController: UITableViewDelegate
```

datasource 객체는 tableview의 데이터와 관련된 모든 요청에 응답하고 더 나아가서 테이블의 데이터를 직접적으로 관리하게 됩니다. 뿐만 아니라 datasource는

- 테이블뷰의 섹션과 행의 숫자를 알려줍니다.
- 각 행에 cell을 제공 해 줍니다.
- 섹션에 해더(header)와 푸터(footer)를 제공 해 줍니다.
- table의 인덱스값을 설정 해 줍니다.
- 필요에 따른 테이블뷰의 데이터 업데이트에 반응 합니다.



이 중에서도 cell을 출력하기 위해 최소한 구현해야 하는 메서드는 아래와 같습니다.

``` swift
// Return the number of rows for the table.     
override func tableView(_ tableView: UITableView, numberOfRowsInSection section: Int) -> Int {
   return 0
}

// Provide a cell object for each row.
override func tableView(_ tableView: UITableView, cellForRowAt indexPath: IndexPath) -> UITableViewCell {
   // Fetch a cell of the appropriate type.
   let cell = tableView.dequeueReusableCell(withIdentifier: "cellTypeIdentifier", for: indexPath)
   
   // Configure the cell’s contents.
   cell.textLabel!.text = "Cell text"
       
   return cell
}
```

테이블뷰의 행의 갯수과 cell을 제공 해 주는 메서드가 최소한 구현되어 있어야 테이블뷰에 cell을 띄울 수 있게 됩니다.



[참고]:

[Apple Developer Document | Table Views](https://developer.apple.com/documentation/uikit/views_and_controls/table_views)

[Apple Developer Document | UITableViewDataSource](https://developer.apple.com/documentation/uikit/uitableviewdatasource)