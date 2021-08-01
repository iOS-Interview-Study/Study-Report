# Week 6 - Fezz

## Swift Standard Library의 map, filter, reduce, compactMap, flatMap에 대하여 설명하시오.



### **맵(map)**

- map은 자신을 호출할 때 매개변수로 전달된 함수를 실행하여 그 결과값을 다시 반환해주는 함수입니다!
- map을 사용하기 위해서는 Swift의 Collection, Sequence 프로토콜을 따르면 가능합니다.
  - 따라서 Array, Dictionary, Set, 옵셔널 등에서 사용이 가능합니다
- map을 사용하여도 기존의 컨테이너의 값은 변경되지 않고 새로운 컨테이너가 생성되어 맵은 기존 데이터를 변형하는데 많이 사용이 되고는 합니다

```swift
let numbers: [Int] = [0,1,2,3]
var doubledNumbers = numbers.map { (num) -> Int in
    num * 2
}
print(doubledNumbers) // 0,2,4,6 
```

```swift
var doubledNumbers = numbers.map { num in
    num * 2
}
var doubledNumbers = numbers.map { return $0 * 2 } 
var doubledNumbers = numbers.map { $0 * 2 }
```

<br>

------

### **compactMap**

compactMap의 경우 컨테이너 안에 옵셔널 값이 있다면 그 옵셔널 값을 제외하고 새로운 컨테이너 안에 넣어주는 데 사용하게 됩니다!

```swift
let possibleNumbers = ["1", "2", "three", "///4///", "5"]
let mapped: [Int?] = possibleNumbers.map { str in Int(str) } // [1, 2, nil, nil, 5]
let compactMapped: [Int] = possibleNumbers.compactMap { str in Int(str) } // [1, 2, 5]
```

<br>

------

### **필터(filter)**

- filter는 단어 뜻 그대로 내부의 값들을 걸러서 추출하는 역할을 합니다.
- map과의 차이점을 map은 기존의 요소를 변경한 값을 반환했다면 필터는 기준을 가지고 기준에 맞는 값들을 반환해준다.

```swift
let x = [10, 3, 20, 15, 4]
var filtered = x.filter { $0 > 5 }.sorted() // 10 15 20
```

 

- 이렇게 배열 x의 요소중 5보다 *큰 값을 걸러내어(filter)* 순서대로 값을 갖는다.

```swift
let x = [10, 3, 20, 15, 4]
    .filter { $0 > 5 }
    .map { $0 * 100 }
```

<br>

------

### **리듀스(reduce)**

컨테이너의 내부의 요소들을 하나로 합치는 기능을 하는 고차 함수

```swift
let numbers: [Int] = [1,2,3,4,5]
var sum = numbers.reduce(0) { (result: Int, element: Int) -> Int in
    print("\(result) + \(element)")
    return result + element
}

// 0 + 1
// 1 + 2
// 3 + 3
// 6 + 4
// 10 + 5
```

```swift
var sum = numbers.reduce(0) { (result, element)  in
    return result+element
}
var sum = numbers.reduce(0, +)
```

<br>

---

### FlatMap vs. CompactMap

```swift
let array1 = [1, nil, 3, nil, 5, 6, 7]
let flatMapTest1 = array1.flatMap{ $0 }
let compactMapTest1 = array1.compactMap { $0 }

print("flatMap:", flatMapTest1)
print("compactMap:", compactMapTest1)

// flatMap: [1, 3, 5, 6, 7]
// compactMap: [1, 3, 5, 6, 7]
```

2차원에서 차이가 발생

```swift
let array2: [[Int?]] = [[1, 2, 3], [nil, 5], [6, nil], [nil, nil]]
let flatMapTest2 = array2.flatMap { $0 }
let compactMapTest2 = array2.compactMap { $0 }

print("flatMap:",flatMapTest2)
print("compactMap:",compactMapTest2)

<출력>
// flatMapTest2 : [1, 2, 3, nil, 5, 6, nil, nil, nil] (옵셔널)
// compactMapTest2 : [[1, 2, 3], [nil, 5], [6, nil], [nil, nil]]
```

