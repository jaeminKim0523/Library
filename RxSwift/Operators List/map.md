# Operator - map
RxSwift의 map은 Swift에 기본으로 있는 람다 함수인 map과 동일한 기능을 한다.  

```Swif
let map: [Int] = [1, 2, 3, 4].map {
  $0 + 3
}
for i in map {
  print(i)
}

//출력: 4
//출력: 5
//출력: 6
//출력: 7
```
RxSwift를 써보기 전에 테스트해보고자 하는 것을 기본 Swift로 변환해보았다.  
map은 기본적으로 배열의 값 하나씩을 돌며 map 함수 내에서 return된 값으로 다시 배열을 선언하는 개념으로 보면 된다.  
테스트용으로 배열의 Int값들에 +3이 된 값을 다시 return하도록 map을 구성하였고 출력을 보면 +3이 된 값들이 출력되는 것을 볼 수 있다.  
그럼 위의 코드를 RxSwift로 구성하면 코드는 어떻게 될까?  

```Swift
Observable<Int>.of(1, 2, 3, 4).map {
  $0 + 3
}.subscribe {
  print($0)
}.disposed(by: disposeBag)

//출력: next(4)
//출력: next(5)
//출력: next(6)
//출력: next(7)
//출력: completed
```
위와 같이 구성 할 수 있다.  
내용을 같다.
