# 강한 순환 참조 (Strong Reference Cycle) 는 어떤 경우에 발생하는지 설명하시오.

---



**강한 순환 참조는 두 개의 객체가 서로가 서로를 참조하고 있을 때 발생합니다**

``` swift
class Man {
    var name: String
    var wife: Woman?
    
    init(name: String) {
        self.name = name
    }
    
    deinit {
        print("Man died")
    }
}
```

``` swift
class Woman {
    var name: String
    var husband: Man?
    
    init(name: String) {
        self.name = name
    }
    
    deinit {
        print("Woman died")
    }
}
```

```swift
let Jake: Man = Man(name: "Jake") // Jake RC + 1
let Suzy: Woman = Woman(name: "Suzy") // Suzy RC + 1
```

```swift
Jake.wife = Suzy 
Suzy.husband = Jake
```

`wife` 프로퍼티에 Man 인스턴스의 주소값을 할당하여 Man의 RC + 1

`husband` 프로퍼티에  Woman 인스턴스의 주소값을 할당하여 Woman의 RC + 1



두 개의 객체가 서로를 참조하고 있습니다.

```swift
Jake = nil // Jake RC == 1
Suzy = nil // Suzy RC == 1
```

때문에 Jake와 Suzy 둘 다 삶을 마감해도 이혼을 하지 않았기 때문에 결혼은 쭉 유지되는?!거죠..ㅎ

왜냐하면 Jake와 Suzy를 가리키던 인스턴스가 힘에서 사라지지 않고 메모리에 올라와 있기 때문이죠



[참고]:

[iOS) 메모리 관리 (2/3) - strong , weak, unowned, 순환 참조 - 소들이](https://babbab2.tistory.com/27)

