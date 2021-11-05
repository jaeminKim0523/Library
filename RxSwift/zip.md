# Operator - zip
zip은 여러개의 값을 파라메터로 넘겨주고 해당 값의 배열 수만큼 반복시키며 고차원 함수를 이용하여 반복 호출을 한다.  
고차원 함수가 호출되는 횟수는 넘겨진 배열중 가장 갯수가 적은 배열을 기준으로 한다.  
예를들어 A = [1, 2, 3], B = [a, b, c, d], C = [x, y] 3가지 배열을 넘겨준다고 예를 들면, C의 배열의 count가 2이므로 고차원 함수는 2번 반복된다.  

```Swift
let aObservable = Observable<String>.of("에단", "도니", "프란치스코")
let bObservable = Observable<Int>.of(1, 2, 3)

let observable = Observable.zip(aObservable, bObservable) { a, b in
    return "idx: \(b), name: \(a)"
}

observable.subscribe { event in
    print(event)
}.disposed(by: disposedBag)

// 출력
// next(idx: 1, name: 에단)
// next(idx: 2, name: 도니)
// next(idx: 3, name: 프란치스코)
// completed
```
aObservable배열과 bObservable배열 두개를 zip 파라메터로 넘겨주고 closure로 해당 값들을 받아온다.  
그리고 a와 b를 하나의 String 형식으로 바꾸어 return해주면 String타입의 Observable이 반환된다.  
해당 Observable을 구독해보면 합쳐진 값이 들어있는 것을 확인 할 수 있다.  
