# DispathQueue 사용법
### 목차
1. DispatchQueue의 특징과 설명
2. Queue의 종류와 설명
3. Main Queue와 Global Queue 차이
4. QoS(Quality of Service)
5. Sync와 Async

## DispatchQueue의 특징
DispatchQueue는 이름에도 나와있듯이 Dispatch[Queue]다.  
Queue란?  
<img width="403" alt="스크린샷 2021-08-02 오후 8 44 12" src="https://user-images.githubusercontent.com/55477102/127856831-26205092-5d2e-4997-b8db-a25e82f5ea89.png">  
위의 스크린샷처럼 처음에 들어가서 처음으로 나오는 것(FIFO: First In First Out)이 Queue이다.  

TMI일 수 있지만, Stack은 LIFO(Last In First Out)이이며, struct와 같은 value type들이 저장되는 Stack이다.  

다시 DispatchQueue로 돌아와서 위에서 설명한 Queue의 FIFO 설명을 보면 Queue의 특성도 짐작 할 수 있다.  
Queue에 작업(이하 Task)이 추가되면, 먼저 들어온 Task부터 순차적으로 처리하게된다.  
우선순위에 따라 달라질 수 있지만, 이 상황은 아래 QoS에서 설명하겠다.  

DispatchQueue는 간단하게 말했지만, 동기와 비동기식 작업을 하는 Task를 효율적이고 편리하게 사용 및 관리를 할 수 있게 해준다.  
![queue1](https://user-images.githubusercontent.com/55477102/128028335-254e7478-e0a0-4f56-9a18-89fb8f7eb183.png)  
하나의 Queue에 Task가 5개가 있다고 가정한 그림이다. (1T, 2T, 3T는 작업의 소요 시간이다.)  
Queue는 한번에 하나의 작업씩 Task가 들어온 순서대로 작업을 수행한다.  
그렇기 때문에 Task1이 2Time을 사용하는 동안 나머지 Task들은 기다리고 Task1이 종료되면 다음으로 들어온 Task2가 3Time동안 수행되는 방식이다.  
DispatchQueue가 작업하는 방식은 위의 예시정도로 간단하게 설명하고 넘어 갈 수 있을 것 같다.  

## Queue의 종류와 설명
DispatchQueue에는 Serial, Concurrency 두가지의 Queue가 존재한다.  
이번 차례에서는 이 두가지를 알아보고자 한다.  
### Serial  
![queue1](https://user-images.githubusercontent.com/55477102/128028335-254e7478-e0a0-4f56-9a18-89fb8f7eb183.png)  
Serial 방식은 특징에서 보았던 스샷처럼 [1개의 쓰레드]에서 Task를 순차적으로 처리한다.  
코드로 텍스트를 출력해보면 더욱 이해하기 쉬울 것 같다.  
```Swift
let queue = DispatchQueue(label: "test")

// 1번 Task
queue.async {
  for num in 0 ..< 5 {
    print(num)
  }
}
// 2번 Task
queue.async {
  for num in 100 ..< 105 {
    print(num)
  }
}

// 출력
// 0
// 1
// 2
// 3
// 4
// 100
// 101
// 102
// 103
// 104
```
DispatchQueue를 선언 할 때 아무 옵션도 주지않은 것을 볼 수 있다.    
이렇게 아무런 옵션도 주지 않는다면, Serial Queue로 선언된다.  
위에서 설명 했듯이 Serial Queue의 방식 그대로 Task 순서대로 출력이 되는 것을 볼 수 있다.

### Concurrency 
![DispatchQueue](https://user-images.githubusercontent.com/55477102/128353147-a63f09f3-ca31-492d-b9eb-6697bb7b48a9.png)  
Concurrency Queue는 위의 스크린샷처럼 동시에 여러개의 Task를 [여러개의 쓰레드]로 분산시켜서 처리하는 방식이다.  
작업의 순서나 작업의 분할은 모두 시스템에서 하기 때문에 그 순서를 확정 지을 수 없다.  
코드를 통해 알아보도록 하자.

```Swift
let queue = DispatchQueue(label: "test", attributes: .concurrent)

queue.async {
  for num in 0 ..< 5 {
    print(num)
  }
}
queue.async {
  for num in 100 ..< 105 {
    print(num)
  }
}

// 출력
// 100
// 0
// 1
// 2
// 3
// 4
// 101
// 102
// 103
// 104
``` 
Concurrency Queue는 DispatchQueue를 선언 할 때 attributes 파라메터에 concurrent를 넘겨주어 설정한다.  
출력 내용을 보면, 순서와 상관없이 코드가 출력되는 것을 알 수 있다.  
위에서 대괄호로 강조를 했듯이 여러개의 쓰레드로 분산시켜서 Task를 처리하기 때문에 1개의 쓰레드에서 순차적으로 Task를 처리하는 Serial Queue보다 속도가 더 빠르다.  

## Main Queue와 Global Queue 차이


## QoS(Quality of Service)


## Sync와 Async


