# Week 11 - Fezz

## Hashable이 무엇이고, Equatable을 왜 상속해야 하는지

Hashable이란 `Set`, `Dictionary`의 Key와 같이 임의의 자료형을 정수의 값으로 변환하여 각가그이 인스턴스를 정수 값을 통해 접글할 수 있도록 해준다. 또한, Equatable 프로토콜을 준수하여야 하는데 이는 Hashable을 통해서 변환된 정수의 값을 바탕으로 인스턴스를 비교하여 찾아야 하기 때문이다.

