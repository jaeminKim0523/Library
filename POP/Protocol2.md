# Protocol - 2장
protocol에 extension을 이용하여 완성된 함수를 작성하는 방법을 알아보자.

### ptorocol extension
클래스와 구조체에 extension을 사용하는 것은 자주 보았을 것이다.  
우리는 protocol에 extension을 사용해 볼 것이다.  

```Swift
protocol TestProtocol {
}

extension TestProtocol {

}
```
어쩌면 당연하겠지만, 위의 코드에서 볼 수 있듯 클래스와 구조체 처럼 프로토콜도 extension을 사용 할 수 있다.  
그러면 프로토콜에 extension을 왜 사용할까?  

2장에서 알아보고자 하는 내용을 보면 알겠지만, 애초에 protocol안에서는 완성된 함수를 만들 수 가 없다.  
protocol 안에는 상속하고자 하는 구조체나 클래스에서 사용해야만 하는 변수, 상수, 함수의 선언부만 작성이 가능하고 이외의 추가 작업을 하게되면, 오류가 발생한다.  
하지만 프로토콜 지향 언어인 Swift에서는 extension을 이용하면, 프로토콜에도 완성된 함수를 작성 할 수 있다.  

서론은 여기까지하고 바로 ptorocol extension을 이용한 함수 작성을 해보도록 하자.  
```Swift
protocol TestProtocol {
    // 프로토콜 내부에는 아무 코드도 작성하지 않았다. 
}

extension TestProtocol {
    func printTestString() {
        // Hello world를 출력한다.
        print("Hello world")
    }
}
```

protocol내부에 printTestString과 같은 함수를 작성했다면, 오류가 발생하였겠지만 extension의 내부에 작성하면 오류가 발생하지 않는다.  
위에서 작성한 TestProtocol을 구조체에 상속시켜서 사용해보자.  
```Swift
let test = TestStructure()
test.printTestString()
```

<img width="198" alt="스크린샷 2022-08-07 오후 8 27 39" src="https://user-images.githubusercontent.com/55477102/183288328-499bea89-4463-4977-8137-0cb7220b2e1d.png">
위와 같이 Hello world가 출력되는 것을 볼 수 있다.  

비록 이번장에서는 간단하게 문자열을 출력하는 것으로 알아보았지만, protocol extension에 함수를 작성해서 상속시키는 방식은 정말 많이 사용되니 알아두는게 반드시 필요하다.  
