# Autolayout2 - Content Hugging Priority
Autolayout1에서는 Constraint를 설정하는 기초를 알아보았다면, 2에서는 조금 심화된 내용인 Content Hugging Priority 알아보려고 한다.  
Content Hugging Priority를 직역하면, 콘텐츠가 안는 우선순위 이다.  
이 의미는 말로 설명하기보다는 스크린샷과 사용방법을 설명하면서 이해하는 것이 좋을 거 같다.  

<img width="256" alt="스크린샷 2021-08-15 오후 3 25 19" src="https://user-images.githubusercontent.com/55477102/129469527-89084b25-9656-4746-b071-b966b906feb9.png">  
Content Hugging Priority는 우측 메뉴 Size Inspector 탭의 아래쪽에서 설정 할 수 있다.  

어디서 설정하는지도 알아보았으니 본론으로 들어가보도록 하자.  

<img width="279" alt="스크린샷 2021-08-15 오후 3 23 52" src="https://user-images.githubusercontent.com/55477102/129469521-24153745-7108-4da9-804b-9c5ba1ca81fc.png">  
UILabel 2개를 생성하고 각 Label에게 좌우 Constraint를 주었더니, Constraint에 오류가 생겼다.  

왜일까?  
이유는 UILabel의 특성에 있다.  
UILabel은 좌우 Constraint나 너비를 고정시켜주지 않으면 UILabel의 Text에 따라 길이가 가변된다.  
하지만 위의 스크린샷에서 각 UILabel은 너비가 고정되어 있지 않을 뿐더러, 좌우 Constraint는 존재하지만 서로 의지하는 대상(각 UILabel)의 너비가 가변이다.  
의지하는 대상이 어느 크기만큼 커질지 작아질지를 알아야 나의 크기를 계산 할 텐데 UILabel의 가변성 때문에 계산을 하지 못하는 것이다.  

그럼 이 문제를 해결해보기 위해 이번 주제인 Content Hugging Priority를 써보도록 한다.  
Content Hugging Priority는 위에서 설명했지만, 중요도를 설정하는 것이다.  
중요도를 높이거나 낮춰서 내가 의지하는 대상보다 나의 크기를 더 우선시 할지 대상을 더 우선시할지를 선택하는 것이다.  

<img width="251" alt="스크린샷 2021-08-15 오후 3 24 31" src="https://user-images.githubusercontent.com/55477102/129469789-4c6abd3c-3155-466a-a1d7-bb4eeeb58769.png">  
Content Hugging Priority 설정 초기값은 251이다.  
우리는 2개의 UILabel에 설정한 우선순위에 따라 가로로 넓어지거나 작아지도록 해볼 것이기 때문에 test1 UILabel의 Horizontal값을 252로 올려보자  
설정한다면, test1의 우선순위는 252 / test2의 우선순위는 251이 된다.  
test1가 test2보다 더 중요해진 것이다.  

<img width="262" alt="스크린샷 2021-08-15 오후 3 53 17" src="https://user-images.githubusercontent.com/55477102/129469858-1a728747-8b28-4ef5-8b80-93ca75e268b8.png">  
test1 UILabel의 Horizontal값을 252로 설정한 결과이다.  
test1 UILabel이 test2 UILabel보다 더 중요해졌고 그로인해 test1의 너비가 test1이라는 텍스트 길이에 딱 맞게 너비가 정해졌다.  
그리고 중요도가 낮은 test2 UILabel의 너비는 자신의 Text와는 상관없이 길어지게 되었다.    
각 UILabel이 서로의 가변성에 의해 자신의 길이가 어느정도가 되어야 하는지 계산이 불가능했기에 생겼던 오류 또한 우선순위에 의해 해결된 것이다.  

반대로 test1 UILabel의 Horizontal값을 251에서 250으로 낮춰보면 어떻게 될까?  

<img width="267" alt="스크린샷 2021-08-15 오후 4 05 28" src="https://user-images.githubusercontent.com/55477102/129470103-055e3caa-4566-4e16-8d11-4696fb847284.png">  
당연히 test1 UILabel의 너비보다 test2 UILabel의 너비가 더 중요해졌고 test1 UILabel은 자신의 너비를 포기하고 test2 UILabel의 너비에 맞춰서 자신의 너비를 넓히게 된다.  

 가변형인 두개의 UI에 Autolayout을 구성하고자 할 때 사용하는 방법중 하나이다.
