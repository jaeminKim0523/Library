# Protocol - 1장
프로토콜 사용 방법에 관하여 기초부터 하나씩 알아보려고 한다.
몇 장 까지 이어질지 모르지만, 이번 장에서 알아보고자 하는 것은 프로토콜 선언과 사용 방법이다.

## 프로토콜 선언
```Swift
protocol TestProtocol {

    var testString: String { get set }
    var testInteger: Int { get set }
    
}
```
프로토콜의 선언은 class, struct, enum 등과 다르지 않다.   
선언이 같다고 해서 사용이 같은건 아니니 오해하지 말자.  

## 프로토콜 사용
```Swift
class ViewController: UIViewController, TestProtocol {
    
    // MARK: - Variables
    var testString: String = ""
    
    var testInteger: Int = 0
    
    // MARK: - Lifecycle Methods
    override func viewDidLoad() {
        super.viewDidLoad()
        // Do any additional setup after loading the view.
        
    }
    
}
```
ViewController에 위에서 예제로 만든 Protocol을 상속시켰다.  
Protocol을 상속시키게 되면, 프로토콜에 선언된 변수, 함수 등을 선언해야한다.  
(선언 강제성을 제거하는 방법은 다음장에 설명하겠다.)  
  
어차피 강제로 선언하게 될 것이면, 프로토콜을 사용하지 않고 클래스나 구조체에 바로 작성하면 되는데 번거롭게 Protocol을 따로 구분하여 사용하는지 의문일 것이다.  
Protocol은 같은 Protocol을 상속받은 클래스와 구조체로 값을 전달 할 수 있다.  

```Swift
protocol TestProtocol {

    var testString: String { get set }
    var testInteger: Int { get set }
    
}

struct A: TestProtocol {
    var testString: String
    
    var testInteger: Int
}

struct B: TestProtocol {
    var testString: String
    
    var testInteger: Int
}

class ViewController: UIViewController {
    
    // MARK: - Variables
    var testStruct: [TestProtocol] = []
    
    // MARK: - Lifecycle Methods
    override func viewDidLoad() {
        super.viewDidLoad()
        // Do any additional setup after loading the view.
        
        let a = A(testString: "A", testInteger: 0)
        let b = B(testString: "B", testInteger: 1)
        
        testStruct.append(a)
        testStruct.append(b)
    }
    
}
```
위 처럼 서로 다른 구조체에서 같은 Protocol을 상속시켜 하나의 배열에서 구조체 전달이 가능해진다.  
