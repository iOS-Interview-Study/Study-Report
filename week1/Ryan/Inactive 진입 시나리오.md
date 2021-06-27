# 앱이 In-Active 상태가 되는 시나리오를 설명하시오.

---

## Respond to Scene-Based Life-Cycle Events
![](https://images.velog.io/images/ryan-son/post/aa207055-876c-4a13-a0a2-f04ad14ecfc7/image.png)


## Respond to App-Based Life-Cycle Events
![](https://images.velog.io/images/ryan-son/post/7a27f1d3-b230-43f9-9872-29eab56b5fc0/image.png)

inactive

- App이 실행 중이지만 이벤트를 받지 않는 상태
- 다른 상태로 넘어가기 전에 앱은 반드시 이 상태를 거침
- 전화나 메시지 같은 interrupt 발생 시 
- 미리알림 같은 특정 알림창이 화면을 덮어서 앱이 실질적으로 event를 받지 못하는 상태 등이 여기에 해당