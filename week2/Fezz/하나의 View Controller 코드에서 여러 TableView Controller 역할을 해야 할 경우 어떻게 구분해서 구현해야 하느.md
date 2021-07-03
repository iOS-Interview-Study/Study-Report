# Week 2 - Fezz

## 하나의 View Controller 코드에서 여러 TableView Controller 역할을 해야 할 경우 어떻게 구분해서 구현해야 하는지 설명하시오.



### UITableView DataSource 

- func tableView(_ tableView: UITableView, cellForRowAt indexPath: IndexPath) -> UITableViewCell

```swift
func tableView(_ tableView: UITableView, cellForRowAt indexPath: IndexPath) -> UITableViewCell {
	if tableView == self.tableView {
		...		
	} else if tableView == self.tableView2 {
		...
	}
    ...
}
```

<br>

- TableView tag 이용하기 

```swift
func tableView(_ tableView: UITableView, cellForRowAt indexPath: IndexPath) -> UITableViewCell {
	switch tableView.tag {
		case 0:
			...
		case 1:
			...
		default: 
			break
	}
}
```

<br>

---



`tag` 는 추후에 앱을 확장하거나 리펙토링이 있을때 유연하게 대응하지 못할 것 같다.

