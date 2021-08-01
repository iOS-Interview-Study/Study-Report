# Swift Standard Library의 map, filter, reduce, compactMap, flatMap에 대하여 설명하시오.

## Maps
### map
```swift
func map<T>(_ transform: (Element) throws -> T) rethrows -> [T]
```
요소에 대해 지정된 closure를 매핑한 결과가 포함된 배열을 반환합니다.

```swift
let cast = ["Vivien", "Marlon", "Kim", "Karl"]
let lowercaseNames = cast.map { $0.lowercased() }
// 'lowercaseNames' == ["vivien", "marlon", "kim", "karl"]
let letterCounts = cast.map { $0.count }
// 'letterCounts' == [6, 6, 3, 4]
```

### flatMap
```swift
func flatMap<SegmentOfResult>(_ transform: (Element) throws -> SegmentOfResult) rethrows -> [SegmentOfResult.Element] where SegmentOfResult : Sequence
```

Complexity: O(m + n), where n is the length of this sequence and m is the length of the result.

`s.flatMap(transform)`은 `Array(s.map(transform)`과 동일합니다.결합됨). 
Complexity: `O(m + n)`, n은 이 시퀀스의 길이이고 m은 결과의 길이입니다.


```swift
let numbers = [1, 2, 3, 4]

let mapped = numbers.map { Array(repeating: $0, count: $0) }
// [[1], [2, 2], [3, 3, 3], [4, 4, 4, 4]]

let flatMapped = numbers.flatMap { Array(repeating: $0, count: $0) }
// [1, 2, 2, 3, 3, 3, 4, 4, 4, 4]
```


### compactMap
```swift
@inlinable public func compactMap<ElementOfResult>(_ transform: (Element) throws -> ElementOfResult?) rethrows -> [ElementOfResult]
```

각 요소를 사용하여 map을 수행하지만 nil이 포함되지 않은 결과의 배열을 반환합니다.

```swift
let possibleNumbers = ["1", "2", "three", "///4///", "5"]

let mapped: [Int?] = possibleNumbers.map { str in Int(str) }
// [1, 2, nil, nil, 5]

let compactMapped: [Int] = possibleNumbers.compactMap { str in Int(str) }
// [1, 2, 5]
```
Complexity: `O(m + n)` n은 이 시퀀스의 길이이고 m은 결과의 길이입니다.

## filter
```swift
func filter(_ isIncluded: (Element) throws -> Bool) rethrows -> [Element]
```

지정된 조건에 해당(true)하는 요소들을 반환합니다.

## reduce
```swift
func reduce<Result>(_ initialResult: Result, _ nextPartialResult: (Result, Element) throws -> Result) rethrows -> Result
```

지정된 closure를 사용하여 시퀀스의 요소를 조합한 결과를 반환합니다.

```swift
let numbers = [1, 2, 3, 4]
let numberSum = numbers.reduce(0, { x, y in
    x + y
})
// numberSum == 10
```

1. `nextPartialResult` closure은 `initialResult - 0`으로 호출되고 숫자의 첫 번째 요소는 합계가 1로 반환됩니다.
2. closure은 이전 호출의 반환 값과 시퀀스의 각 요소를 사용하여 반복적으로 호출됩니다.
3. 시퀀스가 모두 사용되면 마감에서 반환된 마지막 값이 호출자에게 반환됩니다.

Complexity: O(n), n은 이 시퀀스의 길이입니다.