# Autolayout3 - Content Compression Resistance Priority
[Autolayout2]에서 Content Hugging Priority는 자신이 가진 Content이상으로 넓어질지에 대한 중요도였다면,  
Content Compression Resistance Priority는 그와 반대로 중요도가 낮은 UI의 Content의 크기를 무시하고 ...으로 표시하며 줄어들지에 대한 중요도라고 생각하면 될 것 같다.  

<img width="300" alt="스크린샷 2021-08-19 오후 10 34 23" src="https://user-images.githubusercontent.com/55477102/130078353-363a20ad-ea76-4e5c-80b8-e2b6db5da94f.png">
Content Compression Resistance Priority의 설정 위치는 [Autolayout2]에서 보았던 Content Hugging Priority 바로 아래 위치해있다.  

<img width="275" alt="스크린샷 2021-08-19 오후 10 34 41" src="https://user-images.githubusercontent.com/55477102/130078361-583fc63d-3fbe-4d56-8c4c-e312463751dd.png">
Content Compression Resistance Priority 알아보기 위해 두개의 UILabel을 생성하고 Constraint를 주었다.  
왼쪽 Label에는 긴줄의 Text를 넣었고 오른쪽 Label에는 짧은 줄의 Text를 넣었다.  
당연히 두 UI는 서로에게 Constraint로 의존을 하고 있는 상태이지만, 두 UI모두 크기가 가변이므로 너비를 정하지 못하고 Constraint에 오류가 생겨있다.  

이 오류를 해결하기 위한 방법이 Content Compression Resistance Priority이다.  
중요도에 따라 어떤 UI의 Content를 포기하고 어떤 UI를 우선시 할지 Content Compression Resistance Priority의 값을 설정하면 해결 할 수 있다.  
***  
<img width="263" alt="스크린샷 2021-08-19 오후 10 35 09" src="https://user-images.githubusercontent.com/55477102/130078374-cbf2d97e-0640-4672-9cff-4ecac43d7184.png">  
<img width="298" alt="스크린샷 2021-08-19 오후 10 35 00" src="https://user-images.githubusercontent.com/55477102/130078368-230c2fb5-7bec-4401-b318-ae94c7659a1b.png">  
왼쪽 UILabel의 가로 Priority를 증가시켰다.  
이것은 왼쪽 UI의 Content 중요도가 오른쪽 UI의 Content보다 중요하게 여겨진다는 의미이고,  
왼쪽 UI의 Content를 표현하기 위해서 오른쪽 UI의 Content가 보이지 않게 되는 것을 허용한다는 의미이다.  
Content Hugging Priority와 다른점은,  
Content Hugging Priority는 중요도가 낮을때 UI의 너비가 Content보다 넓어져도 된다는 의미의 중요도이고,  
Content Compression Resistance Priority는 중요도가 낮을때 나보다 높은 중요도를 가진 UI가 나의 Content를 줄여도 된다는 의미의 중요도이다.  

***  

글로는 이해가 어려울 수 있으니 스크린샷을 통해 두가지 모두 사용하는 케이스를 알아보자.  
  
<img width="303" alt="스크린샷 2021-08-19 오후 10 35 21" src="https://user-images.githubusercontent.com/55477102/130078381-6a2c4709-efbb-45d9-afb3-60ee47e00667.png">  
왼쪽 UI의 Content Compression Resistance Priority = 751 / Content Hugging Priority = 250   
오른쪽 UI의 Content Compression Resistance Priority = 750 / Content Hugging Priority = 251   
로 설정하였다.  


<img width="262" alt="스크린샷 2021-08-19 오후 10 35 26" src="https://user-images.githubusercontent.com/55477102/130078389-2ecbff20-d6b3-4d7e-9437-a202aafa8301.png">  




[Autolayout2]: https://github.com/jaeminKim0523/Library/blob/main/Autolayout2.md "Read Autolayout2"
