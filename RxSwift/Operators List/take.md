# Operator - take
take은 파라메터로 넘겨준 숫자만큼의 이벤트를 전달한다.  

```Swift
let subject: PublishSubject<Int> = PublishSubject<Int>()

subject.take(3).subscribe(onNext: { (str) in
    print(str)
}).disposed(by: disposeBag)

subject.onNext(1)
subject.onNext(2)
subject.onNext(3)
subject.onNext(4)
subject.onNext(5)

// 출력: 1
// 출력: 2
// 출력: 3
```
onNext이벤트로 1에서 5까지 5번의 이벤트를 발생시켰지만 take으로 3개의 값만 받도록 하였다.  
그렇기 때문에 1에서 3까지 3개의 이벤트만 받아서 출력이 되었고 나머지는 무시되었다.
