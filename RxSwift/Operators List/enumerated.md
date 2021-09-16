# Operator - enumerated
enumerated는 Swift의 반복문등에서 현재 인덱스를 알고 싶을때 사용하던 enumerated와 동일한 기능이다.  
현재 Subject에 전달되어온 이벤트에 index 정보를 추가하여준다.  

```Swift
let subject: PublishSubject<String> = PublishSubject<String>()
        
subject.enumerated().subscribe(onNext: { (str) in
    print(str)
}).disposed(by: disposeBag)

subject.onNext("Test1")
subject.onNext("Test2")
subject.onNext("Test3")
subject.onNext("Test4")
subject.onNext("Test5")

// 출력: (index: 0, element: "Test1")
// 출력: (index: 1, element: "Test2")
// 출력: (index: 2, element: "Test3")
// 출력: (index: 3, element: "Test4")
// 출력: (index: 4, element: "Test5")
```
enumerated를 추가하고 Test1부터 5까지의 5가지 이벤트를 발생시켰다.  
원래라면 Test1부터 5까지의 출력만 되었겠지만, enumerated를 사용했기 때문에 출력 정보에 index 0에서 4까지의 인덱스 정보가 추가된것을 확인 할 수 있다.
