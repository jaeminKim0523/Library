# Method - do
RxSwift의 do 메소드는 우리가 알고 있는 repeat-while(do-while)문과 같이 반복하기 위한 조건을 검사하기전에 반복 함수를 한번 실행하고 하는 같은점과 다른점이 존재한다.  
같은점은 subscribe가 되기 전에 어떤 작업들에 대해서 사전에 선언해두는 것이 같은 점이고 다른점은 해당 작업들이 subscribe된 Observable의 작업과 함께 do 또한 실행된다.  

```Swift
enum error: Error {
  case error
}

let observable: Observable<String> = Observable<String>.create{ (observer) -> Disposable in
   
  observer.onNext("First")
  observer.onNext("Second")
  observer.onCompleted() // onError(error.error)
  observer.onNext("Third")
  
  return Disposables.create()
}

observable.subscribe { (event) in
  print(event)
}

// 출력: next(First)
// 출력: next(Second)
// 출력: completed // error(error)
```

[create]: https://github.com/jaeminKim0523/Library/blob/main/RxSwift/Methods%20List/create.md "Read RxSwift-Mothod-Create"
