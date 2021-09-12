# Week 12 - Fezz

## Extension에 대해 설명하시오.



### Extension

기존 클래스, 구조체, 열거형 타입에 새로운 Property, Method, Initlalizer 등을 추가하는 것으로, 

원본 타입(소스 코드)에 접근하지 못하는 타입들도 확장해서 사용할 수 있다.



### 토의

```swift
// Request
protocol MarketRequest: MulitPartFormConvert {
    func createRequest<T: Encodable>(url: URL?, 
                                     encodeBody body: T,
                                     method: MarketAPI.Method) -> Result<URLRequest?, Error>
    func createMultipartFormRequest<T: Encodable>(url: URL, 
                                                  encodeBody body: T,
                                                  method: MarketAPI.Method) -> URLRequest
}

extension MarketRequest {
    func createRequest<T: Encodable>(url: URL?, 
                                     encodeBody body: T, 
                                     method: MarketAPI.Method) -> Result<URLRequest?, Error> { ... }
    
    func createMultipartFormRequest<T: Encodable>(url: URL, 
                                                  encodeBody body: T, 
                                                  method: MarketAPI.Method) -> URLRequest { ... }
}

// MultiPartForm
protocol MulitPartFormConvert {
    func baseBoundary() -> String
    func createBody(dictionaryData: [String: Any?], _ boundary: String) -> Data
    func convertMulitPartForm(name: String, value: Any, boundary: String) -> Data
    func convertMulitPartForm(imageName: String, images: [Data], boundary: String) -> Data
}

extension MulitPartFormConvert {
    func baseBoundary() -> String { ...}
    func createBody(dictionaryData: [String: Any?], _ boundary: String) -> Data { ... }
    func convertMulitPartForm(name: String, value: Any, boundary: String) -> Data { ...}
    func convertMulitPartForm(imageName: String, images: [Data], boundary: String) -> Data { ... }
}

```

