# Value & Reference Types
### 목차
1. 서론
2. Value Type
3. Stack
4. Reference Type
5. Heap
6. 차이점

## 서론
해당 내용을 시작하기 전에 왜 해당 내용을 알아야 하는지에 대한 목적에 대하여 이해를 하고 시작하는 것이 좋을 것 같다.  
기본적으로 핸드폰도 컴퓨터이고 프로그램을 사용자에게 보여준다.  
우리는 사용자가 프로그램을 사용 할 때 부드럽고 반응이 빠른 프로그램을 만들 의무가 있는 것이다.  
그런 프로그램을 만들려면 어떻게 해야할까?  
말은 쉽지만... 최대한 가볍고 컴퓨터 친화적인 프로그램을 만들면 당연히 컴퓨터의 부담이 적어지고 프로그램이 빨라지게 된다.  
이번에 알아볼 Value와 Reference Type이 가장 기본적인 방법중 하나이다.  

ARC의 ARC란? 부분을 한번 보고오는 것이 이해에 도움이 될 것 같다.  
관련 정보 링크: [ARC]  

[ARC]: https://github.com/jaeminKim0523/Library/blob/main/ARC.md "Read ARC"
 
## Value Type
Value type의 종류
- Struct
- Enum
- Tuple
- Int, String, Bool, Array, Dictionary, Set, Double 등등 기본 선언 타입

위의 Type으로 값을 선언하였을때 Value Type으로 선언이 된다.  
Value는 말 그대로 값을 의미하는 타입이며, Stack에 저장된다.  
또한 Reference 타입이 아니기 때문에 Reference Counting도 필요 없고 메모리를 할당 할 때 빈 곳을 찾아서 관리하는 과정이 없으며, Thread 관련 과정 또한 필요 없다.
그렇기 때문에 일반적으로 사용하는 Value 타입은 Reference 타입보다 더 가볍다.  

## Stack
Value Type이 저장되는 곳이다.  
Stack은 이름과 같이 데이터가 아래부터 쌓이는 느낌이라고 생각하면 된다.  

![ValueRef1](https://user-images.githubusercontent.com/55477102/129483900-112c62e8-6e32-4cfd-a327-c709cec74d18.png)  
Stack은 위의 스크린샷과 같이 Last In First Out(LIFO) 방식으로 데이터가 쌓인다.  

## Reference Type
Reference type의 종류
- Class
- Function
- Closure 등등

위의 Type으로 값을 선언하였을때 Reference Type으로 선언이 된다.  
Reference는 참조라는 의미이며, Heap에 저장이 된다.  
Heap에 저장된다는 것은 ARC가 관리를 해야한다는 의미이며 이는 메모리 할당시에 Heap의 빈 공간을 찾아다녀야 하며 데이터 관리에 있어서도 메모리 할당과 해제에 대한 복잡한 과정이 필요하다.  
또한, 메모리를 할당하고 해제하는 과정에서도 Thread에 영향이 없어야 하기 때문에 이를 보장하기 위해 동작을 멈추는 등의 과정에서 성능에 영향이 올 수 있다.  

## Heap
Reference Type이 저장되는 곳이다.  

![ValueRef2](https://user-images.githubusercontent.com/55477102/129484504-060ffbab-32af-4644-b47c-acb5bb2efddc.png)  
위와 같이 완전 이진트리로 구성되어 있으며, 트리의 층에 따라 중요도를 나타낼때 주로 활용된다.  

## 차이점
위에서는 Value와 Reference에 대해 간단하게 알아보았다면, 가장중요한 Swift에서 둘의 사용할 때의 차이점을 알아보자.  
***  
처음으로 Value는 선언시에 해당 Value의 크기만큼 Stack에 할당하게 된다.  
그리고 Value를 다른 곳으로 전달 할 때 해당 Value를 [복제]한다.  
이해가 힘들 수 있으니 코드와 스크린샷을 통해 알아보도록 하자.  
```Swift
struct Value {
  var x: Int
  var y: Int
}

var value: Value = Value(x: 0, y: 0)
```
![ValueRef3](https://user-images.githubusercontent.com/55477102/129485804-26681d00-872f-4cb8-8b48-3d545a6bef6d.png)  
value type인 struct를 선언한 코드와 stack의 스크린샷이다.  
이러한 struct를 한번 복사하면 어떻게 될까?  

```Swift
struct Value {
  var x: Int
  var y: Int
}

var value1: Value = Value(x: 0, y: 0)
var value2: Value = value1
```
![ValueRef4](https://user-images.githubusercontent.com/55477102/129485803-dfe54e0b-5066-46d3-a633-c928c9b8d2d8.png)  
value1의 struct와 value1을 넘겨준 value2의 코드와 구성된 stack의 스크린샷이다.  

만일 위의 상황에서 value2의 y값을 50으로 변경하면 어떻게 될까?  

![ValueRef5](https://user-images.githubusercontent.com/55477102/129485798-d208cfb2-7ab4-4259-96e3-24f7956d835d.png)  
정답은 value2의 y값만 50으로 변경된다.  
위에서 설명 햇지만, Value Type은 값을 넘겨줄때 자신과 완전히 같은 하나의 값이 생겨난 것이다.  
그렇기에 자기 자신을 넘겨주었다 한들 value2의 값이 변해도 value1과는 아무런 연관이 없는 것이다.  
***  
Reference Type은 선언시에 Heap에 Value의 크기만큼을 할당하고 추가로 Reference관련 데이터들을 추가로 할당하게 된다.  
이는 Reference가 현재 사용되고 있는지 판단하여 ARC가 메모리를 해제시키는데 사용되는 Count 값과 같은 정보가 추가로 할당되는 것이다.  
그리고 이 Reference의 위치를 가지고 있는 데이터가 Stack에 할당된다.  

```Swift
class Value {
  var x: Int
  var y: Int
}

var value: Value = Value(x: 0, y: 0)
```
![ValueRef6](https://user-images.githubusercontent.com/55477102/129485784-30970c9b-d4a0-49fd-b1e8-179c973b3e11.png)  
위에서 설명했듯이 Reference Type인 class를 선언하였을때 해당 class + 추가 정보 만큼의 Heap 공간이 할당되고 데이터가 저장된다.  
그리고 Stack에 해당 Heap의 주소를 가진 값이 할당된다.  

이 Stack가 중요하다.
왜일까?
바로 값을 전달 할 때 Stack을 복사하기 때문이다.  
코드와 스크린샷을 통해 알아보자.  

```Swift
class Value {
  var x: Int
  var y: Int
}

var value1: Value = Value(x: 0, y: 0)
var value2: Value = value1
```
![ValueRef7](https://user-images.githubusercontent.com/55477102/129485956-832ce4ae-fecb-4eaa-b5df-84c50faf7f52.png)  
Value Type에서 struct를 복사했던 방식과 똑같이 class를 복사해보았다.  
하지만 둘은 완전히 다르다.  
자신과 똑같은 값을 가진 완전히 다른 Value를 Stack에 쌓았던 struct와 달리 class는 같은 Heap을 바라보는 Value를 Stack에 쌓았다.  

이 차이점은 해당 Value를 사용함에 있어서 완전히 다른 결과를 가져온다.  

Value class는 struct와 다르게 나를 넘겨준 곳에서 x와 y를 변경하면 자기 자신도 바뀌게 된다.  
```Swift
value2.x = 50

print(value1.x)
print(value2.x)

// 50
// 50
```
![ValueRef8](https://user-images.githubusercontent.com/55477102/129486140-ab4c972a-3311-4765-a66c-40d5a2c71b5b.png)   
위와 같은 결과를 가져오게 된다.  
value1과 value2가 바라보고 있는 곳이 같기 때문에 value2의 값을 바꾼다는 것은 value1의 값 또한 바뀐다.  
그리고 ReferenceCount 또한 1개가 증가한다.  
ARC는 이 Count가 0이 되었을때 메모리를 
***   
비교적 가볍게 Value와 Reference의 차이를 알아보았다.  
Value Type은 Reference Counting이 필요없고 Heap에 접근하지 않아 다량의 데이터를 사용 할 때 접근이 더 빠르고 내용 수정시에도 더 빠르다.  
하지만 Value Type들은 Class와 같은 Reference처럼 하나의 객체로 사용하기에 적합하지 않다.  
하지만 이 단점을 제외한 대부분의 상황에서 빠르 데이터 접근과 수정이라는 장점은 놓칠 수 없다.  
그렇기 때문에 적절한 곳에 Reference와 Value Type을 사용하면 앱을 더 빠르게 작동시킬 수 있다.  
