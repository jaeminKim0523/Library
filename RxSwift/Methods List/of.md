# Method - Of
of 메소드는 [just]와는 다르게 Number of elements이다.  
여러개의 Element를 파라메터로 넘겨주게 된다.  

바로 코드를 통해 사용해보자.  
```Swift
let disposeBag = DisposeBag()

let first = "First"
let second = "Second"
let third = "Third"

let observable: Observable<String> = Observable<String>.of(first, second, third)
observable.subscribe { (event) in
  print(event)
}.disposed(by: disposeBag)

// 출력: next(First)
// 출력: next(Second)
// 출력: next(Third)
// 출력: completed
```
Observable<String>을 보면 Generic에 [String]이 아닌 String을 넣어주었다.  
즉, 한개의 String을 받도록 하였지만 파라메터로 여러개의 값을 넘겨주는 것이다.  
  
3개의 String 타입(first, second, third)을 선언하여 of에 파라메터로 넘겨주었다.  
그리고 subscribe의 closure함수가 각 파라메터의 갯수만큼 불리게되고 event로 해당 순서에 맞는 String이 넘겨져온다.  
그 결과로 출력이 위와 같이 된것이다.  

어려운것 없이 [ust]의 복수형이라고 생각하면 된다.  

[just]: https://github.com/jaeminKim0523/Library/blob/main/RxSwift/Methods%20List/just.md "Read Just"
