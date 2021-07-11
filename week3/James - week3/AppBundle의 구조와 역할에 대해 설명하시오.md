# AppBundle의 구조와 역할에 대해 설명하시오

---



## Bundle이란?

> 디스크의 bundle 디렉토리에 저장되어있는 코드와 리소스의 데이터 표현을 뜻합니다.

``` swift
class Bundle: NSObject
```

해당 클래스는 `NSObject` 를 상속 받고 있습니다.



애플은 `bundle`을 활용하여 앱, 프레임워크, 플러그인, 그리고 여러 다른 타입의 컨텐츠를 나타냅니다.

`Bundle`은 하위 디렉토리에서 저장되어있는 리소스를 관리하고 이 `bundle`의 구조는 플랫폼 또는 타입에 따라 달라집니다. `bundle`  객체를 활용하여 객체의 구조를 알지못하여도 객체의 리소스에 접근할 수 있습니다. `bunldle` 객체는 아이템을 위치하고 bundle의 구조, 유저의 preference , 앱의 [국제화](https://m.blog.naver.com/scw0531/221718340342)[국가에 맞는 언어로 메세지 제공], 그리고 다른 관련된 요소를 제공합니다.

`bundle` 객체를 이용하는 전형적인 패턴은 아래와 같습니다.

1. 특정한 번들 디렉토리의 bundle 객체를 생성할 때
2. 필요한 리소스를 불러오거나 저장하기 위해 번들 객체의 메서드를 사용할 때
3. 다른 시스템의 API를 사용하여 리소스에 접근할 때



자주 사용되는 리소스는 번들 객체 없이도 열고 특정 위치에 저장될 수 있습니다. 예를 들면

*이미지를 로딩할 때 이미지는 asset에 저장하고 이 저장 되어있는 이미지를 `init(named:)` 와 같은 UIImage 또는 NSImage의 메서드를 사용할 수 있습니다. 비슷하게 문자열 리소스 또한 `NSLocalizedString`을 활용하여 전체 문자열`.string` 뿐 아니라 단편적인 문자열 또한 불러올 수 있습니다.*

#### 이와 같은 번들의 구조 덕분에 코드나 자원의 예측가능한 로딩이 가능 해지고 지역화와 같은 시스템적인 기능도 가능해 집니다.

### Finding and Opening a Bundle

번들 객체를 활용하여 번들에 존재하는 특정한 리소스에 접근할 수 있습니다. 리소스를 검색할 때 해당 리소스의 이름과 타입은 최소한 제공해야 합니다. 만약 리소스가 특정한 하위 디렉토리에 존재한다면 이 디렉토리 또한 명시할 수 잇습니다. 

해당 리소스에 접근한 뒤 번들 객체는 **URL 또는 경로 문자열을 제공하여 프로그래머가 해당 파일을 열 수 있게 해줍니다.**



### Locating Resources in a Bundle

번들객체는 아래와 같은 검색 패턴을 활용하여 디스크에 저장되어있는 리소스를 찾습니다.

1. 먼저 글로벌 리소스에 접근합니다.
2. 지역 특화된 리소스(유저 언어 preference에 기반한)
3. 언어 특화 리소스(유저의 prefernece에 기반한)
4. 개발 언어 리소스

만약 글로벌 버전의 리소스가 존재한다면 언어 특화된 리소스를 불러오지 않습니다.



[Bundle | Apple Developer Document](https://developer.apple.com/documentation/foundation/bundle)

### Usage of Bundles

Package는 유저 경험을 개선시키기위해 존재합니다. 반면에 `Bundle`은 개발자가 코드를 구현하고 구현된 코드를 운영체제가 접근할 수 있게 도와줍니다. 변들은 코드와 소프트웨어와 관련된 리소스의 체계를 규정하기 위한 기반을 제공합니다.



#### Localizing Your App

특정 알림 메세지, 또는 label과 같은 텍스트를 여러 나라의 언어로 표현하기 위해서 app의 localizing을 실행할 때 app 번들이 사용됩니다. 

앱 개발 시 유저에게 보여질 앱의 모든 문자열파일을 XML Localization Interchange FileFormat(XLIFF)로 변환한 뒤 지역화 팀에게 다양한 언어로 번역하기 위해 제공한 뒤 결과물을 추후에 받을 수 있습니다. 추가적인 지역화 단계는 개발자가 필요에 따라 스스로 진행해야 합니다.

![Screen Shot 2021-07-08 at 1.21.43 AM](https://raw.githubusercontent.com/inwoodev/uploadedImages/uploadedFiles/20210708012149.png)

제공된 결과물을 bundle의 api를 통해 접근할 수 있습니다. 다양한 언어를 프로젝트에 더한 다면 bundle api는 유저의 언어세팅 그리고 지역 세팅을 고려하여 가장 적합한 언어를 찾아 줍니다. 이렇게 적절한 검색이 가능 한 이유는 `NSBundle` 또는 `CFBundleRef` 타입과 같은 객체를 사용하기 때문에 가능합니다. 접근하고자 하는 파일의 위치는 유저의 언어설정과 번들이 지원하는 지역화에 따라 달라질 수 있기 때문에 bundle이 직접 파일에 접근하게 함으로서 프로그래머는 올바른 파일에 접근하는지 확신할 수 있게 됩니다.

[Localizing Your App | Internationalization and Localization Guide](https://developer.apple.com/library/archive/documentation/MacOSX/Conceptual/BPInternational/LocalizingYourApp/LocalizingYourApp.html)

[Accessing a Bundle's Contents | Bundle Programming Guide](https://developer.apple.com/library/archive/documentation/CoreFoundation/Conceptual/CFBundles/AccessingaBundlesContents/AccessingaBundlesContents.html#//apple_ref/doc/uid/10000123i-CH104-SW7)

### The Advantages of Bundles

번들은 개발자에게 아래와 같은 이점을 제공합니다.

- 번들은 파일시스템의 디렉토리이기 때문에 번들은 파일을 담고 있습니다. 그렇기 때문에 같은 파일에 존재하는 리소스를 관련된 인터페이스를 활용하여 접근할 수 있습니다.
- 번들 디렉토리 구조 덕분에 다양한 지역화를 지원하기 수월합니다. 지역화된 리소스 를 더하거나 필요없는 리소스를 지워서 관리할 수 있습니다.
- 번틀은 다양한 포맷을 지원합니다.
- 사용자는 번들을  finder로 부터 드래그해서 쉽게 삭제, 위지 재정의 그리고 설치할 수 있습니다.
- 번들은 다양한 칩(프로세서)에 호환되고 32비트, 64비트 모두 호환됩니다.
- 거의 모든 application, framework, 그리고 플러그인은 bundle 모델을 지원합니다.
- 번들 application은 서버로부터 다이렉트하게 실행될 수 있습니다.



### Types of Bundles

- **Application** - 애플리케이션 번들은 앱의 실행과 관련된 코드와 리소스를 관리합니다. 애플리케이션 번들은 실행될 수 있는 executable 리소스를 가지고 있고 이 executable을 설명하는 `Info.plist` 파일을 갖고 있습니다. executable에서 사용되는 런치 이미지를 포함한 asset,과 자원, interface file, string file, 그리고 data file로 이루어져있습니다.
- **Frameworks** - 프레임워크 번들은 동적 공유 라이브러리와 이와 관련된 코드와 리소스를 관리합니다. 
- **Loadable** - Loadable 번들은 image Unit plug-in, interface builder plug-in과 같이 애플리케이션의 기능성을 확장시켜주는 실행가능한 코드와 자원을 포함하고 있는 `plug-in`을 포함하고 있습니다.



## 결론

Bundle은 하위 디렉토리에 저장되어있는 리소스와 데이터를 표현하는 수단입니다. 다양한 번들이 존재하고 특정한 번들 객체를 생성하고 활용하여 해당 번들에 존재하는 특정한 리소스에 접근할 수 있습니다. 이 중 appBundle은 앱의 실행과 관련된 코드와 리소스를 관리합니다. app Bundle은 앱이 실행될 수 있는 image asset, interface file, string file, datafile, 그리고 앱관련된 여러 리소스를 가지고 있고 해당 리소스를 설명하는 `info.plist` 파일을 갖고 있습니다.



[About Bundles | Bundle Programming Guide](https://developer.apple.com/library/archive/documentation/CoreFoundation/Conceptual/CFBundles/AboutBundles/AboutBundles.html#//apple_ref/doc/uid/10000123i-CH100-SW1)

[Bundles and Packages - Matt](https://nshipster.co.kr/bundles-and-packages/)