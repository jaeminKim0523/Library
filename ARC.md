# ARC (Automatic Reference Counting)
### 목차
1. ARC란?
2. 강한 참조(Strong)
3. 약한 참조(Weak)
4. 미소유 참조(Unowned)

## ARC란?
Automatic Reference Counting의 약자로 자동으로 참조를 카운팅해준다.  
이름상의 개념은 이것이고... 낮은 세대의 언어에서는 어떤 클래스을 선언 할 때 그 값이 들어갈 메모리를 할당하기 위해 메모리 할당 선언을 한다.  
A라는 클래스를 선언하면 A클래스의 크기만큼의 메모리를 할당 받아서 사용하는 것이다.

그렇다면 이 사용한 메모리가 필요가 없어졌을때 할당 되었던 메모리를 다시 회수하는 것도 당연히 필요하다.  
하지만 Swift에서 개발자는 이런 메모리 할당 선언도, 해제 선언도 하지 않는다.  
왜냐하면 오늘의 주제인 ARC가 자동으로 해주기 때문이다.  
우리 눈에 보이는 코드로 선언이 되어있지 않을 뿐이지, 실제로는 컴파일 단계에서 해당 코드가 사용되지 않는 시점에 해제 선언을 자동으로 해주고 있는 것이다.  

ARC는 이름과 같이 할당된 메모리를 사용하는 참조의 갯수를 카운팅한다.  
그래야만 현재 사용되고 있는 메모리를 참조하는 곳이 몇개인지, 메모리가 사용되고 있는 중인지 정보를 알아야 해당 메모리를 해제 할 수 있기 때문이다.  
```Swift
class A {
  let name: String = "Kim jaemin"
}

var a1 = A() // count = 1
var a2 = a1 // count = 2
var a3 = a1 // count = 3

a1 = nil // count = 2
a2 = nil // count = 1
a3 = nil // count = 0
```
위와 같이 인스턴스가 참조 될 때 마다 Count가 증가하고 해제 될 때 감소한다.  

아주 편한 기능이고 대부분의 상황에 사용자가 메모리 관리를 하지 않아도 된다.
[대부분]의 상황에 한해서다.  
그러면 사용자가 메모리를 [관리]해야하는 상황은 어떤 상황일까?

관리해야 할 상황은 아래에서 3가지 참조 코드를 예시로 알아보도록 한다.

#### 추가 정보
기본적으로 클래스와 같은 레퍼런스형 인스턴스는 Heap에 저장된다.  
![ARC1](https://user-images.githubusercontent.com/55477102/128631958-918f1afa-9678-4435-8071-65d66f04050b.png)  
위의 스크린샷 처럼 A Class를 선언하면, Heap에 메모리가 할당되고 해제된다.  

## Strong Reference
```Swift
class A {
  var b: B?
}

class B {
  var a: A?
}

let myA: A? = A() // 1
let myB: B? = B() // 2

myA.b = b // 3
myB.a = a // 4

myA = nil // 5
myB = nil // 6
```
1번에서는 A라는 myA클래스를, 2번에서는 B라는 myB클래스를 선언했다.  
이 클래스들은 서로의 클래스를 가질 수 있도록 선언되어 있다.  
3번 4번에서 서로 가지고 있는 값에 서로를 넣어주었다.  

![ARC2](https://user-images.githubusercontent.com/55477102/128632635-b216732e-f683-4ccf-9191-f124bde112e4.png)  
4번까지의 코드의 상황을 그림으로 보면 이런 상황이 된것이다.  

이 상황에서 5번과 6번 코드가 실행된다면 어떤 상황이 일어날까?  
myA와 myB가 해제되어도 myB는 myA.b가 강한 참조를 하고 있기 때문에 ARC에서 카운트가 감소되지 않아 메모리를 소유하고 있게 된다.
반대로 myA는 myB.a가 강한 참조를 하고 있기 때문에 카운트가 감소되지 않아 메모리를 잡고 있게 된다.
![ARC2-1](https://user-images.githubusercontent.com/55477102/128858172-71b86852-8fd2-44d7-9d32-a43a1c619a48.png)  
위의 스크린샷과 같은 상황이 일어난 것이고 이 상황을 강력 상호 참조 또는 강력 순환 참조(이하 순환 참조)라고 한다.  
순환 참조를 해소 할 수 있는 방법은 아래 weak와 unowned를 보면서 알아본다.  

## Weak Reference
```Swift
class A {
  var b: B?
}

class B {
  weak var a: A? // 1
}

let myA = A()
let myB = B()

myA.b = b
myB.a = a

myA = nil
myB = nil
```
1번 코드 줄을 보면, weak로 선언되어 있다.  

![ARC3](https://user-images.githubusercontent.com/55477102/128632637-abf79fa3-6fcd-4e68-b8a0-83d26b036834.png)  
weak는 약한 참조를 나타내며, let(상수)이 아닌 var(변수)로 선언되어야 한다.  
추가로 weak는 값이 nil일 수 있다는 상황을 전제로 하기 때문에 optional로 선언되어야 한다.  

위의 강한 참조에서 알아보았던 강한 참조 일 때 참조하는 대상이 nil이 되어도 계속 참조하는 것을 보았다.  
그렇다면, 약한 참조는 그와 반대라고 생각하면 된다.  

![ARC3-1](https://user-images.githubusercontent.com/55477102/128862550-41399d8e-28cf-4f8e-847b-adb73990d4d8.png)  
강한 참조 처럼 상대가 해제되어도 자신은 남아있는 것이 아니라 대상이 해제되면 자기 자신도 해제가 되어버린다.  

weak라는 약한 참조를 사용하는 것이 순환 참조를 해결하는 가장 많이 사용되는 방법 중 하나이다.

## Unowned Reference
unowned는 weak와 다르게 let(상수)을 사용 할 수 있으며, Optional일 필요가 없다.  
다만 unowned는 자신의 대상이 항상 존재한다고 생각하고 있다.  
맞는 예시일지 모르겠지만, Optional의 !를 생각하면 조금은 이해에 도움이 될 것 같다.  
상대가 존재 할지 모르지만, 존재한다는 가정을 강제하는 것이다.  
물론 상대가 존재하지 않으면... 런타임 오류가 나고 앱이 죽게된다.

위의 이유 때문에 unowned 참조는 2가지 예시를 들어야 할 것 같다.  
일반적인 상황과 런타임 오류가 발생하는 상황 두가지이다.  

### 일반 상황
```Swift
class A {
  var b: B?
}

class B {
  unowned let a: A
  
  init(a: A) { //
    self.a = a // 1
  }            // 
}

let myA = A() // 2

myA.b = B(a: myA) // 3

myA = nil // 4
```
위에서 보았던 strong과 weak의 클래스와 구성이 달라졌다.  
런타임 오류가 나지 않는 상황을 나타내기 위함이다.  

1번에서 B class를 선언 할 때 초기값으로 A클래스를 받아오도록 하였다.  
그리고 2번에서 A class인 myA를 선언하고 myA의 b에 B Class를 넣어주는데, 그 B Class는 myA를 갖고 있도록 하였다.  

위의 코드를 그림으로 표현하면 아래와 같다.  
![ARC4-1](https://user-images.githubusercontent.com/55477102/128867059-0062140a-9cdd-4e5e-ab58-46f197d69329.png)  
myA는 B를 가지고 있고 그 B는 A를 unowned로 가지고 있다.  

4번에서 myA를 해제하게 되면, myA는 해제되게 된다.  
그리고 myA의 b가 myA를 참조하고 있지만, strong이 아닌 unowned기 때문에(강한 참조가 아니기 때문에) count가 0이되어 해제되고 그러므로 myA의 b 또한 해제되게 된다.  

이렇게 unowned를 이용하여 순환 참조를 해결 할 수 있다.
그렇다면, 런타임 오류가 나는 상황을 한번 만들어 보도록 한다.  
***  
### 런타임 오류
```Swift
class A {
  var b: B?
}

class B {
  unowned let a: A
  
  init(a: A) { //
    self.a = a // 1
  }            // 
}

let myA = A() // 2
let myB = B(a: myA)

myA.b = myB // 3

myA = nil // 4
```
위의 코드를 그림으로 표현해보면 아래와 같다.  
![ARC4](https://user-images.githubusercontent.com/55477102/128867953-b4080623-45ed-4129-8fac-8a1bb20c9d2e.png)

이전에 보았던 일반 상황과 다른점은 myB가 B Class를 강한 참조하고 있다는 것이다.  
그렇기 때문에 myA를 해제한다고 해서 myB의 A class가 해제되는 것이 아니게 되었고, 이 점은 myB를 통해 해제된 myA인 myB.a의 사용을 시도 할 수 있다는 말이 된다.  
이 상황은 위에서 Optional의 !로 예시를 들엇던 것 처럼 해제된 메모리에 접근을 시도하는 것이기 때문에 런타임 오류가 발생하게 된다.  
***  
