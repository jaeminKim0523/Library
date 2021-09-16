# Operator - ignoreElements
ignoreElements는 이벤트를 무시한다.  
하지만, completed와 error와 같은 종료와 관련된 이벤트는 수신한다.  

```Swift
let subject: PublishSubject<String> = PublishSubject<String>()
        
subject.ignoreElements().subscribe(onNext: { (str) in
    print(str)
}, onCompleted: {
    print("onCompleted")
}, onDisposed: {
    print("onDisposed")
}).disposed(by: disposeBag)

subject.onNext("Test1")
subject.onCompleted()

// 출력: onCompleted
// 출력: onDisposed
```
onNext로 전달한 Test1이라는 이벤트는 무시되었고 완료형 이벤트인 onCompleted와 onDisposed이벤트만 전달된 것을 출력을 통해 확인 할 수 있다.  
onError이벤트도 onCompleted와 동일한 완료형 이벤트이기 때문에 onCompleted와 동일하게 작동한다.
