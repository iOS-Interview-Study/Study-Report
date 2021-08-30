#### Retain Count 방식에 대해 설명하시오.

RC(Retain Count) 

- 참조 횟수, MRC의 메모리 관리 방식. 

- NSObject 클래스의 함수이며 ,객체의 Reference Count를 증가시킨다. 

- 해당 객체의 레퍼런스 카운트를 1 올리는 것을 말한다. 

- 객체가 메모리에서 해제되지 않도록 이 함수를 수동적으로 호출하여 Reference Count를 증가시킨다. 

- 함수 블록 내에서 alloc, retain, copy의 수와 release, autorelease의 수가 동일해야 한다. 

- factory 함수를 사용해 생성한 객체의 경우는 따로 release 해줄 필요 없이 대부분 autorelease로 반환되어진다. 

- 클래스의 인스턴스 변수들은 dealloc 메소드에서 다 release 해야 한다.

  

  https://sdw8001.tistory.com/102