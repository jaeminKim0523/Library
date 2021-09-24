# Operator -  duration, scheduler
```Swift
let subject: PublishSubject<Int> = PublishSubject<Int>()
let scheduler: MainScheduler = MainScheduler.instance
subject.skip(RxTimeInterval.second(3), scheduler: scheduler).subscribe(onNext: { (str) in
  print(str)
}).disposed(by: disposeBag)

subject.onNext(1)
subject.onNext(2)
DispatchQueue.main.asyncAfter(deadline: .now() + 3) {
  subject.onNext(3)
  subject.onNext(4)
  subject.onNext(5)
}

// 출력: 3
// 출력: 4
// 출력: 5
```
duration, scheduler은 duration의 시간동안 이벤트를 넘긴다.  
근데 scheduler는 왜 필요할까?  
그 이유는 duration이후에 작업을 한다는 것은 결국 비동기로 작업 해야하는 상황이 만들어졌다는 것이다.  
그 비동기적인 코드에서는 RxSwift의 MainScheduler에서 하도록 선언했다.

최초로 1과 2를 출력한 뒤에 DispatchQueue를 이용하여 3초 딜레이를 주고 나머지 3, 4, 5를 출력하도록 했다.  
위에서 subject에 3초간 스킵을 하도록 시간을 설정했기 때문에 3초 이전에 실행된 1과 2 이벤트는 출력되지 않고 3초 이후에 실행된 3, 4, 5는 출력된다.
