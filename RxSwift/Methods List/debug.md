# Method - Debug

```Swift
let observable: Observable<String> = Observable<String>.create { (observer) -> Disposable in

    observer.onNext("Test")
    observer.onCompleted()

    return Disposables.create()
}

observable.debug("debug").subscribe { (event) in
    print(event)
}.disposed(by: disposeBag)
```
