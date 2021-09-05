# Hashable이 무엇이고, Equatable을 왜 상속해야 하는지 설명하시오.
## Hashable
[Hasher](https://developer.apple.com/documentation/swift/hasher)를 통해 Int 타입의 해쉬 값을 만들 수 있게 하는 프로토콜으로, 이를 준수하는 타입은 Set과 Dictionary의 키 값이 될 수 있습니다.

특정 인스턴스의 identity를 `hasher(into:)` 함수를 통해 Int 타입으로 나타내는 과정에서 결과 값인 해쉬 값이 동일한 인스턴스가 존재하지 않음을 보장하기 위해 `Hashable` 프로토콜은 `Equatable` 프로토콜을 상속하여 확장하는 형태로 구현 되어 있습니다.

# 참고자료
- [Why does Hashable require Equatable?](https://forums.swift.org/t/why-does-hashable-require-equatable/16817) - Swift forums
- [Equatable, Comparable, Hashable](https://jcsoohwancho.github.io/2019-10-27-Equatable,-Comparable,-Hashable/) - Rhyno's DevLife Log