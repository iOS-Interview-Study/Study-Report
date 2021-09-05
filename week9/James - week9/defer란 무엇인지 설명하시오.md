# defer란 무엇인지 설명하시오

---

## `defer`  는

함수 안에서 작성 되는 클로저입니다. 

이 클로저는 **작성된 위치와 상관 없이 함수 종료 직전에 실행된다는** 특징을 가지고 있습니다. 

``` swift
func openDocument() {
  defer {
    // openDocument() 메서드가 끝나기 직전에 실행되는 구문
  }
  
  self.document.open()
}
```



### defer를 언제 사용하면 될까?

함수를 종료하기 직전에 변수나 상수를 처리하기 위해 사용될 수 있습니다.

예를 들면 아래와 같이 NSLock을 활용하여 상호배제를 걸 때 함수 종료 전 lock이 걸린 경우 이 Lock을 풀어주어야만 deadlock에 걸리지 않게 됩니다.

``` swift
let myLock: NSLock = .init()

func fetchData() {
    myLock.lock()
    
    defer { myLock.unlock() }
    
    if data == nil { return }
    
    //이후 작업 ~.~
}

```

출처: [Swift) defer를 알아보자 - 개발자 소들이](https://babbab2.tistory.com/80)



### ⚠️주의사항⚠️

**`defer` 구문 내부에는 break, return 등과 같이 구분을 빠져나갈 수 있는 코드 또는 오류를 던지는 코드는 작성하면 안됩니다.**



[참고]:

스위프트 프로그래밍 - 야곰 pg510-511

# defer가 호출되는 순서는 어떻게 되고, defer가 호출되지 않는 경우를 설명하시오.

---



## 여러개의 defer 구문히 한 메서드에 있다면?

맨 마지막에 작성된 `defer` 구문부터 **역순**으로 실행됩니다.

```swift
func openDocument() {
  defer {
    self.document.close()
  }
  
  defer {
    self.document.save()
  }
  
  self.document.open()
}

// 코드 블록 내 실행 순서
// 1. self.document.open()
// 2. self.document.save()
// 3. self.document.close()
```



## 복합적인 defer 구문의 실행 순서

`do` 구문을 통해 메서드 내부에서 한 단계 하위 블록을 만들 수 있고 이럴 경우 모든 메서드가 호출되기 직전이 아닌 하위구문인  `do` 구문이 종료되기 직전에 전에 호출되게 됩니다.

```` swift
func openDocument() {
  defer {
    self.document.close()
    print("문서 메모리로부터 해제")
  }
  
  do {
    defer {
      self.document.prepare()
      print("문서 준비됨")
    }
    self.document.checkValidity()
    print("문서 유효성 검증")
  }
  
  print("문서를 열겠습니다.")
  self.document.open()
}

// 코드 블록 내 실행 순서
// 1. self.document.checkValidity() - 문서 유효성 검증
// 2. self.document.prepare() - 문서 준비됨
// 3. self.document.open() 문서를 열겠습니다.
// 4. self.document.close() 문서 메모리로부터 해제
````



## defer가 호출되지 않는 경우는?

#### **1. throw를 통해 오류를 던질 경우**

``` swift
func analyzeString(text: String?) throws -> String {
    defer {
        print("읽을 수 있는 문자열입니다.")
    }
    
    guard let verifiedtext = text else {
        print("손상된 텍스트입니다.")
        throw TextError.damagedtext
    }
    
    return verifiedtext
}
```

해당 메서드를 실행 할 때

```swift
analyzeString(text: nil)
```

이렇게 `nil` 값이 전달인자로 들어올 경우 `throw` 를 통해 에러를 반환하게되고 `defer` 에 도달하기 전에 함수가 종료되어서 `defer` 문이 호출되지 않습니다.



#### **2. guard 문을 통해 중간에 함수를 종료할 경우**

``` swift
func analyzeString(text: String?) -> String {
    defer {
        print("읽을 수 있는 문자열입니다.")
    }
    
    guard let verifiedtext = text else {
        print("손상된 텍스트입니다.")
        return TextError.damagedtext
    }
    
    return verifiedtext
}
```

해당 메서드의 전달인자로 `nil` 값이 전달인자로 들어올 경우 `throw`  문과 마찬가지로  `guard` 문을 통해 중간에 함수가 종료되고 그렇기 때문에 `defer` 에 도달하기 전에 함수가 종료되어서 `defer` 문이 호출되지 않습니다.



#### **3. 비반환함수(Never)가 리턴값인 경우**

**비반환 함수란?**

- 종료되지 않는 함수 즉 정상적으로 끝나지 않는 함수를 뜻합니다. 비반환 함수 안에서는 오류를 던지거나 중대한 시스템 오류를 보고하는 등의 일을 하고 프로세스를 종료 해 버립니다. 

``` swift
func crashAndBurn() -> Never {
  defer {
    print("끝나기 직전에 실행되어야하는데...")
  }
  fatalError("Something is wrong")
}
```

**위와 같이 `Never` 를 반환하는 비반환 함수인 경우 에러가 발생하면서 함수를 반환하지 않고 실행을 종료하기 때문에 defer가 호출되지 않습니다.**



## for 문 내에서 defer를 사용할 경우 iteration이 끝날 때 마다 defer가 실행될까요?

 `for` 문 의 코드 블록 내에서 iteration이 끝날 때 마다 안에 있는 `print(i)` 가 호출 되면서 숫자가 화면에 표시되는 것을 확인할 수 있습니다.

반복문이 종료된 후 상위 코드블록에 존재하는 `defer` 문이 호출되면서 메서드가 종료됩니다.

``` swift
import Foundation
func hi() {
    defer {
        print("finish greetings")
    }
    for i in 0...10 {
        defer {
            print(i)
        }
        print("hi")
    }
}

hi()
```

![Screen Shot 2021-08-22 at 2.52.24 AM](https://raw.githubusercontent.com/inwoodev/uploadedImages/uploadedFiles/20210822025931.png)



[참고]:

[defer 구문 알아보기 - 뀔뀔(swieeft)의 개발새발기](https://swieeft.github.io/2020/02/26/defer.html)

스위프트 프로그래밍 - 야곰 pg168(7.4 종료되지 않는 함수)
