# DispathQueue 사용법
### 목차
1. DispatchQueue의 특징과 설명
2. Queue의 종류와 설명
3. Main Queue와 Global Queue 차이
4. QoS(Quality of Service)
5. Sync와 Async

## DispatchQueue의 특징
DispatchQueue는 이름에도 나와있듯이 Dispatch[Queue]입니다.  
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
```Swift
let queue = DispatchQueue(label: "test")

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

### Concurrency 
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






