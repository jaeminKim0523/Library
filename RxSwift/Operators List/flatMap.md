# Operator - flatMap
flatMap은 넘겨진 모든 Observable을 구독한다.  

```Swift
struct Visitor {
  let name: BehaviorSubject<String>
  let age: PublishSubject<Int> = PublishSubject<Int>()
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
firstGuest.age.onNext(20)
visitor.onNext(secondGuest)
secondGuest.name.onNext("Bob")
firstGuest.name.onNext("John")

// 출력: next(First)
// 출력: next(Tom)
// 출력: next(Second)
// 출력: next(Bob)
// 출력: next(John)
```
조금 어려울 수 있다...
순서대로 위에서부터 차근차근 내려가보자.  

테스트를 위해 Visitor라는 구조체를 하나 만들었고 First와 Second 이름으로 2가지 상수(firstGuest, secondGuest)를 선언했고 flatMap을 통해 Visitor구조체의 name만을 return해주었다.  
그리고 순서대로 firstGuest, Tom, 20, secondGuest, Bob, John의 이벤트를 발생시켰다.  

우리는 flatMap을 통해 Visitor의 name만을 구독하고자 했기 때문에 firstGuest이벤트가 발생 되었을때 firstGuest의 초기 name값인 First가 출력되고 다음 Tom이 출력됐다.  
다음 이벤트인 age는 구독하지 않았기 때문에 출력이 되지 않는다.  
아래의 secondGeust 또한 같은 맥락이다.  

***
### flatMap과 map의 차이점
둘의 차이점은 너무나 명확하다.  
```$0.name```코드를 예로 들어보자.  
map은 visitor를 구독하고 flatMap은 name을 구독한다.  
그렇기 때문에 flatMap에서 firstGuest를 넘겼을때 firstGuest의 초기 이름인 First부터 Tom, Second, Bob, John이 출력되는 것이다.  
map에서는 어떻게 될까?

```Swift
visitor.map {
  $0.name
}. subscribe {
  print($0)
}

// 출력: next(RxSwift.BehaviorSubject<Swift.String>)
// 출력: next(RxSwift.BehaviorSubject<Swift.String>)
```
map은 visitor를 구독했고 visitor의 이벤트인 firstGuest와 secondGeust의 name을 출력했다.  
말이 조금 어렵지만, 단순하게 구독한 visitor의 name을 구독하지는 않은 것이고 flatMap은 name까지도 구독한 한것이다.  
name까지 구독이 되었기 때문에 name에 이벤트를 발생시키면 해당 이벤트가 출력된 것이다.  

만약 map을 flatMap처럼 사용하려면 어떻게 해야했을까?
```Swift
visitor.map {
  $0.name
}. subscribe {
  $0.element?.subscribe {
    print($0)
  }
}

// 출력: next(First)
// 출력: next(Tom)
// 출력: next(Second)
// 출력: next(Bob)
// 출력: next(John)
```
visitor로 넘겨온 Visitor 구조체의 name을 구독하면 같은 역할을 기대 할 수 있다.
