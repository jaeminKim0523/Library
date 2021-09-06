# Method - range
```Swift
static func range(start: Int, count: Int, scheduler: ImmediateSchedulerType = CurrentThreadScheduler.instance) -> Observable<Int>
```
range의 Declaration이다.  
파라메터를 설명하면, start부터 시작하여 count까지 Loop를 하게 된다.  
```Swift
for num in start ..< count {
  print(num)
}
```
for문의 이것과 같다고 생각하면 쉽다.  

어려운 메소드가 아니니 바로 사용해보자.  

```Swift
let disposeBag = DisposeBag()
let observable: Observable<Int> = Observable<Int>.of(start: 0, count: 5)
observable.subscribe { (event) in
  print(event)
}.disposed(by: disposeBag)

// 출력: 0
// 출력: 1
// 출력: 2
// 출력: 3
// 출력: 4
// 출력: completed
```
위와 같이 출력이 된다.  
