# Week 11 - Fezz

## GCD API 동작 방식과 필요성에 대해 설명하시오.



### GCD (Grand Central Dispatch)

C언어 기반의 저수준 API(Aplication Progeam Interface)



#### Dispatch Queue

`GCD`는 Dispatch Queue를 사용하여 작업을 관리하며, Multi Threading을 지원한다. 

##### Serial Queue

<img src="https://blog.kakaocdn.net/dn/lsDJo/btqOYtFVKxm/X4dpXy6k9Vwj57v9RCPDj0/img.png" alt="img" width="50%;" /> 

Task(작업)들을 **순차적**으로 처리하며, **한번에 한 개의 Task** 밖에 처리 못함 

<br>

##### Concurrent Queue

<img src="https://blog.kakaocdn.net/dn/bmZmpd/btqO0ftQmqr/k3wGVnRYF31ncWVub3Exw0/img.png" alt="img" width="60%;" /> 

**동시에 여러 개의 Task**들을 처리할 수 있다.

<br>

#### Global Queue (Concurrent Queue)

#####QoS (Priority)

| **userInteractive** | 중요도가 높고 즉각적인 반응이 요구되는 작업(UI업데이트, 이벤트핸들링 등)일 때 사용 Main Thread에서 실행되는 Qos | 즉각적으로 실행          |
| ------------------- | ------------------------------------------------------------ | ------------------------ |
| **userInitiated**   | userInteractive까진 아니더라도 유저가 빠른 결과를 기대할 때 사용 저장된 파일을 열거나 할 때 사용 | 몇 초 이하나 거의 즉각적 |
| **default**         | 작업을 분리하지 않을 때 사용되는 Qos로 기본값임              |                          |
| **utility**         | 즉각적인 결과가 필요하지 않을 때 프로그레스 바가 등장하는 작업에 어울림 (네트워크, 다운로드, 계산, 데이터를 가져오기 등을 처리 등) | 몇 초 ~ 몇 분            |
| **background**      | 급히 필요하지 않은 작업일 때 사용자에겐 보이지 않는 처리 (ex: 백업) background로 줄 경우 iPhone 저전력 모드에선 실행되지 않는다 | 몇 분 ~ 몇 시간          |
| **unspecified**     | Qos 정보가 없음을 나타냄 시스템에게 Qos를 추론하라는 신호를 줌 |                          |

<bt>

---

📑 참고

[소들이 - iOS) GCD (Grand Central Dispatch)](https://babbab2.tistory.com/65?category=831129)