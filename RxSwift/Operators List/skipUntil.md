# Operator - skipUntil
```Swift
let subject: PublishSubject<Int> = PublishSubject<Int>()
let nextSubject: PublishSubject<Int> = PublishSubject<Int>()
subject.skip(until: nextSubject).subscribe(onNext: { (str) in
  print(str)
}).disposed(by: disposeBag)

subject.onNext(1)
subject.onNext(2)

nextSubject.onNext(0)

subject.onNext(3)
subject.onNext(4)
subject.onNext(5)

// 출력: 3
// 출력: 4
// 출력: 5
```
until은 인자로 넘겨진 Observable의 이벤트가 들어오는 시점까지 자신의 이벤트를 무시한다.  
위의 코드를 보면 알겠지만, nextSubject에 이벤트가 들어오는 시점까지 subject의 이벤트가 무시된다.  
코드는 subject의 1, 2 이벤트 이후에 nextSubeject의 이벤트를 발생시켰기 때문에 subject의 1, 2 이벤트는 무시되고 그 이후의 이벤트만을 출력시켰다.  
