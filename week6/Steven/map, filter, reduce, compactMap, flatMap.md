
# Swift Standard Library의 map, filter, reduce, compactMap, flatMap에 대하여 설명하시오.

### 고차함수

다른 함수를 매개변수로 받거나 함수 실행의 결과를 함수로 반환하는 함수

```swift
// 출처: 위키피디아
func f(x: Int) -> Int {
    return x + 3
}

func g(function: (Int) -> Int, paramX: Int) -> Int {
    return function(paramX) * function(paramX)
}

g(function: f, paramX: 7)
```

## Map

- 컨테이너(Array, Set 등) 내부의 기존 데이터를 변형 하여 새로운 컨테이너를 생성한다.
- 핵심 기존 컨테이너의 자료는 변경되지 않고 **새로운 컨테이너를 생성한다는 것**이다.

```swift
let numbers: [Int] = [0, 1, 2, 3]
let doubledNumbers: [Int] = numbers.map { return $0 * 2 }
```

## filter

- 컨테이너 내부의 값을 걸러서 새로운 컨테이너로 추출한다.
- 이 역시 기존 것을 수정하지 않고 새로운 컨테이너를 만든다.

```swift
let numbers: [Int] = [0, 1, 2, 3]
let evenNumbers: [Int] = numbers.filter{ return $0 % 2 == 0 }
```

## reduce

- 컨테이너 내부의 데이터를 하나로 합칠 때 사용한다.

```swift
let numbers = [0, 1, 2, 3, 4]

numbers.reduce(0) {$0 + $1} // 10
numbers.reduce(10, +)       // 20 = 10 + numbers
```

## compactMap

- Returns an array containing the **non-nil results** of calling the given transformation with each element of this sequence.
- 배열 내부에 nil이 아닌 것만 리턴을 한다.

```swift
let possibleNumbers = ["1", "2", "three", "///4///", "5"]

let mapped: [Int?] = possibleNumbers.map { str in Int(str) }
// [1, 2, nil, nil, 5]

let compactMapped: [Int] = possibleNumbers.compactMap { str in Int(str) }
// [1, 2, 5]
```

## flatMap

- Returns an array containing the concatenated results of calling the given transformation with each element of this sequence.
- 쉽게 말해서 다차원(2,3) 배열을 1차원으로 만들어 준다.

```swift
let numbers = [1, 2, 3, 4]

let mapped = numbers.map { Array(repeating: $0, count: $0) }
// [[1], [2, 2], [3, 3, 3], [4, 4, 4, 4]]

let flatMapped = numbers.flatMap { Array(repeating: $0, count: $0) }
// [1, 2, 2, 3, 3, 3, 4, 4, 4, 4]
```

### 참고자료

[https://yagom.github.io/swift_basic/contents/22_higher_order_function/](https://yagom.github.io/swift_basic/contents/22_higher_order_function/)

[https://ko.wikipedia.org/wiki/고차_함수](https://ko.wikipedia.org/wiki/%EA%B3%A0%EC%B0%A8_%ED%95%A8%EC%88%98)

[https://shark-sea.kr/entry/Swift-고차함수-Map-Filter-Reduce-알아보기](https://shark-sea.kr/entry/Swift-%EA%B3%A0%EC%B0%A8%ED%95%A8%EC%88%98-Map-Filter-Reduce-%EC%95%8C%EC%95%84%EB%B3%B4%EA%B8%B0)

[https://zeddios.tistory.com/448](https://zeddios.tistory.com/448)

[https://developer.apple.com/documentation/swift/sequence/2950916-compactmap](https://developer.apple.com/documentation/swift/sequence/2950916-compactmap)

[https://developer.apple.com/documentation/swift/sequence/2905332-flatmap](https://developer.apple.com/documentation/swift/sequence/2905332-flatmap)

[https://jinshine.github.io/2018/12/14/Swift/22.고차함수(2) - map, flatMap, compactMap/](https://jinshine.github.io/2018/12/14/Swift/22.%EA%B3%A0%EC%B0%A8%ED%95%A8%EC%88%98(2)%20-%20map,%20flatMap,%20compactMap/)
