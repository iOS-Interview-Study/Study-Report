---
layout: post
title: "하나의 ViewController에 여러개의 TableView 쓰기"
image:
categories: 
tags: 
  - 
sitemap:
  changefreq: daily
  priority: 1.0
---

## Table View

우선 테이블뷰를 사용하기 위해서는 아래의 작업들이 필요하다.

- cell 등록 (register)
- datasource, delegate 지정

cell register는 각각의 tableView 마다 register를 해주면 되기에 문제가 되지 않는다. (스토리보드를 사용한다면 cell을 자동으로 등록해주기 때문에 register cell을 직접적으로 해주지 않아도 된다.)

하나의 뷰 컨트롤러에서 여러개의 tableView를 쓸 때 가장 문제인건 datasource와 delegate 메서드를 공유한다는 점이다. 이를 해결하기 위해선 아래와 같이 메서드 내에서의 분기를 통해 해결할 수 있다.

```swift
func tableView(_ tableView: UITableView, cellForRowAt indexPath: IndexPath) -> UITableViewCell {
	if tableView == tableView1 {
		// ...
	} else if tableView == tableView2 {
		// ...
	}
}
```