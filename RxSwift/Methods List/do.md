# Method - do
RxSwift의 do 메소드는 우리가 알고 있는 repeat-while(do-while)문과 같이 반복하기 위한 조건을 검사하기전에 반복 함수를 한번 실행하고 하는 같은점과 다른점이 존재한다.  
같은점은 subscribe가 되기 전에 어떤 작업들에 대해서 사전에 선언해두는 것이 같은 점이고 다른점은 해당 작업들이 subscribe된 Observable의 작업과 함께 do 또한 실행된다.  

```Swift
enum error: Error {
  case error
}

let observable: Observable<String> = Observable<String>.create{ (observer) -> Disposable in
   
  observer.onNext("First")
  observer.onNext("Second")
  observer.onCompleted() // onError(error.error)
  observer.onNext("Third")
  
  return Disposables.create()
}

observable.do(onNext: { (next) in
    print("onNext: \(next)")
},
afterNext: { (next) in
    print("afterNext: \(next)")
},
onError: { (error) in
    print("onError: \(error)")
},
afterError: { (error) in
    print("afterError: \(error)")
},
onCompleted: {
    print("onCompleted")
},
afterCompleted: {
    print("afterCompleted")
},
onSubscribe: {
    print("onSubscribe")
},
onSubscribed: {
    print("onSubscribed")
},
onDispose: {
    print("onDispose")
}).subscribe { (event) in
    print(event)
}.disposed(by: disposeBag)

// 출력: onSubscribe
// 출력: onNext: First
// 출력: next(First)
// 출력: afterNext: First
// 출력: onNext: Second
// 출력: next(Second)
// 출력: afterNext: Second
// 출력: onError: error
// 출력: error(error)
// 출력: afterError: error
// 출력: onSubscribed
// 출력: onDispose
```
[Mothod-Create]에서 보았던 코드를 수정해서 예제를 만들어 보았다.  
do의 파라메터 구성을 보면, 일단 모든 파라메터들이 모두 closure 함수다.  
1. onNext: observable의 onNext가 실행되기 전
2. afterNext: observable의 onNext가 실행된 이후
3. onError: observable의 onError가 실행되기 전
4. afterError: observable의 onError가 실행된 이후
5. onCompleted: observable의 onCompleted가 실행되기 전
6. afterCompleted: observable의 onCompleted가 실행된 이후
7. onSubscribe: subscribe가 시작된 이후
8. onSubscribed: subscribe가 완료된 이후
9. onDispose: 구독이 disposed되기 전

위의 파라메터들이 작동하는 타이밍이다.  
on == will / after == did의 느낌이라고 생각하면 이해하기 편할 것 같다.  

순서를 간단하게 정리하면,
1. do - on
2. observable - on
3. do - after
가 된다.  

[Mothod-Create]: https://github.com/jaeminKim0523/Library/blob/main/RxSwift/Methods%20List/create.md "Read RxSwift-Mothod-Create"
