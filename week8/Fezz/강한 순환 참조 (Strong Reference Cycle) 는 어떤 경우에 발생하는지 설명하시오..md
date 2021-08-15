# Week8 - Fezz

## 강한 순환 참조 (Strong Reference Cycle) 는 어떤 경우에 발생하는지 설명하시오.

```swift
class Man {
    var name: String
    var girlfriend: Woman?
    
    init(name: String) {
        self.name = name
    }
    deinit { print("Man Deinit!") }
}
 
class Woman {
    var name: String
    var boyfriend: Man?
    
    init(name: String) {
        self.name = name
    }
    deinit { print("Woman Deinit!") }
}

var chelosu = Man(name: "철수")
var yeonghee = Woman(name: "영희")
```

Man에게는 Woman 타입의 프로퍼티(girlfriend)가 있고, Woman에게는 Man 타입의 프로퍼티(boyfriend)가 있다.

Man -> Woman

Woman -> Man



<img src="https://raw.githubusercontent.com/Fezravien/UploadForMarkdown/forUpload/img/img-20210815211413633.png" alt="img" style="zoom:50%;" />



```swift
chelosu?.girlfriend = yeonghee
yeonghee?.boyfriend = chelosu
```



<img src="https://raw.githubusercontent.com/Fezravien/UploadForMarkdown/forUpload/img/img-20210815191131794.png" alt="img" style="zoom:50%;" />

```swift
chelosu = nil
yeonghee = nil
```

<img src="https://raw.githubusercontent.com/Fezravien/UploadForMarkdown/forUpload/img/img-20210815191345007.png" alt="img" style="zoom:50%;" />



### 정리

|                        | **strong**                                            | **weak**                                           | **unowned**                                                  |
| ---------------------- | ----------------------------------------------------- | -------------------------------------------------- | ------------------------------------------------------------ |
| **Reference Counting** | O                                                     | X                                                  | X                                                            |
| **사용 시점**          | ▪ Default                                             | ▪ 강한 순환 참조가 발생할 경우                     | ▪ 강한 순환 참조가 발생할 경우  ▪ 참조하는 인스턴스가  먼저 메모리에서 해제될  가능성이 없는 경우 |
| 특징                   | ▪ 강한 순환 참조로 인해 Memory leak 이 발생할 수 있음 | ▪ 참조하던 인스턴스가 해제되면 자동으로 nil을 할당 | ▪ 참조하던 인스턴스가  먼저 메모리에서 해제되면,  해제된 주소값을 계속 들고 있음 (에러로 이어질 가능성 높음) |

<br>

---



## 참고

https://babbab2.tistory.com/27?category=831129