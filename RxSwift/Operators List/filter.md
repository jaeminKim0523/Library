# Operator - filter
filter는 말 그대로 필터해서 해다 조건에 맞는 이벤트만 전달한다.  

```Swift
let subject: PublishSubject<Int> = PublishSubject<Int>()

subject.filter({ $0 > 3 }).subscribe(onNext: { (str) in
    print(str)
}).disposed(by: disposeBag)

subject.onNext(1)
subject.onNext(2)
subject.onNext(3)
subject.onNext(4)
subject.onNext(5)

// 출력: 4
// 출력: 5
```
조건은 $0이 3보다 크면 해당 이벤트를 전달하도록 했다.  
그릭 출력은 그 조건에 맞게 걸러져서 4와 5가 출력된다.
