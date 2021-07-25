# App thinning 에 대해 설명하시오

---



## App thinning 이란?

애플리케이션이 기기에 설치될 때 앱스토어와 운영체제가 기기의 특성에 맞게 설치되도록 하는 설치 최적화 기술을 뜻합니다.



앱 시닝은 슬라이싱(slicing), 비트코드(bitcode), 그리고 주문형 리소스(on-demand resource)가 있습니다.



### Slicing이란?

![Screen Shot 2021-07-24 at 3.26.20 PM](https://raw.githubusercontent.com/inwoodev/uploadedImages/uploadedFiles/20210724152627.png)

Slicing은 다양한 기기와 다양한 OS버전에 필요한 앱 번들의 조각을 생성하는 작업입니다. 해당 조각은 타겟으로 하는 기기와 OS 버전의 실행가능한 아키텍처 그리고 리소스를 포함합니다.

개발자가 App Store Connect에 full version의 앱을 업로드 하면 앱스토어는 앱 기기와 버전에 따른 다양한 조각을 앱을 slicing 하고 사용자는 자신이 사용하고 있는 기기에 알맞는 앱 조각을 제공 받습니다.



### Bitcode란?

Bitcode는 기계 언어로 번역되기 전 단계의 중간 표현(intermediate representation)을 뜻합니다.

이 bitcode를 갖고 있는 앱은 App Store로 연결되고 App Store에서 기계 언어로 컴파일 됩니다. 이 bit code를 앱이 갖게되면 추후 필요시 Apple은 해당 앱의 업데이트 버전이 제출되지 않아도 업로드된 앱의 app binary(기계언어를 포함하는 파일)를 최적화 할 수 있습니다.



### On-Demand Resource란?

이미지와 사운드에는 on-Demand Resource의 좋은 예시입니다. 해당 리소스를 다운로드 받는 것을 앱스토어에서 관리하고 필요시 다운로드 받습니다:

이는 더 나은 사용자 경험을 제공합니다. 그 이유는

1. 앱의 크기가 작아지기 때문에 초기 앱 다운로드 속도가 비교적 더 빨라집니다.
2. On-demand resource의 다운로드는 사용자 필요시 백그라운드에서 진행됩니다.
3. OS는 불필요한 on-demand resource를 제거할 수 있기 때문에 필요한 저장공간이 줄어듭니다.

예를 들면 앱은 필요한 리소스를 레벨 또는 요청 단위로 나눌 수 있습니다.

- 유저가 특정 래밸로 진입시 필요한 리소스를 다운로드 받아서 유저에게 제공할 수 있습니다.

- 비슷한 맥락으로 유저가 인앱 결제 요청을 할 때 특정 리소스를 유저에게 제공할 수도 있습니다.

[참고]:

[Xcode Help | What is app thinning?(iOS, tvOS, watchOS)](https://help.apple.com/xcode/mac/current/#/devbbdc5ce4f)

