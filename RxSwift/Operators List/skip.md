# Operator - skip
skip은 이름 그대로 이벤트를 넘긴다.  
이벤트를 넘기는 조건을 나눌 수 있는 4가지 메소드가 있다.  

1. count
2. until
3. while
4. duration, scheduler

하나씩 알아보자.
***  
### count
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
***
### until
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
***
### while
```Swift
let subject: PublishSubject<Int> = PublishSubject<Int>()
subject.skip(while: { $0 < 3 }).subscribe(onNext: { (str) in
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
while은 closure함수 안에서 return하는 논리값이 true인동안 skip을 한다.  
위의 코드중 { $0 < 3 } 이 부분을 보면 된다.  
$0은 각 이벤트로 들어오는 값이며 1에서 5까지 숫자가 들어오게 된다.  
그리고 그 숫자가 3보다 작을때까지 스킵이기 때문에 1 < 3은 true이기 때문에 skip, 2또한 3보다 작기 때문에 스킵된다.  
만약 앞에 3보다 큰 숫자가 나온 후 뒤에 3보다 작은 숫자가 나오면 어떻게 될까?  

```Swift
subject.onNext(1)
subject.onNext(3)
subject.onNext(2)
subject.onNext(4)
subject.onNext(5)

// 출력: 3
// 출력: 2
// 출력: 4
// 출력: 5
```
위 처럼 1은 출력되지 않았고 다음 이벤트인 3이 들어오면서 while에서 빠져나왔기 때문에 그 뒤에 들어오는 2는 3보다 작더라도 출력이 된다.  
우리가 알고 있는 while반복문과 같다.  
한번 반복문에서 나왔기 때문에 그 뒤의 조건은 의미가 없는 것이다.  
***
### duration, scheduler
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
