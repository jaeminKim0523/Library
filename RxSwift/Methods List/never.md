# Method - never
[empty]는 비어있기 때문에 completed 이벤트만을 사용했다면, never는 completed 이벤트 조차도 사용하지 않는다.  

``` Swift
let disposeBag = DisposeBag()
let observable: Observable<Void> = Observable<Void>.never()
observable.subscribe { (event) in
  print(event)
}.disposed(by: disposeBag)
```
아무 출력도 하지 않는다.  
