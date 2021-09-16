# Operator - distinctUntilChanged
distinctUntilChanged는 이전에 받았던 이벤트의 element와 현재 element가 같으면, 현재 element를 무시한다.  

```Swift
let subject: PublishSubject<Int> = PublishSubject<Int>()

subject.distinctUntilChanged().subscribe(onNext: { (str) in
    print(str)
}).disposed(by: disposeBag)

subject.onNext(1)
subject.onNext(1)
subject.onNext(3)
subject.onNext(2)
subject.onNext(2)
subject.onNext(3)

// 출력: 1
// 출력: 3
// 출력: 2
// 출력: 3
```
onNext로 1 -> 1 -> 3 -> 2 -> 2 -> 3 순서로 이벤트를 발생시켰다.  
그리고 출력은 1 -> 3 -> 2 -> 3이 출력되었다.  
1 -> (1) -> 3 -> 2 -> (2) -> 3 괄호 안의 element가 이전에 들어온 element와 같기 때문에 무시된것이다.  
뒤에 3이 나온 이유는 3이라는 값이 중복으로 사용되긴 했지만, 직전에 전달된 element는 2이기 때문에 출력이 가능하다.
