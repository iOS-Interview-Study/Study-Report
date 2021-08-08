# 앱이 foreground에 있을 때와 background에 있을 때 어떤 제약사항이 있나요?

---



## Foreground 제약사항

없지않나...



## Background 제약사항

- 오디오, airplay, gps, NewsStand, Bluetootn, pushNotification, backgroundFetch등을 제외한 사용자 이벤트 처리 외 다른 이벤트를 정상적으로 처리하기 어렵습니다(foreground task보다 더 낮은 자원할당을 받기 때문에 이와 같은 제약사항이 생깁니다.)

- 공유 시스템의 리소스가 해제됩니다. 
- 이미지, 디스크파일 및 임시 객체들이 해제가 됩니다.

[App Programming Guide for iOS - Background Execution(3)](https://wnstkdyu.github.io/2018/06/09/appprogrammingguidebackgroundexecution/)

[[iOS] (3) 앱이 foreground와 background에 있을때의 제약사항](https://lidium.tistory.com/45)

