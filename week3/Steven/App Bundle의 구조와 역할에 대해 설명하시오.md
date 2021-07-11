# App Bundle의 구조와 역할에 대해 설명하시오.

### Bundle이란

- 묶음, 뭉치 → 뭔가 여러개가 함께 있는 느낌
- 앱 번들 → 앱을 실행하는데 필요한 파일 뭉치들..

## App Bundle

- 개발자들에 의해 생성되는 실행가능한 코드 파일과 그 코드가 사용하는 자원들

### Main Bundle

- 앱에 실행되는 코드가 있는 번들 디렉토리에 접근 할 수 있게 도와주는 번들임

```swift
print(Bundle.main.bundelURL) // 메인 번들 위치
```

## 앱 번들의 구조

- Info.plist -앱에 대한 구성(configuration) 정보(bundle ID, version number 등)를 포함한 파일
- Excutable -  모든 앱은 실행 가능 파일이 있어야 하고 앱의 메인 진입점과 정적으로 앱 타겟에 연결된 코드가 포함한다.
- Resources files - 앱의 실행가능한 파일 밖에 데이터 파일로 존재한다. 일반적으로 이미지, 아이콘, 사운드, nib 파일, 문자열 파일, 구성 파일 등의 것들로 구성된다. 대부분의 리소스는 특정 언어와 지역에 따라 localized될 수 있으며 localized된 파일은 lproj 확장자를 가진 파일로 저장된다.
- 거의 대부분 옵셔널하지만 항상 그런 것은 아니다. 예를 들어 iOS앱은 보통 추가적인 아이콘 이미지를 요구하기도 한다.

### 예시

```swift
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

- MyApp: 실행 파일이다. 반드시 있어야 한다.
- Info.plist: 앱에 대한 정보들을 가지고 있는 파일로 반드시 있어야 한다.
- png파일들: ios 앱 번들은 앱의 아이폰 이미지를 요구한다.
- MainWindow.nib: 앱이 launch time에 앱을 로드하기 위한 기본 인터페이스 오브젝트를 포함한다. 보통 앱의 메인 window 객체와 App delegate를 가지고 있다.

### 참고자료

[https://thoonk.tistory.com/64](https://thoonk.tistory.com/64)

[https://woongsios.tistory.com/92](https://woongsios.tistory.com/92)

[https://lidium.tistory.com/38](https://lidium.tistory.com/38)

[https://nshipster.co.kr/bundles-and-packages/](https://nshipster.co.kr/bundles-and-packages/)

[https://hcn1519.github.io/articles/2018-12/bundle](https://hcn1519.github.io/articles/2018-12/bundle)

[https://sihyungyou.github.io/iOS-app-bundle/](https://sihyungyou.github.io/iOS-app-bundle/)
