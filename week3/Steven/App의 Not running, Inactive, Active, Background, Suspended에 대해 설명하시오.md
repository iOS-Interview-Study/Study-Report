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
