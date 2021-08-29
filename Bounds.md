# Bounds 란?

![bounds1](https://user-images.githubusercontent.com/55477102/131249544-efcd1c78-3154-4389-a984-9ae86865ad7e.png)  

Apple Document에서 Bounds를 자체 좌표계에서 View의 위치와 크기를 가진 사각형이라고 설명한다.  
여기서 주목할 부분은 [자체 좌표계] 일 것이다.  

```Swift
let containerView = UIView()
self.view.addSubview(containerView)

containerView.frame = CGRect(x: 50, y: 50, width: 50, height: 50)

print(containerView.bounds)
// 출력: (0.0, 0.0, 50.0, 50.0)
```
Bounds는 자신의 자체 좌표계를 갖기 때문에 frame에 x를 50, y를 50으로 설정하였어도 bounds는 0, 0이다.  
frame을 아무리 바꾸어도 bounds의 좌표는 0, 0을 갖게된다.  

그럼 bounds는 어떻게 변경할까?  

```Swift
let firstView = UIView()
let secondView = UIView()

self.view.addSubview(firstView)
firstView.addSubview(secondView)

// 1번
firstView.frame = CGRect(x: 100, y: 100, width: 200, height: 200)
secondView.frame = CGRect(x: 50, y: 50, width: 50, height: 50)
print(secondView.frame) // (50.0, 50.0, 50.0, 50.0)
print(secondView.bounds) // (0.0, 0.0, 50.0, 50.0)

// 2번
firstView.bounds.origin = CGPoint(x: 50, y: 50)
print(secondView.frame) // (50.0, 50.0, 50.0, 50.0)
print(secondView.bounds) // (0.0, 0.0, 50.0, 50.0)
```

<img width="283" alt="스크린샷 2021-08-29 오후 10 30 49" src="https://user-images.githubusercontent.com/55477102/131252223-9d4a4b23-35da-44a5-b5ff-e360f36abbfb.png">  

1번 코드에서의 UI 스크린샷이다.  

<img width="276" alt="스크린샷 2021-08-29 오후 10 35 59" src="https://user-images.githubusercontent.com/55477102/131252702-6b31e15c-827b-470f-8284-025c732e6285.png">  

2번 코드에서의 UI 스크린샷이다.  
일단 몇가지를 살펴보면, 
1. secondView의 frame은 변화가 없다.
2. secondView의 bounds도 변화가 없다.
3. 변한것은 firstView.bounds 좌표를 x: 0, y: 0에서 x:50, y:50으로 바뀌었다.
4. 변한것은 firstView.bounds이지만, UI는 secondView의 위치가 바뀌었다. 
이런 사항들이 있다.  

위의 내용으로 알 수 있는 것은, 부모 View의 bounds를 조정하면 자식 View들의 위치를 수정하지 않아도 해당 위치만큼 자식 View들의 위치를 바꿀 수 있다는 것이다.  
이러한 bounds의 기능을 우리는 어디서 사용해왔을까?  

바로 ScrollView이다.  
ScrollView는 화면을 스크롤 할 때 ScrollView의 모든 자식 View를 옮기는 것이 아니라 bounds를 수정해서 마치 화면이 이동되는 것 처럼 표현 할 수 있는 것이다.  
