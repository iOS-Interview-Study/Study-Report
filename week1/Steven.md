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
