# map, filter, reduce, compactMap, flatMap에 대하여 설명하시오

---



## Map

**컬렉션 내부의 기존 데이터를 변형하여 새로운 컬랙션으로 변경 및 생성하여 반환하는 메서드**

``` swift
let cast = ["Vivien", "Marlon", "Kim", "Karl"]
let lowercaseNames = cast.map { $0.lowercased() }
// 'lowercaseNames' == ["vivien", "marlon", "kim", "karl"]
let letterCounts = cast.map { $0.count }
// 'letterCounts' == [6, 6, 3, 4]
```



## filter

**주어진 조건에 합당하는 요소를 담은 배열을 반환하는 메서드**

``` swift
let cast = ["Vivien", "Marlon", "Kim", "Karl"]
let shortNames = cast.filter { $0.count < 5 }
print(shortNames)
// Prints "["Kim", "Karl"]"
```





## reduce

**컨테이너 내부의 요소를 하나로 통합해서 반환하는 메서드**

``` swift
let numbers = [1, 2, 3, 4]
let numberSum = numbers.reduce(0, { x, y in
    x + y
})
// numberSum == 10
```



## compactMap

**주어진 요소를 체크한 뒤 nil이 아닌 요소를 담은 배열로 반환하는 메서드**

``` swift
let possibleNumbers = ["1", "2", "three", "///4///", "5"]

let mapped: [Int?] = possibleNumbers.map { str in Int(str) }
// [1, 2, nil, nil, 5]

let compactMapped: [Int] = possibleNumbers.compactMap { str in Int(str) }
// [1, 2, 5]
```



## flatMap

**파라미터에 들어가는 요소들을 single-level collection으로 반환하는 메서드**

``` swift
let numbers = [1, 2, 3, 4]

let mapped = numbers.map { Array(repeating: $0, count: $0) }
// [[1], [2, 2], [3, 3, 3], [4, 4, 4, 4]]

let flatMapped = numbers.flatMap { Array(repeating: $0, count: $0) }

```





[출처]:

[Apple Developer Document | filter(_:)](https://developer.apple.com/documentation/swift/sequence/3018365-filter)

[Apple Developer Document | reduce(_:)](https://developer.apple.com/documentation/swift/array/2298686-reduce)

[Apple Developer Document | map(_:)](https://developer.apple.com/documentation/swift/array/3017522-map)

[Apple Developer Document | compactMap(_:)](https://developer.apple.com/documentation/swift/sequence/2950916-compactmap)

[Apple Developer Document | flatMap(_:)](https://developer.apple.com/documentation/swift/sequence/2905332-flatmap)

