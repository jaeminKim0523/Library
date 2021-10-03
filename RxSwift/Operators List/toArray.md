# Operator - toArray
toArray는 Observable에 들어온 값들을 배열로 변환하여 넘겨준다.

```Swift
Observable<Int>.of(1, 2, 3, 4).subscribe{ value in
  print(value)
}.disposed(by: disposeBag)

// 출력: next(1)
// 출력: next(2)
// 출력: next(3)
// 출력: next(4)
// 출력: completed
```
일단 일반적인 Observable에 Int를 넣어서 출력시켜보았다.  
1부터 4까지의 Int가 들어갔고 순서대로 출력되는 것을 볼 수 있다.  
위의 코드에서 Observable에 toArray를 추가시켜주면 어떻게 출력이 될까?  

```Swift
Observable<Int>.of(1, 2, 3, 4).toArray.subscribe{ value in
  print(value)
}.disposed(by: disposeBag)

// 출력: success([1, 2, 3, 4])
```
위처럼 각 모든 값들이 하나의 배열로 합쳐져서 들어오게된다.  
