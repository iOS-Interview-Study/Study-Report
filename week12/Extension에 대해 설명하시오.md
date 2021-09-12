# Extension에 대해 설명하시오.

---



## Extension이란?

Extension은 기존에 존재하는 클래스, 구조체, 열거형, 프로토콜 타입에 추가적인 새로운 기능을 추가합니다.



### Extension을 통해 이러한 것들을 확장할 수 있습니다:

- 계산 인스턴스, 계산 타입 프로퍼티
- 인스턴스 메서드, 타입 메서드(static func & class func)
  - 변경가능한 인스턴스 메서드(mutating instance methods)
- 편리한 이니셜라이저(convenience initializer)
- 서브스크립트 정의
- 중첩타입 



### Extension의 제한(limitation)

- 저장 프로퍼티 추가
- 메서드 오버라이드(override)
- observer 프로퍼티 추가(기존 프로퍼티를 감시하는)
- designated initializer, deinitializer 추가



[참고]:

[The Swift Programming Language | Extensions](https://docs.swift.org/swift-book/LanguageGuide/Methods.html)