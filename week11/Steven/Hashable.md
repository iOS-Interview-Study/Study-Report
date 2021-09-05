# Hashable이 무엇이고, Equatable을 왜 상속해야 하는지 설명하시오.

## Hash

![image](https://user-images.githubusercontent.com/35272802/132124420-0b6a1afe-a101-4725-ab97-97f3b64ef28c.png)

- 임의의 크기를 가진 데이터(key)를 고정된 크기의 데이터로 변화시켜 저장하는 것
- 키 데이터를 해쉬함수에 넣고 그것에 대한 결과 값을 해쉬로 둔다.
    - 10을 나누는 해쉬함수가 있다고 가정하자 15라는 키값이 들어오면 5라는 해쉬값에 저장한다.
- 검색과 저장에 매우 빠른 속도를 보여준다( O(1) )

## Hashable

- 해당 타입에 정수형 해쉬 값을 제공해주는 프로토콜이다.
- set이나 딕셔너리의 키 값에 커스텀 타입을 사용할 때 채택해야 한다.
- swift에서 제공하는 기본 타입들(String, Int, Double, Bool)은 모두 이미 채택되어 있다.

### Equatable을 상속해야하는 이유

- Set이나 딕셔너리의 키 값을 중복 되서는 안된다.
- 해당 해쉬 값이 아주 적은 확률로 중복이 될 수 있다. 그러므로 비교를 해야하기 때문에 Equatable 상속받는다.

### 참고자료

[https://power-overwhelming.tistory.com/42](https://power-overwhelming.tistory.com/42)

[https://velog.io/@wonhee010/Hashable](https://velog.io/@wonhee010/Hashable)
