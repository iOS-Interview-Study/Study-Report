# Strong과 Weak 참조 방식에 대해 설명하시오

---



Strong 참조 방식은 데이터가 heap에 머물도록 강제하는 방법입니다. 인스턴스를 참조할 시 Reference counting이 증가합니다.



Weak 참조 방식은 데이터가 heap 에 머물되 메모리에 할당된 데이터가 nil일 수도 있다는 것을 내포하는 참조 방식입니다. 따라서 인스턴스를 참조할 시 reference counting이 증가하지 않습니다.



[[iOS Swift] RC, ARC 와 MRC 란? 그리고 Strong, Weak, Unowned 는? 간단하게 적어봤습니다.](https://medium.com/@jang.wangsu/ios-swift-rc-arc-%EC%99%80-mrc-%EB%9E%80-%EA%B7%B8%EB%A6%AC%EA%B3%A0-strong-weak-unowned-%EB%8A%94-%EA%B0%84%EB%8B%A8%ED%95%98%EA%B2%8C-%EC%A0%81%EC%96%B4%EB%B4%A4%EC%8A%B5%EB%8B%88%EB%8B%A4-988a293c04ac)

[iOS) 메모리 관리 (2/3) - strong , weak, unowned, 순환 참조](https://babbab2.tistory.com/27?category=831129)

