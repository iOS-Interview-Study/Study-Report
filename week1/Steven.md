# 1. hugging, resistance에 대해서 설명하시오.

### hugging

- 사전적 정의: 껴안다, 포옹하다
- 사전적 정의를 통한 유추: 뷰의 컨텐츠(라벨에서는 글씨)와, 뷰의 테두리(border)가 서로 껴안으려고 하는 힘.
- 라벨과 버튼의 경우 컨텐츠의 크기를 유지하려고 하는 힘.
- Hugging 우선 순위가 높은 뷰가 원래 상태를 유지하고 낮은 뷰는 오토레이아웃에 맞게 늘어난다.

### (compression) resistance

- 사전적 정의 : 압축, 저항
- 사전적 정의를 통한 유추: 뷰가 그것의 컨텐츠 보다 줄어드는 것을 저항하는 힘.
- resistance 우선순위가 높은 것은 컨텐츠가 줄어들이 않고, 낮은 것은 줄어들거나 심지어는 없어지기까지 한다.

# 2. Ios 앱 라이프 사이클

![Untitled](https://user-images.githubusercontent.com/35272802/123532671-24ce8c80-d74a-11eb-9fb8-287796ea6370.png)

## 앱의 상태

- Not Running - 앱이 아예 실행 되지 않았거나 완전히 종료되어 진 상태
- Inactive - 앱이 foreground 상태이지만 사용자의 이벤트를 받을 수 없는 상태
- Active - 앱이 foreground 상태에서 사용자의 이벤트를 받을 수 있는 상태
- Background - 앱이 background 상태에 있고 코드가 실행되고 있는 상태
- Suspened - 앱이 background 상태이고 메모리에 남아 있지만 코드가 실행되고 있지 않은 상태

## 앱이 Inactive 상태가 되는 경우

- 앱이 시작 되었을 때 Launch Screen 이 뜬 경우
- 제어센터나 알림창이 앱의 화면을 가린 경우
- 다른 앱의 알림이 기존에 사용 하던 앱을 가리는 경우
- 앱 전환 화면이 올라왔을 경우

# 3. ARC란

> Automatic Reference Counting

자동으로 참조를 카운팅을 한다.

### 설명

Swift에서는 클래스와 클로져가 참조 타입이다. 이것들은 힙 메모리에 저장된다. 힙 메모리에 저장되는 것들은 개발자가 직접 할당하고 해제하는 작업을 직접해주어야 하는데 이를 자동으로 해주는 녀석이 ARC이다.

### 예시

```swift
class Person {
  let name: String
  init(name: String) {
    self.name = name
    print("\(name) is being initialized")
  }
  deinit {
    print("\(name) is being deinitialized")
  }
}
```

- `사람` 이라는 클래스를 만들었습니다.
- 클래스의 인스턴스가 생성이 되면 `init` 이 호출 되면서 힙 메모리에 올라가게 됩니다.
- 인스턴스가 메모리에서 해제가 되면 `deinit` 을 호출합니다.

```swift
var reference1: Person?
var reference2: Person?
var reference3: Person?

reference1 = Person(name: "Steven")
reference2 = reference1
reference3 = reference1
// 이시점에서 Reference Counting은 3

reference1 = nil
reference2 = nil
reference3 = nil
```

- `reference1` 변수에서 이름이 스티븐인 인스턴스를 생성하고, 나머지 변수들은 이를 참조합니다.
- 그리고 nil을 할당해줌으로 써 참조를 해체 합니다.
- 마지막인 `reference3` 해체하는 순간 `Person` 클래스의 `deinit` 함수가 호출이 되면서 메모리에서 내려갑니다.

```swift
// reference3 = nil
```

- 만약 마지막 변수를 참조 해제하지 않았다면 `deinit` 함수는 호출되지 않습니다.
- 아직 스티븐 인스턴스에 대한 참조 횟수가 1이 남아 있기 때문입니다.

> 예제 처럼 클래스 인스턴스를 변수에 할당 한 것을 "강한 참조"라고 한다.

- ARC는 강한 참조의 횟수를 관리하고 강한 참조 횟수가 0이 되면 자동으로 메모리를 해체한다.

> 그런데... 위 예제 처럼 모든 참조를 해제를 해도 여전히 인스턴스가 메모리에 남아 있는 경우가 있습니다.

## 클래스 인스턴스 사이의 강한 참조 순환(Strong Reference Cycles Between Class Instances)

### 강한 참조 순환이 걸려있는 경우

![_2021-06-24__4 37 51](https://user-images.githubusercontent.com/35272802/123532722-a9210f80-d74a-11eb-8996-e4dfb68c10b2.png)

공식 문서에 있는 사진(코드는 생략)

- `사람` 클래스와 `아파트` 클래스가 서로가 서로를 참조하고 있는 상황이다.

![Untitled-2](https://user-images.githubusercontent.com/35272802/123532728-b3dba480-d74a-11eb-8ad3-31fa995e6b6a.png)

- `john` 과 `unit4A` 변수에서 참조를 해체해도 각 인스턴스가 서로를 참조하고 있어서 메모리가 해체되지 않는다. → 그러면 메모리의 누수가 발생할 수도 있다.

## 해결하기

### 약한 참조(`weak`)

- 강한 참조은 참조 카운트를 1증가 하지만 약한 참조는 증가시키지 않습니다.
- 무조건 변수(`var`) 그리고 옵셔널로 선언 해야 합니다.
    - 이유는 참조하고 있는 인스턴스가 해제 되면 자동으로 `nil` 을 할당해야하기 때문입니다.
- 인스턴스의 수명이 짧을 것으로 예상되는 변수에 선언해야 한다.

![Untitled-3](https://user-images.githubusercontent.com/35272802/123532731-b938ef00-d74a-11eb-9a1d-48e025fa6afd.png)

- 위(강한 참조 순환 문제) 예시와 다른 점은 `아파트` 클래스의 `거주자` 프로터티를 **약한 참조**로 선언을 했습니다.

![Untitled-4](https://user-images.githubusercontent.com/35272802/123532735-c3f38400-d74a-11eb-9039-b84da6debdb0.png)

- 그리고 `john = nil` 을 해주게 되면 `사람 인스턴스`는 참조 카운트가 0이 되어서 메모리에서 내려오게 됩니다.
    - 그러면 `tenant` 프로퍼티도 참조하고 있는 것이 없게 됨으로 `nil` 이 할당 됩니다.

### 미소유 참조(`unowned`)

- 약한 참조와 거의 비슷하다.
- 차이점은 인스턴스를 참조하는 도중에 그 인스턴스가 절대로! 사라질수 없다고 생각할 때 선언해야한다.
    - 그래서 미소유 참조는 꼭 옵셔널이 아니여야 한다.(swift5에서는 옵셔널로도 선언 가능)
    - 상수(`let`)로 선언을 해도 된다.
    - 만약 미소유 참조가 가리키고 있는 인스턴스가 해제 되었는데.. 그 변수를 사용하려고 하면 에러가 나면서 프로그램이 종료된다.
- 약한 참조와 다르게 수명이 길 것으로 예상되는 변수에 선언해야 한다.
