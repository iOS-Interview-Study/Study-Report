# 하나의 View Controller 코드에서 여러 TableView Controller 역할을 해야 할 경우 어떻게 구분해서 구현해야 하는지 설명하시오.

- 매개변수로 넘어오는 `tableView` 를 가지고 구분한다.
    - 선언한 테이블뷰 변수 이름과 비교하거나
    - `tag` 를 사용해서 구현할 수 있다.

### 변수이름으로 구분 지어 구분한 코드

```swift
class ViewController: UIViewController {

    @IBOutlet weak var firstTableView: UITableView!
    @IBOutlet weak var secondTableView: UITableView!
    
    override func viewDidLoad() {
        super.viewDidLoad()
        firstTableView.dataSource = self
        firstTableView.delegate = self
        secondTableView.dataSource = self
        secondTableView.delegate = self
    }
    
}

extension ViewController: UITableViewDelegate, UITableViewDataSource {
    func tableView(_ tableView: UITableView, numberOfRowsInSection section: Int) -> Int {
        return 10
    }
    
    func tableView(_ tableView: UITableView, cellForRowAt indexPath: IndexPath) -> UITableViewCell {
        guard let cell = tableView.dequeueReusableCell(withIdentifier: "cell") else {
            fatalError()
        }
        
        if tableView == firstTableView {
            cell.textLabel?.text = "1, \(indexPath.row)"
        } else if tableView == secondTableView {
            cell.textLabel?.text = "2, \(indexPath.row)"
        }
    
        return cell
    }
    
}
```

### 참고자료

[https://woonhyeong.tistory.com/6](https://woonhyeong.tistory.com/6)

[https://www.youtube.com/watch?v=gufghHEbD_w](https://www.youtube.com/watch?v=gufghHEbD_w)
