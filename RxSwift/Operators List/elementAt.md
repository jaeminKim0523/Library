# Operator - element(at:)
elementAt은 Subject의 Event중 at파라메터로 전달한 인덱스의 순서에 해당하는 이벤트만 리턴한다.

```Swift
let subject: PublishSubject<String> = PublishSubject<String>()

subject.element(at: 3).subscribe { (str) in
    print(str)
}.disposed(by: disposeBag)

subject.onNext("Test1")
subject.onNext("Test2")
subject.onNext("Test3")
subject.onNext("Test4")

// 출력: Test4
```
3번째 인덱스 이벤트만을 받도록 선언을 했고 구독시 str이라는 값을 출력하도록 했다.  
그리고 출력 결과는 Test4다.  
인덱스이기 때문에 0, 1, 2, 3의 순서로 3번 인덱스에 해당하는 Test4가 출력된 것이다.  
