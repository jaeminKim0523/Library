# Operator - take(Until:)
takeUntil는 파라메터가 다른 세 종류의 메소드가 있다.  
1. take(until: ObservableType)
2. take(until predicate: (Int) throws -> Bool)
3. take(until: (Int) throws -> Bool, behavior: TakeBehavior)

우리는 위의 세가지를 모두 알아보도록 하자.  
***
### take(until: ObservableType)
첫번째로 ObservableType을 파라메터로 받는 takeUntil이다.  
간단하게 설명하면 ObservableType을 가질때 까지 해당 Subscribe가 유지되는 것이다.  

```Swift
let subject: PublishSubject<Int> = PublishSubject<Int>()
let nextSubject: PublishSubject<Int> = PublishSubject<Int>()

subject.take(until: nextSubject).subscribe(onNext: { (str) in
    print(str)
}).disposed(by: disposeBag)

subject.onNext(1)
subject.onNext(2)
subject.onNext(3)

nextSubject.onNext(0)

subject.onNext(4)
subject.onNext(5)

// 출력: 1
// 출력: 2
// 출력: 3
```
왜 출력이 3까지만 되었을까?  
바로 subject.onNext(3) 이벤트 이후에 nextSubject에 onNext(0)라는 이벤트가 발생했기 때문이다.  
take(until: ObservableType)은 파라메터로 넘겨준 ObservableType의 이벤트가 발생하엿을때 현재 subject가 멈추게된다.  
ps. 단순히 nextSubject를 구독하는것으로는 subject의 구독이 dispose되지는 않는다.  
***
### take(until predicate: (Int) throws -> Bool)
until predicate 파라메터의 타입을 보면 closure라는 것을 알 수 있다.  
그럼 어떻게 동작하는지 코드로 바로 테스트를 해보자.  

```Swift
let subject: PublishSubject<Int> = PublishSubject<Int>()

subject.take(until: { $0 > 3 }).subscribe(onNext: { (str) in
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
출력내용을 보면 closure로 넘어온 element가 3보다 커질때까지의 조건으로 출력이 된것을 볼 수 있다.  
Until의 의미 그대로 3보다 element가 커질때까지 작동하는 것이다.  
take(until: { $0 < 3 })로 바꾸어보면, onNext로 처음 들어온 element가 1이기 때문에 3보다 작은 조건에 만족하여 아무것도 출력하지 않고 끝이난다.  
***
### take(until: (Int) throws -> Bool, behavior: TakeBehavior)
take(until:, behavior:)는 사실 바로 위에서 보았던 take(until predicate:)와 같은 메소드다.  
그럼 다른점은 무엇일까?  
바로 파라메터에 behavior가 추가되었을 뿐이다.  
그리고 take(until predicate:)메소드를 를 보면 behavir:가 있고 기본값으로 .exclusive가 선언되어있다.  
그럼 우리는 behavir 파라메터가 뭔지 알아보도록하자.  

behavior 파라메터에서 요구하는 타입은 TakeBehavior이며, behavior 파라메터에 대한 설명을 보면 last element를 포함시킬지 말지를 정한다고 한다.  
TakeBehavior는 열거형(enum)타입이고 두가지 값을 가지고 있다.  
1. inclusive: 포함
2. exclusive: 미포함

길게 설명할거 없이 코드로 보도록 하자.  

```Swift
let subject: PublishSubject<Int> = PublishSubject<Int>()

subject.take(until: { $0 > 3 }, behavior: .exclusive).subscribe(onNext: { (str) in
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
전에 보았던 take(until predicate:)와 출력이 동일하다.  
바로 위에서 말하고 내려왔지만 결국 take(until predicate:)도 behavior의 값이 기본값으로 exclusive로 설정되어있기 때문이다.  
그러면 behavior의 값을 .inclusive로 선언해주어보자.  

```Swift
take(until: { $0 > 3 }, behavior: .inclusive로)

// 출력: 1
// 출력: 2
// 출력: 3
// 출력: 4
```
behavior의 값을 .inclusive로 선언하면 마지막 값을 포함하도록 설정하는 것이기 때문에 출력시 3보다 커질때까지의 조건에 맞추어진 마지막 element인 4까지 출력되는 것을 확인 할 수 있다.  
