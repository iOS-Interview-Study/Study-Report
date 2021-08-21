# defer의 의미, 호출되는 순서, 호출되지 않는 경우와 특징을 설명하시오.
## defer
> 실행 구문이 현재 scope를 벗어날 때 실행하도록 하는 코드 블럭

```swift
func updateImage() {
    defer { print("Did update image") }

    print("Will update image")
    imageView.image = updatedImage
}

// Will update Image
// Did update image
```

## 호출 순서
`defer` 구문은 스택 구조로 후입선출 (Last-In First-Out; LIFO) 방식으로 코드 블럭이 실행됩니다.
```swift
func deferMultiplePrints() {
    defer { print ("3") }
    defer { print ("2") }
    defer { print ("1") }
}
deferMultiplePrints() // outputs 1,2,3
```

중첩해서 사용한 경우 가장 바깥의 `defer`부터 실행됩니다.
```swift

func nestedDefer() {
    defer {
        defer {
            defer {
                print("defer #3")
            }
            print("defer #2")
        }
        print("defer #1")
    }
}

// prints defer #1
// prints defer #2
// prints defer #3
```

## 호출되지 않는 경우
### defer를 읽기 전에 함수가 종료된 경우
1. throw를 이용하여 에러를 던진 경우
메서드가 에러를 던지는 경우 에러를 던지는 코드 이후에 선언된 `defer`문은 호출되지 않습니다.
```swift
func deferThrow(isError: Bool) throws -> Void{
    defer { print("test 1") } // 종료 직전에 호출됨

    if isError {
        throw TestError.error
    }

    defer { print("test 2") } // 호출되지 않음
    print("test 3") // 호출되지 않음
}

deferThrow(isError: true)
```

2. guard문을 통해 메서드가 종료된 경우
앞서 살펴본 `throw`의 예시와 마찬가지로 `guard`도 `guard`문 이후에 작성된 `defer`문은 호출되지 않습니다.
```swift
func deferGuard(string: String?){
    defer { print("test 1") } // 종료 직전에 호출됨

    guard let str = string else { return }

    defer { print("test 2") } // 호출되지 않음
    print("test 3") // 호출되지 않음
}

deferGuard(string: nil)
```

### 반환 타입이 비반환함수인 `Never`인 경우
에러가 발생하며 함수를 반환하지 않고 실행을 종료하여 `defer`문이 실행되지 않습니다.
```swift
func deferNever() -> Never {
    defer { print("test 1") }
    defer { print("test 2") }
    defer { print("test 3") }

    abort()
}
```

## 호출은 되지만 실질적으로 의미 없이 사용된 경우
아래와 같이 `defer`문 안에서 변수를 변경하는 경우 `return`에 선언된 변수를 읽는 시점보다 `defer` 코드 블럭이 이후 시점이기에 실질적으로 의미 없는 코드 블럭이 됩니다.

```swift
var hello: String = "Hello"

func attachWorld() -> String {
    defer { hello.append(" world") }
    return hello // 이 시점의 hello는 defer가 실행되기 이전의 hello임
}

print(attachWorld()) // prints Hello
```

## 특징
1. defer 구문이 선언된 시점의 변수를 Capture하지 않습니다.
```swift
func deferCapture() {
    var i = 1
    defer { print (i) }
    i += 1
}
deferCapture() // outputs 2
```

2. Scope 밖에서 실행될 수 없습니다. 따라서 `defer` 블럭 내에서 에러를 던질 수 없습니다.
```swift
func errorFunction() throws {
    defer { throw }
}
try errorFunction() // outputs the error below
```

## 용례
함수가 완료되어 변수나 상수와 같은 리소스들을 깨끗이할 필요가 있을 경우 사용할 수 있습니다.
```swift
// 예시 1. 수정이 완료된 후 파일을 종료
func writeFile() {
    let file: FileHandle? = FileHandle(forReadingAtPath: filepath)
    defer { file?.closeFile() } // 

    // Write changes to the file
}

// 예시 2. 파일 작업 후 NSLock 해제
let someLock = NSLock()

func fetchData() {
    someLock.lock()
    defer { myLock.unlock() }
    
    if data == nil { return }
    
    // data를 이용한 작업
}
```

# 참고자료
- [Using the defer Keyword in Swift](https://medium.com/swlh/using-the-defer-keyword-in-swift-b7916fa40f26) - Steven Curtis
- [Defer usage in Swift](https://www.avanderlee.com/swift/defer-usage-swift/) - SwiftLee
- [How “defer” operator in Swift actually works](https://medium.com/@sergeysmagleev/how-defer-operator-in-swift-actually-works-30dbacb3477b) - Sergey Smagleev
- [Swift) defer를 알아보자](https://babbab2.tistory.com/80) - 개발자 소들이