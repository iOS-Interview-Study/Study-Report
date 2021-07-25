# App thinning에 대해서 설명하시오.

![](https://images.velog.io/images/ryan-son/post/3d81b66c-516d-4f80-b1e2-a7e81a56df00/image.png)
![](https://images.velog.io/images/ryan-son/post/f04c0cb3-c257-4bb3-bb79-c8dd98fa6264/image.png)

### App Thinning 이란?

- 애플리케이션이 디바이스에 설치될 때, 앱 스토어와 운영체제가 디바이스의 특성에 맞게 설치되도록 하는 설치 최적화 기술
- 최소한의 디스크 사용과 빠른 다운로드를 제공할 수 있음
- 앱 시닝은 슬라이싱(slicing), 비트코드(bitcode), 그리고 주문형 리소스(on-demand resource)가 있음

### Slicing 이란?

- 앱이 지원하는 여러 디바이스에 대해 각각 조각 애플리케이션 번들을 생성하고, 해당 디바이스에 가장 적합한 조각을 전달하는 기술
- 개발자가 App store connect에 업로드하면, 앱 스토어에서 디바이스 특성에 따라 다양한 버전의 조각들을 생성, 사용자가 그 조각 중에서 가장 알맞은 조각(app variant)을 다운로드 받는 것

![](https://images.velog.io/images/ryan-son/post/34d2f116-9765-4e13-a888-f8c956d4e1a8/image.png)

### Bit Code 란?

- 비트코드는 기계언어로 번역되기 이전 단계의 중간표현(Intermediate Representation)
- 현재 iOS에서는 옵션이나 기본 설정으로, 개발자가 프로젝트 옵션에서 선택할 수 있음
- 비트코드를 사용하여 업로드를 하면 애플이 애플리케이션을 재컴파일하여 앱 바이너리(app binary)를 생성합니다. 비트코드를 사용하지 않으면, 모든 경우의 디바이스 경우에 따라 바이너리로 생성하여 합쳐져서 fat binary라는 것이 업로드되지만, 비트코드를 사용하면 필요 경우에 따라 재컴파일하게 되므로 여기에서 최적화할 수 있습니다.

=  **아직 기계코드도 아니고 내가 이해 할 수 있는 코드도 아닌 중간단계의 코드**

애플은 다운로드되기전에 **디바이스에 맞게 앱을 최적화 하여 바이너리를 새로 만들어 제공,** Bitcode를 사용하면, 최신 컴파일러용으로 자동으로 앱을 컴파일하고, 특정 아키텍쳐(예 : iPhone 6 및 iPad Air 2와 같은 64 비트 프로세서의 경우 arm64)에 맞게 최적화 함

Bitcode는 다른 아키텍쳐에 대한 최적화를 "제거"하여 다운로드는 더 작게 만듬 / **관련 최적화만 다운로드하여** 위에 언급한 App Thining 기술과 함께 사용됩니다.

### On-Demand Resource 란?

쉽게 말해서, 필요할 때 다운로드 받는다는 것입니다. 예를 든다면 사용자가 게임을 합니다. 현재 레벨보다 상위레벨은 필요하지 않으므로 갖고 있을 필요가 없습니다. 사용자의 레벨이 필요할 때 다운로드 받는 것입니다. 또한 인앱 구매를 예로 들 수 있습니다. 사용자의 선택에 따라 다운로드를 받는 것입니다.

![](https://images.velog.io/images/ryan-son/post/1c117576-5733-46bf-994d-fec7c5a0329e/image.png)

앱스토어 상에서 일어나는 것

개발자가 올릴 때는 Universal App 올림

앱스토어에서 해당 기기에 맞는 에셋, 리소스들을 골라서 최적화된 ipa 를 다운받도록 함

# 출처
- [seonyoung169 notion](https://www.notion.so/App-thinning-6968cfd296a148f89a97d407673a6cae)
- [App Thinning in Xcode](https://developer.apple.com/videos/play/wwdc2015/404) - WWDC15
