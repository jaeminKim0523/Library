# Method - Just
<img width="254" alt="스크린샷 2021-09-04 오후 5 01 33" src="https://user-images.githubusercontent.com/55477102/132087564-1bc47221-0f62-442b-8591-4e24aaf45860.png">  

Observable의 just 메소드의 Quick Help를 읽어보면, Observable sequence를 반환하는데 그게 Single element이다.  
단 한개의 Element를 보낸다는 것이다.  
Quick Help의 Declaration을 보면,  
```Swift
static func just(_ element: String) -> Observable<String>
```
테스트용으로 Generic의 타입을 String으로 선언했기 때문에 element의 타입이 String이며, 결국 파라메터를 보면 String 한개의 값만을 받도록 되어있다.  
즉, 한개의 값만 받아서 한번 전달하는 것이다.  

한번 예제 코드를 만들어보자.  
```Swift
let disposeBag = DisposeBag() 

let str = "First"
let observable: Observable<String> = Observable<String>.just(str)
observable.subscribe { (event) in
  print(event)
}.disposed(by: disposeBag)

// 출력: next(First)
// 출력: completed
```
상수 str에 First라는 String을 선언해주었고 str을 just 메소드에 넣어주었다.  
그러면, observable을 subscribe했을때 event에는 First가 들어오는 것이다.  
