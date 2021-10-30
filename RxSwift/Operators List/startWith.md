# Operator - startWith
startWith는 Observable의 이벤트가 시작하기전에 원하는 값을 가장 앞에 추가하여 실행되게 하고 싶을때 사용한다.

```Swift
let disposedBag = DisposeBag()

let observable = Observable<Int>.of(2, 3, 4, 5)
        
observable.startWith(1).subscribe { event in
    print(event)
}.disposed(by: disposedBag)

// 출력
// next(1)
// next(2)
// next(3)
// next(4)
// next(5)
// completed
```

코드를 보면, ```Observable<Int>.of(2, 3, 4, 5)```을 통해 Observable에 값이 2, 3, 4, 5가 선언된 것을 알 수 있다.  
그리고 Observable이 실행되기 전에 startWith를 이용하여 1을 넣어주었고 그 결과 "1, 2, 3, 4, 5"가 출력된 것이다.   
