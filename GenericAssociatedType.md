# Generic과 AssociatedType
### Generic이란?
우리는 class, struct, function, method 등 여러가지 Reference와 Value를 개발 할 때 한가지 타입이 아닌 여러가지 타입으로 사용하고자 할 때가 종종 생기곤 한다.  
그때 이 타입을 한가지로 단정 짓지않고 사용시에 타입을 선언하는 방식을 Generic이라고 이해하면 될 것 같다.  

```Swift
class Test<T> {
    var myType: T
    
    init(value: T) {
        self.myType = value
    }
}

let test = Test<Int>(value: 0)
```
위의 코드처럼 나는 myType을 String, Double, Int로 단정하지 않고 선언시 Int로 사용하겠다고 선언하여, ```Test class```의 사용을 더욱 유연하게 하였다.  
우리는 Swift라는 프로토콜 지향 언어를 사용하고 있고 설계시에 Protocol에서 Generic 사용을 선언 할 수 있다.  

하지만 Protocol에서 Generic사용시 "Protocols do not allow generic parameters; use associated types instead" 라는 오류가 발생한다.  
Generic은 Protocol에서 불가능하고 AssociatedType을 사용해라 라는 오류이다.

그럼 AssociatedType을 사용하여 Protocol에 Generic을 도입해보도록 하자.

## AssociatedType
```Swift
protocol TestProtocol {
    associatedtype MyType
    
    var myValue: MyType { get set }
}
```
associatedtype을 MyType 선언하였다.  
이것은 MyType이라는 Generic과 같은 개념인 타입이 하나 생선된 것이다.  
그리고 MyType이라는 Type을 가진 myValue라는 값을 하나 선언하였다.  

TestProtocol을 따르는 class를 하나 선언하면 어떻게 될까?

```Swift
class Test: TestProtocol {
    typealias myType = type
}
```
최초로 myType이 사용해야하는 Type을 넣어주어야 한다.  
예를들면 String, Int, Double 혹은 class과 될 수 도 있고 Struct가 될 수 도 있다.    

```Swift
protocol TestProtocol {
    associatedtype MyType
    
    var myValue: MyType { get set }
}

class Test: TestProtocol {
    typealias myType = String
    
    var myValue: String = ""
}
```
위와 같이 TestProtocol을 상속 받아 class를 구성해보았다.  
Type을 정해주면 Protocol에서 MyType으로 선언되었던 값과 메소드 등이 해당 Type으로 선언된다.  
매우 편리한 기능이며 Generic에 이 기능을 함께 사용해보자.

```Swift
protocol ViewModelTestable {
    associatedtype MyType
    
    var viewModel: MyType { get set }
    
}

extension ViewModelTestable {
    mutating func setViewModelName(viewModel: MyType) {
        self.viewModel = viewModel
    }
    
    mutating func getViewModelName() -> MyType {
        return self.viewModel
    }
}

class ViewModel {
    var name: String = "KJM"
}

class TestView<T>: ViewModelTestable {
    typealias MyType = T
    
    var viewModel: T
    
    init(viewModel: T) {
        self.viewModel = viewModel
    }
}

let viewModel: ViewModel = ViewModel()
let view: TestView<ViewModel> = TestView<ViewModel>(viewModel: viewModel)
view.setViewModelName(viewModel: <#T##ViewModel#>)
```
적절한 예제일지 자신이 없지만, 예제를 만들어 보았다.  
MVVM으로 치면 ViewModel을 Type으로 넣어주어 View에서 다양한 ViewModel을 유연하게 선언하여 사용 할 수 있도록 예제를 만들어 보았다.
가장 마지막 줄을 코딩해둔 이유는 우리가 protocol에서 MyType으로 Type이 지정한 타입이 Generic으로 선언한 ViewModel타입으로 변한 것을 보여주기 위함이다.  

이처럼 associatedtype과 Generic, protocol을 적절히 섞어서 쓰면 편리하게 사용 할 수 있다.
