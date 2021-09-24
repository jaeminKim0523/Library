# Operator - skipWhile
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
