# Method - debug
debug 메소드는 Observable의 이벤트가 발생하였을때 콘솔창에 String을 출력시켜 준다.

```Swift
let observable: Observable<String> = Observable<String>.create { (observer) -> Disposable in

    observer.onNext("Test")
    observer.onCompleted()

    return Disposables.create()
}

observable.debug("debug").subscribe { (event) in
    print(event)
}.disposed(by: disposeBag)

// 출력1: yyyy-MM-dd HH:mm:ss.z: debug -> subscribed
// 출력2: yyyy-MM-dd HH:mm:ss.z: debug -> Event next(Test)
// 출력3: next(Test)
// 출력4: yyyy-MM-dd HH:mm:ss.z: debug -> Event completed
// 출력5: completed
// 출력6: yyyy-MM-dd HH:mm:ss.z: debug -> isDisposed
```
출력n 로그를 순서대로 보면,
1. 디버그 출력: 구독이 되었을때
2. 디버그 출력: onNext 이벤트가 발생하기 전
3. 이벤트 출력: onNext("Test")
4. 디버그 출력: onCompleted 이벤트가 발생하기 전
5. 이벤트 출력: onCompleted()
6. 디버그 출력: dispose가 되기 전

어느 observable에서 어떤 이벤트로 어떤 값이 전달되었는지 디버깅 할 때 사용하면 된다.
