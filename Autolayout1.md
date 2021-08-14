# Autolayout - Constraint
Autolayout의 가장 기본적인 Constraint를 알아보자.  

![Autolayout1](https://user-images.githubusercontent.com/55477102/129450034-688bbb60-d674-4bdb-9b58-c5819a0f3cc8.png)  
Constraint의 뜻은 강제이다.  
그럼 어떤 것을 강제하는 것일까?

바로 UI의 위치와 크기, 다른 UI와의 간격 등을 강제하는 것이다.  
Autolayout을 구성하기전에 Constraint를 어떻게 설정하는지 간단한 스크린샷으로 보고 넘어가자.  
<img width="275" alt="스크린샷 2021-08-14 오후 11 31 21" src="https://user-images.githubusercontent.com/55477102/129449780-ceb815d5-1807-4b35-9c6c-f4eb6cebb03b.png">  
Storyboard, Xib에서 우측하단 메뉴중 Add New Constraints메뉴(빨간색의 네모 박스 메뉴)를 누르면 나오는 창을 통해 Constraint를 추가 할 수 있다.  

글로는 이해하기 힘드니 스크린샷을 통해보자.  

<img width="363" alt="스크린샷 2021-08-14 오후 11 56 59" src="https://user-images.githubusercontent.com/55477102/129450406-e5d34ede-02df-4494-9573-b887f3a37b7e.png">  
UILabel을 하나 Storyboard의 ViewController에 생성해주었다.  
아직 Constraint가 설정되어 있지 않은 상태이며, 기본 값으로 ViewController의 View와의 간격이 주어진다.  

이제 UILabel의 상단과 좌측의 Constraint를 100으로 설정해보자.  

<img width="251" alt="스크린샷 2021-08-15 오전 12 15 33" src="https://user-images.githubusercontent.com/55477102/129450912-0d204fb4-07d0-4337-be0a-4510343b5bd6.png">  
<img width="396" alt="스크린샷 2021-08-15 오전 12 17 06" src="https://user-images.githubusercontent.com/55477102/129450966-895c1d55-8497-4b37-9bd1-ae91c44db6d3.png">  
위의 스크린샷을 보면 UILabel에 파란색 간격을 표시하는 UI가 생긴 것이 보인다.  
UILabel의 Constraint가 설정되었다는 표시이다.  

이 간격은 핸드폰이 가로 간격이 되어도, 화면의 크기가 달라져도 그 [간격]이 변하지 않는다.  
<img width="257" alt="스크린샷 2021-08-15 오전 12 22 34" src="https://user-images.githubusercontent.com/55477102/129451118-d09ecf21-9461-4836-93cf-3925d5cf565f.png">  
위의 스크린샷 처럼 화면이 가로가 되어도 설정한 상단 100의 간격과 좌측 100의 간격을 유지하고 있는다.  
  
<img width="488" alt="스크린샷 2021-08-15 오전 12 28 13" src="https://user-images.githubusercontent.com/55477102/129451261-19a48592-7766-4d4e-9e03-0b0cec8a2f57.png">  
UILabel을 하나 더 추가해주고 높이 100 간격, 좌우 50, 너비 100의 간격을 추가해보자.

<img width="255" alt="스크린샷 2021-08-15 오전 12 34 20" src="https://user-images.githubusercontent.com/55477102/129451524-efb5e243-9db9-4a64-b87c-c0aff68b504b.png"><img width="554" alt="스크린샷 2021-08-15 오전 12 39 34" src="https://user-images.githubusercontent.com/55477102/129451589-903f6e19-a0fb-4a9d-b71d-7a52f1398ab3.png">  
추가된 UILabel은 처음의 UILabel 우측과 50, 상단 100, 우측 50 그리고 너비 100의 Constraint가 설정되었다.  
이 간격은 가로가 되어도 변하지 않기 때문에 우측 스크린샷과 같이 가로모드가 되면 처음에 만들어진 UILabel의 너비가 길어지고 너비 100으로 설정된 UILabel은 현재 크기를 갖고 자동으로 레이아웃이 잡히게 된다.  
