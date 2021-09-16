# Operator - take(While:)
takeWhile은 반복문 while과 같은 개념이다.  
while문은 조건에 맞는 동안 즉, 조건이 true인 동안 반복된다.  
takeWhile도 while문과 동일하게 파라메터의 closure에서 return되는 Bool값이 true일때까지 반복한다.  

```Swift
let subject: PublishSubject<Int> = PublishSubject<Int>()

subject.take(While: { $0 < 3 }).subscribe(onNext: { (str) in
    print(str)
}, onDisposed: {
    print("Disposed")
}).disposed(by: disposeBag)

subject.onNext(1)
subject.onNext(2)
subject.onNext(3)
subject.onNext(4)
subject.onNext(5)

// 출력: 1
// 출력: 2
// 출력: Disposed
```
take의 While 파라메터에 element가 3보다 작을때까지 반복하도록 하였고 출력은 1, 2가 출력되었다.  
만약 take의 While 파라메터에 3보다 클때까지 반복하도록 조건을 설정하면 어떻게 될까?

```Swift
subject.take(While: { $0 > 3 })
```
당연히 단하나의 출력도 되지 않는다.  
While문에서 조건이 맞지 않으면 반복문에서 탈출하는 것 처럼 take(While:)도 하나의 반복문이기 때문에 조건에 틀린 순간 해당 구독이 끝나고 onDispose된다.  
