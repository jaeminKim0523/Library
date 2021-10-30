# Operator - concatMap
concatMap은 closure 함수에서 return해주는 값을 모아서 하나의 Observable로 반환하여 준다.  

```Swift
let buildingObservable = Observable<String>.of("A동", "B동", "C동")
let aObservable = Observable<String>.of("존", "에단", "테리")
let bObservable = Observable<String>.of("카일", "도니")
let cObservable = Observable<String>.of("제이슨", "올리버", "프란치스코", "그레이")

let town = ["A동" : aObservable, "B동" : bObservable, "C동" : cObservable]

buildingObservable.concatMap{ value in
    town[value] ?? .empty()
}.subscribe { event in
    print(event)
}.disposed(by: disposedBag)

// 출력
// 존
// 에단
// 테리
// 카일
// 도니
// 제이슨
// 올리버
// 프란치스코
// 그레이
// completed
```
A, B, C 동이 있다고 가정하고 각 동에 사는 사람의 이름을 Observable에 선언하였다.  
각 동의 이름을 사용하여 town dictionary를 선언하였다.  
그리고 concatMap을 통해 town dictionary의 Observable을 가져와서 반환해주었고 해당 Observable이 순서대로 합쳐져 출력되었다.  
