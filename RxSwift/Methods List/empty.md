# Method - empty
empty는 이름 그대로 비어있는 Observable이다.  
이 말은 onNext와 같은 이벤트가 들어오지 않는다는 의미이며, 결국 Generic의 타입 또한 의미가 없어지게 된다.  

```Swift
let disposeBag = DisposeBag()
let observable: Observable<Int> = Observable<Int>.empty()
observable.subscribe { (event) in
  print(event)
}.disposed(by: disposeBag)

// 출력: completed
```
위와 같이 어차피 Generic에 Int를 선언하든, String을 선언하든 비어있기 때문에 이벤트는 completed만 출력이 되게 된다.  
그러므로 Generic에 들어갈 타입은 Void로 하는 것이 좋다.  

```Swift
let disposeBag = DisposeBag()
let observable: Observable<Void> = Observable<Void>.empty()
observable.subscribe { (event) in
  print(event)
}.disposed(by: disposeBag)

// 출력: completed
```
completed 이벤트만을 필요로 할 때 사용하면 된다.  
