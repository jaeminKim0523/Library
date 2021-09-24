# Operator - skip(count)
```Swift
let subject: PublishSubject<Int> = PublishSubject<Int>()
let skipCount: Int = 2
subject.skip(skipCount).subscribe(onNext: { (str) in
  print(str)
}).disposed(by: disposeBag)

subject.onNext(1)
subject.onNext(2)
subject.onNext(3)
subject.onNext(4)
subject.onNext(5)

// 출력: 3
// 출력: 4
// 출력: 5
```
count는 인자로 넘겨준 Int만큼 이벤트를 넘긴다.
위의 코드와 출력을 보면 인자로 2를 넘겨주었고 1과 2라는 2개의 이벤트가 넘겨져 3에서 5까지만 출력이 된다.
