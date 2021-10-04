# Operator - flatMapLatest
flatMapLatest는 [flatMap](https://github.com/jaeminKim0523/Library/blob/main/RxSwift/Operators%20List/flatMap.md, "flatMap Link")과 동일하다.  
Observable을 구독하는데 이름에 있듯이 마지막 Observable만을 구독한다.  

```Swift
struct Visitor {
  let name: BehaviorSubject<String>
}

let visitor = PublishSubject<Visitor>()
let firstGuest = Visitor(name: BehaviorSubject<String>(value: "First"))
let secondGuest = Visitor(name: BehaviorSubject<String>(value: "Second"))

visitor.flatMap {
  $0.name
}.subscribe {
  print($0)
}.disposed(by: disposeBag)

visitor.onNext(firstGuest)
firstGuest.name.onNext("Tom")
visitor.onNext(secondGuest)
secondGuest.name.onNext("Bob")
firstGuest.name.onNext("John")

// 출력: next(First)
// 출력: next(Tom)
// 출력: next(Second)
// 출력: next(Bob)
```
출력을 보면, 마지막으로 구독된 Observable이 secondGuest이기 때문에 마지막 firstGuest의 이벤트는 출력되지 않게된다.  
