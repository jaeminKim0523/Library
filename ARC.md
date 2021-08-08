# ARC (Automatic Reference Counting)
### 목차
1. ARC란?
2. 강한 참조(Strong)
3. 약한 참조(Weak)
4. 미소유 참조(Unowned)

## ARC란?
Automatic Reference Counting의 약자로 자동으로 참조를 카운팅해준다.  
이름상의 개념은 이것이고... 많은 언어에서 어떤 클래스을 선언 할 때에는 그 값이 들어갈 메모리를 할당하기 위해 메모리 할당 선언을 한다.  
A라는 클래스를 선언하면 A클래스의 크기만큼의 메모리를 할당 받아서 해당 메모리를 사용하는 것이다.

그렇다면 이 사용한 메모리가 필요가 없어졌을때 할당 되었던 메모리를 다시 회수하는 것도 필요한 것이 당연하다.  
하지만 Swift에서 개발자는 이런 메모리 할당 선언도, 해제 선언도 하지 않는다.  
왜냐하면 오늘의 주제인 ARC가 자동으로 해주기 때문이다.  
우리 눈에 보이는 코드로 선언이 되어있지 않을 뿐이지, 실제로는 컴파일 단계에서 해당 코드가 사용되지 않는 시점에 해제 선언을 해주고 있는 것이다.  

ARC는 할당된 메모리의 갯수를 카운팅한다.  
현재 사용되고 있는 메모리가 몇개인지 어디에 있는 메모리를 사용하고 있는지 알아야 해당 메모리를 해제 할 수 있기 때문이다.  
기본적으로 클래스와 같은 레퍼런스형 인스턴스는 Heap에 저장된다.  

![ARC1](https://user-images.githubusercontent.com/55477102/128631958-918f1afa-9678-4435-8071-65d66f04050b.png)  
위의 스크린샷 처럼 A Class를 선언하면, Heap에 메모리가 할당되고 해제됩니다.  

아주 편한 기능이고 대부분의 상황에 사용자가 메모리 관리를 하지 않아도 된다.
[대부분]의 상황에 한해서다.  
그러면 사용자가 메모리를 [관리]해야하는 상황은 어떤 상황일까?

관리해야 할 상황은 3가지 참조를 예시로 알아보도록 한다.

## Strong Reference
![ARC2](https://user-images.githubusercontent.com/55477102/128632635-b216732e-f683-4ccf-9191-f124bde112e4.png)  
```Swift
class A {
  var b: B?
}

class B {
  var a: A?
}

let myA: A? = A()
let myB: B? = B()

myA.b = b
myB.a = a

myA = nil
myB = nil
```


## Weak Reference
![ARC3](https://user-images.githubusercontent.com/55477102/128632637-abf79fa3-6fcd-4e68-b8a0-83d26b036834.png)  
```Swift
class A {
  var b: B?
}

class B {
  weak var a: A?
}

let myA = A()
let myB = B()

myA.b = b
myB.a = a

myA = nil
myB = nil
```


## Unowned Reference
![ARC4](https://user-images.githubusercontent.com/55477102/128632638-37968fd2-dcbb-4af8-8fc3-0599d335f12c.png)  
```Swift
class A {
  var b: B?
}

class B {
  unowned var a: A?
}

let myA = A()
let myB = B()

myA.b = b
myB.a = a

myA = nil
myB = nil
```


