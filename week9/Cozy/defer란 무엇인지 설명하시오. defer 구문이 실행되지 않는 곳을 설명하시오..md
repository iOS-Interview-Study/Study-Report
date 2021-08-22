#### defer란 무엇인지 설명하시오.



현재 코드 블록을 나가기 전에 꼭 실행해야 하는 코드.  블록을 어떤 방식으로 빠져나가든 꼭 마무리해야하는 작업이 있을 때 사용. 

한 코드 블록안에 여러개의 defer를 선언했다면, 가장 마지막에 선언된 defer 순으로 실행이 된다.  임시 중간 변수를 추가할 필요가 없어짐.

```swift
func test() {
    defer {
        print("test function end")
    }

    print("Hello world")
}
```





다중 defer의 경우 역순으로 실행

```swift
func f() {
    defer { print("First defer") }
    defer { print("Second defer") }
    print("End of function")
}
f()
// Prints "End of function"
// Prints "Second defer"
// Prints "First defer"
```



##### defer가 호출되는 순서는 어떻게 되고, defer가 호출되지 않는 경우를 설명하시오.

1. throw 구문의 오류를 던질 경우 

```swift
func test() throws -> Void {
    defer {
        print("test 1")
    }

    i enum testError {
        case error
    }
    
    throw testError.error
    defer { print("test 2") }
    
    print("test 3")
}

// print("test 1")
```



2. guard 문을 사용하여 중간에 함수를 종료하는 경우 

```swift
func example() {
    defer { print("1") } 
    guard false else { return }
    
    defer { print("2") }
}

example()

// print("1")
```



3. return 값이 Never(비반환함수)인 경우 

```swift
func test() -> Never {
    defer {
        print("test 1")
    }

    defer {
        print("test 2")
    }

    defer {
        print("test 3")
    }

    abort()
}
```

