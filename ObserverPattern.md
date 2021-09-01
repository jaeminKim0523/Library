# Observer Pattern
### 목차
1. 기본 개념
2. Observer
3. Subject
4. 마무리

## 기본 개념
Observe의 뜻은 '관찰하다' 이다.  
Observer는 '관찰자' 이다.  

사실 개념은 이것으로 50% 이상의 설명이 된다.  

Observer pattern은 값을 관찰하고 그 값의 상태가 변하는 알림을 받으면, 해당 알림에 관찰자가 대응을 하는 개념이다.  
구성으로는 Observer, Subject, 구독자가 있다.  

## Observer
```Swift
protocol Observer {
  let id: Int { get set }
}
extension Observer {
  func notify() {
    print("notify: \(self.id)")
  }
}
```
코드는 정말 단순하다.  
Observer는 자신의 id를 갖고 자신의 id를 알려주는 함수를 갖는다.  
우리는 여러개, 여러종류의 Observer로 테스트를 해보기 위해 ObserverA와 B클래스를 선언해준다.  

Observer의 역할은 정말 코드와 같이 별게 없다.  
그냥 자신의 고유값을 갖고 어떤 정보를 알려주는 역할을 할 뿐이다.  
지금은 자신의 id를 출력했지만, String이나 Int와 같은 값을 받아서 알려주는 역할으로도 사용 할 수 있다.  

```Swift
class ObserverA: Observer {
  var id: Int
  
  init(id: Int) {
    self.id = id
  }
}

class ObserverB: Observer {
  var id: Int
  
  init(id: Int) {
    self.id = id
  }
}
```
위의 두가지 Observer class로 Subject를 이용하여 테스트를 해볼 예정이다.  

## Subject
```Swift
protocol Subject {
  var observers: [Observer] { get set }
}
extension Subject {
  mutating func subscribe(observer: Observer) {
    self.observers.append(observer)
  }
  
  mutating func unSubscribe(observer: Observer) {
    if let idx = self.observers.firstIndex(where: { $0.id == observer.id }) {
      self.observers.remove(at: idx)
    }
  }
  
  func notify() {
    self.observers.forEach{ $0.notify() }
  }
  
}
```
Subject는 위에서 선언한 Observer들을 한곳에서 관리하는 역할이다.  
Subject의 의미와 같이 주제가 같은 Observer들을 모아두는 것이다.  
Protocol을 보면 Observer의 배열이 요구되며, subscribe, unSubscribe, notify 3개의 함수를 선언해두었다.  
subscribe는 Observer를 Subject에 추가하는 함수,  
unSubscribe는 Observer를 Subject에서 삭제하는 함수,  
notify는 모든 Observer의 notify 함수를 실행시키는 역할을 한다.  

```Swift
class TempSubject: Subject {
  var observers: [Observer]
  
  init(observers: [Observer] = []) {
    self.observers = observers
  }
}
```
위의 클래스로 우리는 Observer 패턴을 사용해보자.  

## 마무리
```Swift
var observerA: ObserverA = ObserverA(id: 0)
var observerB: ObserverB = ObserverB(id: 1)

var subject: TempSubject = TempSubject()
subject.subscribe(observer: observerA)
subject.subscribe(observer: observerB)

subject.notify()
// notify: 0
// notify: 1

subject.unSubscribe(observer: observerA)
observerB.id = 3

subject.notify()
// notify: 3
```
ObserverA와 B 2가지 class와 subject 1개를 선언했다.  
그리고 그 subject에 2개의 class를 구독시켰고 subject의 notify함수를 실행시켰다.  
당연히 ObserverA의 id인 0 / B의 id인 1이 출력된다.  

그리고 id가 0인 A를 구독 해제했으니 그 다음 notify에서는 id가 3으로 변경된 observerB 3만 출력이 된다.  
이렇게 Observer와 Subject를 이용하여 Observer attern을 사용해보았다. 
