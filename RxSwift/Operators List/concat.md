# Operator - concat
concat은 파라메터로 넘겨준 Observable의 값들을 현재 Observable의 뒤에 붙여넣어 준다.  

```
let disposedBag = DisposeBag()
    
let frontObservable = Observable<Int>.of(1, 2, 3)
let backObservable = Observable<Int>.of(4, 5, 6)

frontObservable.concat(backObservable).subscribe { event in
    print(event)
}.disposed(by: disposedBag)

// 출력
// 1
// 2
// 3
// 4
// 5
// 6
// completed
```
frontObservable을 실행하는데 concat의 파라메터로 backObservable을 넘겨주었다.  
위에서 설명했듯이 frontObservable에 concat으로 넘겨진 backObservable이 뒤에 붙여졌고 그 결과 출력이 1, 2, 3, 4, 5, 6이 되었다.
