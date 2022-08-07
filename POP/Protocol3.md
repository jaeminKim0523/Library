# Protocol - 3장
이번장에서는 클래스와 구조체에서 프로토콜을 사용 할 때 유용한 선언 강제성을 제거하는 방법을 알아볼것이다.

## Protocol의 선언 강제성 제거
```Swift
protocol TestProtocol {
    
    var testString: String { get set }
    var testInteger: Int { get set }
    
}

struct TestStructure: TestProtocol {

}
```
강제성 제거를 해보기 위해 [Protocol1]에서 간단하게 사용했던 Protocol과 structure를 선언했다.  

위의 경우에 TestStructure에서 아래와 같은 오류가 발생하게 된다.
<img width="580" alt="스크린샷 2022-08-07 오후 7 45 42" src="https://user-images.githubusercontent.com/55477102/183287191-3fb3988c-7182-495c-b731-fd11dd9ce9fd.png">

해당 오류를 해결하기 위해서는 Protocol에서 선언된 testString, testInteger를 TestStructure에 선언해야 한다.  
하지만, 우리는 해당 변수를 강제 하고 싶지 않을 수 있다.  

그 방법을 알보고자 하는데, 방법은 너무 쉽다.  
아래와 같이 protocol extension을 이용하면 쉽게 제거 할 수 있다.  
```Swift
protocol TestProtocol {
    
    var testString: String { get set }
    var testInteger: Int { get set }
    
}

extension TestProtocol {

    var testString: String {
        get {
            return "test"
        }
        set {
            self.testString = newValue
        }
    }
    
}

struct TestStructure: TestProtocol {
    
    var testInteger: Int

}
```
[Protocol2]에서 알아보았던 protocol extension을 이용하여 제거한다.  
extension에 미리 변수의 초기값을 생성해주고 구조체나, 클래스에서 해당 프로토콜을 상속하였을때 해당 구조체에서 값을 선언한 것과 같게 만든다.  

위의 방법은 함수에도 동일하게 적용된다.  

```Swift
protocol TestProtocol {
    
    func testFunction(text: String)
    
}

extension TestProtocol {

    func testFunction(text: String) {
        print(text)
    }
    
}

struct TestStructure: TestProtocol {

}
```
위와 같이 선언하면 TestProtocol을 상속하는 곳에서 testFunction을 선언하지 않더라도 오류가 나지 않도록 강제성을 제거 할 수 있다.


[Protocol2]: https://github.com/jaeminKim0523/Library/blob/main/POP/Protocol1.md ""
