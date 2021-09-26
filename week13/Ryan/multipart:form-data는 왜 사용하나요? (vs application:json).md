# multipart/form-data는 왜 사용하나요? (vs application/json)

## Overview
> *"multipart/form-data" content type은 파일, ASCII 형식이 아닌 데이터와 바이너리 데이터 제출(submit)에 사용된다.* - HTML 4.01 Specification (W3C)

[RFC2045](https://www.ietf.org/rfc/rfc2045.txt)에 소개된 모든 multipart Multipurpose Internet Mail Extensions (MIME) 데이터 스트림의 규칙을 준수합니다.

하나의 multipart/form-data 메시지는 [successful control](https://www.w3.org/TR/html401/interact/forms.html#successful-controls)이라 표현하는 여러 파트로 이루어져 있으며 각 파트는 기본값을 `text/plain`으로하는 `Content-Type`을 가질 수 있습니다.

각 파트는 아래 내용을 가져야 합니다.
1. `"form-data"` 값을 가지는 `"Content-Dispoisition"` 헤더
2. 상응하는 control을 지칭하기 위한 control name

```swift
Content-Disposition: form-data; name="mycontrol"
```

## 예시
HTML 4.01 Specification에서 설명한 내용은 어떻게 이루어져 있는지 아래 예시를 통해 확인하실 수 있습니다.

배경지식으로 각 파트에서 등장하는 `\r\n`는 Carriage Return (CR), Line Feed (LF)를 의미하는`CRLF`이며 아래를 의미합니다.

1. Carriage Return (CR, `\r`): 현재 라인에서 가장 앞자리로 위치 이동
2. Line Feed(LF, `\n`): 커서의 위치를 바꾸지 않은 상태로 종이를 한 라인 위로 올림

### Initial and encapsulated part
multipart/form-data의 첫 파트와 중간 파트는 고유 메시지임을 나타내는 `boundary`와 control name을 가진 `content-disposition`, 그리고 값을 가지고 있습니다.

```swift
--ryanmarket.boundary-D5ECC632-707B-4D97-B046-5F69C4B342CA\r\n
Content-Disposition: form-data; name=\"price\"\r\n\r\n

12000\r\n
```

### Final part
마지막 파트는 메시지의 종료를 나타내기 위해 `--` 문자로 끝을 냅니다.

```swift
--ryanmarket.boundary-D5ECC632-707B-4D97-B046-5F69C4B342CA--\r\n
```


### Entire message
전체 메시지는 아래와 같이 구성되어 있습니다.

```swift
--ryanmarket.boundary-D5ECC632-707B-4D97-B046-5F69C4B342CA\r\n
Content-Disposition: form-data; name=\"price\"\r\n\r\n

12000\r\n
--ryanmarket.boundary-D5ECC632-707B-4D97-B046-5F69C4B342CA\r\n
Content-Disposition: form-data; name=\"currency\"\r\n\r\n

KRW\r\n
--ryanmarket.boundary-D5ECC632-707B-4D97-B046-5F69C4B342CA\r\n
Content-Disposition: form-data; name=\"descriptions\"\r\n\r\n

상품 상세 정보입니당~\r\n
--ryanmarket.boundary-D5ECC632-707B-4D97-B046-5F69C4B342CA\r\n
Content-Disposition: form-data; name=\"discounted_price\"\r\n\r\n

10000\r\n
--ryanmarket.boundary-D5ECC632-707B-4D97-B046-5F69C4B342CA\r\n
Content-Disposition: form-data; name=\"password\"\r\n\r\n

1234\r\n--ryanmarket.boundary-D5ECC632-707B-4D97-B046-5F69C4B342CA\r\n
Content-Disposition: form-data; name=\"images[]\"; filename=\"BD9B2CA6-F70D-46C5-AD02-540528441574.jpeg\"\r\n
Content-Type: image/jpeg\r\n\r\n

(data)\r\n
--ryanmarket.boundary-D5ECC632-707B-4D97-B046-5F69C4B342CA\r\n
Content-Disposition: form-data; name=\"images[]\"; filename=\"1CA0A2CF-BBEC-49AC-946A-D9A0D4014DE2.webp\"\r\n
Content-Type: image/webp\r\n\r\n

(data)\r\n
--ryanmarket.boundary-D5ECC632-707B-4D97-B046-5F69C4B342CA\r\nContent-Disposition: form-data; name=\"title\"\r\n\r\n

상품 이름이에요~!\r\n
--ryanmarket.boundary-D5ECC632-707B-4D97-B046-5F69C4B342CA\r\n
Content-Disposition: form-data; name=\"stock\"\r\n\r\n

100\r\n
--ryanmarket.boundary-D5ECC632-707B-4D97-B046-5F69C4B342CA--\r\n

```

## vs `application/json`
`multipart/form-data`를 이용하는 주된 이유는 가장 먼저 말씀드린 바와 같이 *파일, ASCII 형식이 아닌 데이터와 바이너리 데이터*를 업로드할 수 있는 형태이기 때문인 것으로 판단됩니다. 그렇다면 주로 활용되는 `application/json` 형식으로는 파일을 업로드할 수 없을까요?

파일의 컨텐츠를 `base64` 인코딩한 후 아래와 같이 구성하여 메시지를 보내면 업로드할 수 있습니다.

```json
EXAMPLE 9: Files
<form enctype='application/json'>
  <input type='file' name='file' multiple>
</form>

// assuming the user has selected two text files, produces:
{
    "file": [
        {
            "type": "text/plain",
            "name": "dahut.txt",
            "body": "REFBQUFBQUFIVVVVVVVVVVVVVCEhIQo="
        },
        {
            "type": "text/plain",
            "name": "litany.txt",
            "body": "SSBtdXN0IG5vdCBmZWFyLlxuRmVhciBpcyB0aGUgbWluZC1raWxsZXIuCg=="
        }
    ]
}
```

### `application/json`
- 장점:
  - 파일과 함께 전달된 데이터의 연관 관계를 표현하기 용이하다.
- 단점: 
  - 인코딩 방식의 특성으로 인해 바이너리 데이터보다 33-37 % 많은 용량을 차지한다.
    (base64 인코딩 방식이 인코딩 시 바이트 당 8 비트를 차지하는 바이너리 방식과 달리 6 비트를 차지하여 이론상 8/6 = 1.33, 약 33 % 많은 용량을 차지하지만 실제로는 패딩 등의 이유로 3~4 % 더 차지함)
  - 서버는 디코딩, 클라이언트는 인코딩을 위한 프로세싱 오버헤드가 부가된다.
  - 인코딩한 파일 컨텐츠의 문자열이 길어져 서버측에서 파일을 읽지 못하는 경우가 발생할 수 있다.
  
### `multipart/form-data`
- 장점:
  - 파일 업로드 시 `application/json` 방식에서 발생하는 단점을 감수하지 않을 수 있다.
- 단점:
  - 파일과 함께 전달된 데이터의 연관 관계 표현이 제한된다.

# 참고자료
- [HTML 4.01 Specification](https://www.w3.org/TR/html401/interact/forms.html#h-17.13.4.2) - W3C
- [What does enctype='multipart/form-data' mean?](https://stackoverflow.com/questions/4526273/what-does-enctype-multipart-form-data-mean) - Stack overflow
- [W3C HTML JSON form submission](https://www.w3.org/TR/html-json-forms/) - W3C Working Group Note
- [HTTP multipart/form-data 이해하기](https://lena-chamna.netlify.app/post/http_multipart_form-data/) - 레나 참나 블로그