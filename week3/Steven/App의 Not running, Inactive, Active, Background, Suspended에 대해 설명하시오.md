# App의 Not running, Inactive, Active, Background, Suspended에 대해 설명하시오.

![1](https://user-images.githubusercontent.com/35272802/125194020-6272ff80-e28a-11eb-9cf7-6cf1556361d2.png)

![2](https://user-images.githubusercontent.com/35272802/125194014-5e46e200-e28a-11eb-85d7-ea8c87fbcf17.png)

### Active

- 앱이 실행 되어서 메인 루프가 돌고 있는 상태
- 사용자의 이벤트를 받고 처리할 수 있는 상태
- 앱이 foreground에 있는 상태

### Inactive

- 앱이 foreground 상태에 있지만 사용자의 이벤트를 받을 수 없는 경우
- 제어 센터나, 알림, app switcher를 사용할 때의 상태

### Background

- 앱이 백그라운드에서 실행되고 있는 상태
- 음악 재생이나 파일 다운로드등에 작업이 이루어짐

### Suspended

- 앱이 백그라운드에 있지만 실행되고 있지 않는 상태
- 아직 메모리에 남아 있지만 메모리가 부족하면 시스템에 의해 제거될 수 있음.

### Not Running

- 앱이 아직 실행되지 않거나, 시스템에 의해서 종료된 상태

# 접근 제어자의 종류엔 어떤게 있는지 설명하시오.

![3](https://user-images.githubusercontent.com/35272802/125194026-69017700-e28a-11eb-8017-40d4a1f9f179.png)

### 모듈이란

- 코드들의 집합
- 프레임워크, 라이브러리, 프로젝트등에 해당 됨.
- ios에서는 모듈을 가져와서 사용하고 싶으면 `import` 해야 함!

## private

- 선언한 중괄호 안에서만 접근이 가능한 제어자
- 클래스, 구조체의 프로퍼티의 주로 사용 됨.

## fileprivate

- `.swift` 파일 안에서만 접근이 가능한 제어자

## internal

- defalut 접근 제어자
- 모듈 안에서는 접근이 가능한 제어자
- 프로젝트도 하나의 모듈이기 때문에 프로젝트 안에서는 모두 접근 가능!

## public

- 모듈의 외부에서 접근이 가능한 제어자
- 하지만 모듈 외부에서 상속은 불가능

## Open

- 모듈의 외부에서 접근이 가능한 제어자
- 모듈 외부에서 상속 가능
- 예시로 우리가 `UIViewController` 를 상속받아서 사용할 수 있음.

### 참고자료

[https://jinshine.github.io/2018/05/23/Swift/7.접근제어(Access Control)/](https://jinshine.github.io/2018/05/23/Swift/7.%EC%A0%91%EA%B7%BC%EC%A0%9C%EC%96%B4(Access%20Control)/)

[https://baked-corn.tistory.com/80](https://baked-corn.tistory.com/80)
