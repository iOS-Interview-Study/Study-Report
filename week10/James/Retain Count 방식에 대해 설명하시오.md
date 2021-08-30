# Retain Count 방식에 대해 설명하시오.

---



ARC 에서 RC를 Reference Count라고 부르고

MRC에서 RC는 Retain Count라 부릅니다.



## RC의 특징

### 참조 계산 시점

- Compile 시점에 언제 참조되고 해제되는지 결정되고 Runtime 때 실행됩니다.



### RC의 장점

- 개발자가 참조 해제 시점을 확인할 수 있습니다.
- Runtime 시점에 추가 resource가 발생되지 않습니다.



### 단점

- 순환 참조 발생 시 메모리가 영구적으로 해제되지 않을 수 있습니다.





수동으로 retain Count 하는 방식은 이렇게 진행될 수 있습니다.



## Retain Count 방식에 대한 간단한 설명

**1. 새로운 인스턴스를 생성할 경우 RC가 자동으로 증가하게 됩니다.**

```objc
University Class * university = [[UniversityClass alloc] init];
// university retain count == 1
```



**2, 기존 인스턴스를 참조하는 경우 RC가 자동으로 +1 되지 않습니다. 따라서 `retain` 을 해주어야 합니다.**



```objc
KoreaUniversityClass *koreaUniversity = university
// university retain count == 1

[university retain]; // university retain count == 2
```

`objc` 의 retain 함수 자체가 자기 자신(인스턴스)의 주소값을 반환하기에 아래와 같이 좀 더 간단하게 사용해 볼 수 있습니다.

```objc
KoreaUniversityClass *koreaUniversity = [university retain];
// university retain count == 2
```



**3. 인스턴스 사용이 끝난 뒤 메모리에서 해제되어야 할 때 `release` 를 해주어야 합니다.**

```objc
// university retain count == 2

koreaUniversity = nil
// university retain count == 2

[koreaUniversity release];
koreaUniversity = nil
// university retain count == 1
```





[참고]:

[iOS) 메모리 관리 (1/3) - ARC(Automatic Reference Counting) - 소들이](https://babbab2.tistory.com/26?category=831129)
