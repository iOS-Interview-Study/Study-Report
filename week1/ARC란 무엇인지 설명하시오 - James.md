# ARC란?

---

ARC는 Automatic Reference Counting의 약자로 

기존 object-c에서 Manual Reference Counting(MRC)을 통해 개발자가 직접 reference counting을 retain/release를 통해 관리 해야 하는 부분을 자동으로 관리 해 주는 방식입니다.

ARC는 더 이상 필요하지 않는 Class 또는 의 Instance를 메모리에서 해제하는 방식으로 동작합니다.

여기서 ARC가 관리하고 있는 메모리 영역은 Data, Heap, Stack, Code 영역 중 Heap 영역과 관련이 있습니다.

Heap 영역은 참조형 자료가 머무는 공간이고 개발자가 동적으로 할당하는 메모리 공간이기 때문에 관리가 필요합니다. heap 영역의 참조형 자료가 얼마나 참조 되어 있는지 counting 하고 동시에 메모리 할당 및 제거를 자동으로 해 주는 것이 바로 ARC 인 것입니다.

## ARC의 메모리 관리

- ARC는 **compile time**에 자동으로 retain과 release등의 코드를 적절한 위치에 삽입하는 방식으로 메모리 관리를 합니다.

- 삽입된 코드는 run time에 실행됩니다.
- `retain` 과 `release`를 통해 reference count를 증감 시키다 count가 되면 reference count가 0이 되면 `deinit`을 호출해서 메모리 해제를 시켜줍니다.

![img](https://raw.githubusercontent.com/inwoodev/uploadedImages/uploadedFiles/20210625182218.jpg)



## ARC의 메커니즘

> ARC의 메커니즘은 *Swift Runtime* 이라는 library에 구현되어있습니다. *Swift Runtime*은 동적 할당되는 모든 object를 [HeapObject](https://github.com/apple/swift/blob/4fd0671e542299d7805e41cf9426640ab3b399af/stdlib/public/SwiftShims/HeapObject.h) 라는 struct로 표현합니다. HeapObject에서는 Swift에서 객체를 구성하는 모든 데이터, 즉 reference count와 type meta data를 포함하고 있습니다.
>
> [[Swift] ARC 뿌시기 - naljin](https://sujinnaljin.medium.com/ios-arc-뿌시기-9b3e5dc23814)



## 종합 해 보자면

ARC란 Automatic Reference Counting의 약자이며 app 내 heap 영역에 저장 되는 class의 인스턴스 또는 closure와 같은 참조형 데이터의 메모리를 추적하고 관리 해 주는 기법입니다. ARC는 컴파일 시 `retain` , `release`와 같은 코드를 적절한 위치에 삽입하고 run-time 시 해당 코드를 실행하여 참조형 자료들이 얼마나 참조되고 있는지 counting  해 주고 이에 따라 메모리를 할당 및 제거 해 줍니다. 그리고 reference counting이 0이 되면 deinit을 시켜서 메모리를 해제 시켜줍니다.



[참고]:

https://github.com/soleJin/TIL/blob/main/Swift/210411ARC.md

[[Swift] ARC 뿌시기 - naljin](https://sujinnaljin.medium.com/ios-arc-뿌시기-9b3e5dc23814)

[Swift - Automatic Reference Counting](https://docs.swift.org/swift-book/LanguageGuide/AutomaticReferenceCounting.html)