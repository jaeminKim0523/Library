# Method - create
create는 observable의 내용을 직접 코딩하는 것이다.  
바로 코드를 보고 시작하자.  

```Swift
let observable: Observable<String> = Observable<String>.create{ (observer) -> Disposable in
   
  observer.onNext("First")
  observer.onNext("Second")
  observer.onNext("Third")
  observer.onCompleted()
  
  return Disposables.create()
}
```
위와 같이 observable의 내용을 코딩해보았다.  
쉬운 이해를 위해서 [Method - of]와 같은 결과를 출력하도록 구성해보았다.  
of에서는 ("First", "Second", "Third")를 of의 파라메터로 넣어주었고 of 메소드가 이벤트와 completed를 해준것이다.  

이 작업을 create에서는 직접 코딩 해줄 수 있다.  
이벤트는 onNext, onError, onCompleted가 있다.

위의 코드를 구독해보자.  
```Swift
observable.subscribe { (event) in
  print(event)
}

// 출력: next(First)
// 출력: next(Second)
// 출력: next(Third)
// 출력: completed
```

위와 같이 출력된다.  
만일 onError, onCompleted가 Second와 Third사이에 들어가면 Third는 출력이 되지 않게된다.  

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

[Method - of]: https://github.com/jaeminKim0523/Library/blob/main/RxSwift/Methods%20List/of.md "Read of"
