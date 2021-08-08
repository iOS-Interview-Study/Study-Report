# Delegate 패턴을 활용하는 경우를 예를 들어 설명하시오.

![simulator_screenshot_33C362C5-5221-49E1-AA9A-1928085481C8](https://raw.githubusercontent.com/inwoodev/uploadedImages/uploadedFiles/20210801145521.png)

상품명이라는 textField에서 작성을 마친뒤 해당 필드에 key값을 부여한 뒤 key와 value를 ViewController의 dictionary에 저장하고자 하면 어떤 방법이 있을까요?

View와 ViewController의 소통방법에는 여러가지가 있지만 그 중 제가 생각하기에 가장 적합한 방법은 delegate 패턴을 활용하는 것입니다.



Delegate 패턴은 쉽게 말해 **대신 처리해줄 객체** 와 **처리하라고 시키는 객체** 를 구현한 뒤 해당 객체들 간의 소통을 통해 어떤 객체가 해야하는 일을 델리게이트가 부분적으로 확장해서 대신 처리를 하는 패턴이라고 이해할 수 있습니다.





``` swift
protocol TextFieldConvertible: AnyObject {
    func convertTextFieldToDictionary(_ itemToPost: OpenMarketItemToPost, _ text: String?)
  
}
```

먼저 TextFieldConvertible이라는 프로토콜을 선언한 뒤 필요한 메서드를 정의합니다.

그 뒤 titleTextField라는 커스텀 `UITextField`  를 구현한 뒤 해당 클래스 안에서 데이터를 dictionary로 바꾸는 일을 **대신 처리해줄 객체를 구현해야 합니다.**



먼저 일을 시키는 객체를 아래와 같이 구현 해 보았습니다.

``` swift
class TitleTextField: UITextField {
    
    weak var textFieldConvertDelegate: TextFieldConvertible?
    
    override init(frame: CGRect) {
        super.init(frame: frame)
        self.placeholder = OpenMarketItemToPost.title.placeholder.description
        self.textColor = .black
        self.font = UIFont.preferredFont(forTextStyle: .body)
        self.translatesAutoresizingMaskIntoConstraints = false
        self.delegate = self
    }
    required init?(coder: NSCoder) {
        super.init(coder: coder)
    }
}
extension TitleTextField: UITextFieldDelegate {
    func textFieldShouldReturn(_ textField: UITextField) -> Bool {
        textField.resignFirstResponder()
        return true
    }
    
    func textFieldDidEndEditing(_ textField: UITextField) {
        textFieldConvertDelegate?.convertTextFieldToDictionary(OpenMarketItemToPost.title, textField.text)
    }
}
```
textField에 유저가 입력을 마친 다음 textField의 값을 변환 해 주고 싶기 때문에 `textFieldDidEndEditing` 메서드 안에서 textFieldComvertDelegate에게 일을 시키는 것을 볼 수 있습니다.



``` swift
weak var textFieldConvertDelegate: TextFieldConvertible?
```

*위 프로토콜 프로퍼티는 대신 처리 해줄 객체를 저장해 둘 프로퍼티입니다.



그리고 일을 대신할 OpenMarketItemViewController 옆에 `TextFieldConvertible` 라고 채택 작업을 진행 해줘야 합니다.

일을 대신할 객체(viewControleller)는 프로토콜에 적혀진 규약에 따라서 `convertTextFieldToDictionary` 메서드를 갖고 있게 되고 해당 메서드 속에서 TitleTextField를 대신해서 데이터를 딕셔너리로 변환 해 주는 작업을 구현했습니다.

``` swift
extension OpenMarketItemViewController: TextFieldConvertible {
  func convertTextFieldToDictionary(_ itemToPost: OpenMarketItemToPost, _ text: String?) {
        
        guard let text = text else { return }
        if let number = Int(text) {
            itemInformation.updateValue(number, forKey: itemToPost.key)
            
        } else {
            itemInformation.updateValue(text, forKey: itemToPost.key)
        }
    }
}
```



마지막으로 ViewController에서 위임자를 정해주는 과정을 거쳐야 합니다.

``` swift
class OpenMarketItemViewController: UIViewController {
  private var itemInformation: [String: Any?] = [:]
  
  override func viewDidLoad() {
    titleTextField.delegate = self
    titleTextField.textFieldConvertDelegate = self
  }
  
}
```

`titleTextField.textFieldConvertDelegate = self`

이 작업을 쉽게 말해 대신 일해주는 객체가 누구인지 알려주는 과정입니다.

textField에서 딕셔너리로 변환하는 작업은 (내)가 할게라고 선언하는 것이라고 보면 됩니다.

그리고 여기서 나는 OpenMarketItemViewController 자신이기 때문에 `self` 를 할당 해 주는 것이고 이 작업을 통해 OpenMarketItemViewController가  `TitleTextField` 클래스 에서 선언 해 준 textFieldConvertDelegate로서 역할할 수 있게되고 TitleTextField에서 `convertTextFieldToDictionary` 작업을 실행할 수 있는 것입니다.

``` swift
    func textFieldDidEndEditing(_ textField: UITextField) {  textFieldConvertDelegate?.convertTextFieldToDictionary(OpenMarketItemToPost.title, textField.text)
    }
```



이렇게 Delegate 패턴을 활용하면 
