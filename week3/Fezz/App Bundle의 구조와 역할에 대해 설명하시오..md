# Week3 - Fezz

## App Bundle의 구조와 역할에 대해 설명하시오.

<br>

### Bundle

실행가능한 코드와 관련 리소스(앱 아이콘, 이미지 등)를 한 공간에 묶는 디렉토리 모음

 애플 공식문서에 따르면 번들은 실행가능한 코드와 관련 리소스(이미지, 사운드 등)을 한 공간에 묶는 파일시스템에 있는 `디렉토리`이다. iOS나 OS X 환경에서는 애플리케이션, 프레임워크, 플러그인, 그리고 다른 타입의 소프트웨어들이 번들이라고 할 수 있다. 즉, 번들이란 실행 가능한 코드와 코드에 의해 사용되는 리소스를 갖는 standardized, hierarchical 디렉토리 구조이다.

<br>

### Application Bundles

- 개발자들에 의해 흔히 생성되는 가장 흔한 번들
- 앱이 정상적으로 작동하기 위해 필요한 모든 것
- 플랫폼에 따라 상세한 구조는 다르지만 번들을 사용하는 방법은 동일

<br>

### Application Bundle에 포함되어 있는 파일들

- `Info.plist file`: 앱에 대한 구성(configuration) 정보(bundle ID, version number 등)를 포함한 파일
- `Excutable`: 모든 앱은 실행 가능 파일이 있어야 하고 앱의 메인 진입점과 정적으로 앱 타겟에 연결된 코드가 포함한다.
- `Resources files`: 앱의 실행가능한 파일 밖에 데이터 파일로 존재한다. 일반적으로 이미지, 아이콘, 사운드, nib 파일, 문자열 파일, 구성 파일 등의 것들로 구성된다. 대부분의 리소스는 특정 언어와 지역에 따라 localized될 수 있으며 localized된 파일은 `lproj` 확장자를 가진 파일로 저장된다. 거의 대부분 옵셔널하지만 항상 그런 것은 아니다. 예를 들어 iOS앱은 보통 추가적인 아이콘 이미지를 요구하기도 한다.
- `Other support files`: 커스텀 데이터 리소스를 iOS 앱 번들에 포함할 수 있지만 커스텀 프레임워크 또는 플러그인은 포함할 수 없다.

<br>

---



### iOS App Bundle structure

프로젝트 템플릿은 아이폰 또는 아이패드 앱의 번들에 대한 대부분의 필요한 작업을 Xcode에 의해 제공한다. 하지만 앱 번들 구조에 대한 이해는 커스텀 파일을 어디에 배치해야할지 결정하는데 도움을 준다.

iOS 앱의 번들 구조는 모바일 기기의 필요에 더 초점이 맞춰져 있다. 디스크 공간을 절약하고 파일 접근을 단순화하기 위해 적은 수의 외부 디렉토리가 있는 비교적 평평한 구조를 사용한다.

일반적인 iOS 앱 번들은 최상위 번들 디렉토리에서 앱 `excutable`과 앱에서 사용되는 `리소스`(앱 아이콘, 이미지, localized된 내용)로 구성된다.

아래는 MyApp이라는 앱의 구조이다. 서브 디렉토리로 존재하는 파일들은 localized 파일이다. 하지만 추가 서브 디렉토리에 리소스 또는 관련 파일을 추가할 수 있다.

```tex
   MyApp.app
   MyApp
   MyAppIcon.png
   MySearchIcon.png
   Info.plist
   Default.png
   MainWindow.nib
   Settings.bundle
   MySettingsIcon.png
   iTunesArtwork
   en.lproj
      MyImage.png
   fr.lproj
      MyImage.png
```

<br>

#### MyApp (필수)

- 앱의 코드를 포함하고 있는 실행가능한 파일이다. 
- .app 확장자를 뗀 것이 실제 앱 프로젝트의 이름과 같다.

<br>

#### Application icons (MyAppIcon.png, MySearchIcon.png, and MySettingsIcon.png)

- 앱 아이콘은 앱을 표시하는데 사용된다. 예를 들어 홈 스크린, 검색 결과 그리고 설정에서 앱이 앱의 아이콘으로 표시된다. 대부분의 경우 앱 아이콘을 꼭 포함해야 한다.

<br>

#### Info.plist (필수)

- bundle ID, 버젼 번호 등 앱에 대한 구성(configuration) 정보를 포함하고 있는 파일이다.

<br>

#### Launch images(Default.png)

- 앱의 시작 인터페이스를 보여주는 이미지이고 시스템은 제공된 런치 이미지 중 하나를 앱이 윈도우와 유저 인터페이스를 로드할 동안 임시로 사용한다. 만약 임시 런처 이미지가 없다면 검은 화면이 보여진다.

<br>

#### MainWindow.nib

- 앱의 main nib file은 앱 런치 시간에 앱을 로드하기 위한 기본 인터페이스 객체를 포함한다. 보통 앱의 메인 윈도우 객체와 앱 델리게이트 객체를 갖고 있다.

<br>

#### Settings.bundle

- 앱의 application-specific preferences를 포함하는 특별한 타입의 플러그인이다. 이 번들은 property list와 구성하기 윈한 다른 리소스 파일이 포함되어 있고 preference를 보여준다.

<br>

#### Custom resource files

- non-localized 리소스들은 최상위 디렉토리에 위치하고 localized 리소스는 language-specific 하위 디렉토리에 위치한다.

<br>

정리 ⚠️ 

> iOS App Bundle은 코드와 리소스 디렉토리의 구조를 통해 개발자가 앱 만드는 것을 도와준다.



출처: 

[thoonk](https://thoonk.tistory.com/64)

[유셩장](https://sihyungyou.github.io/iOS-app-bundle/)



