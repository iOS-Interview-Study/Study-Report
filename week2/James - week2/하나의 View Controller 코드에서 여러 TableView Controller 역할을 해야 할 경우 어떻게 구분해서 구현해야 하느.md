# 하나의 View Controller 코드에서 여러 TableView Controller 역할을 해야 할 경우 어떻게 구분해서 구현해야 하나요?

---



여러 개의 `TableView` 의 변수명을 구변될 수 있게 코드 또는 스토리보드를 활용하여 작성 합니다:

예: `toDoTableView`, `doingTableView`, `doneTableView`

하나의 View Controller에 `tableViewDatasource` 그리고 `tableViewDelegate` 프로토콜을 채택한 뒤 프로토콜을 활용하여 cell의 속성을 설정 해 줄 때 `switch`문 또는 `if else` 문을 활용 하여 구분해서 구현해줄 수 있습니다:



``` Swift
@ibOutlet weak var todoTableView
@ibOutlet weak var doingTableView
@ibOutlet weak var doneTableView

func tableView(_ tableView: UITableView, cellForRowAt indexPath: IndexPath) -> UITableViewCell {
  
  switch tableView: {
    case todoTableView:
  		// todoTableView 셀 구현
  
  	case doingTableView:
  		// doingTableView 셀 구현
  
  	case doneTableView:
  		// doneTableView 셀 구현
  }
}
```

