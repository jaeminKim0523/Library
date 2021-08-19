# Autolayout3 - Content Compression Resistance Priority
[Autolayout2]에서 Content Hugging Priority는 자신이 가진 Content이상으로 넓어질지에 대한 중요도였다면,  
Content Compression Resistance Priority는 그와 반대로 중요도가 낮은 UI의 Content의 크기를 무시하고 ...으로 표시하며 줄어들지에 대한 중요도라고 생각하면 될 것 같다.  

<img width="300" alt="스크린샷 2021-08-19 오후 10 34 23" src="https://user-images.githubusercontent.com/55477102/130078353-363a20ad-ea76-4e5c-80b8-e2b6db5da94f.png">
Content Compression Resistance Priority의 설정 위치는 [Autolayout2]에서 보았던 Content Hugging Priority 바로 아래 위치해있다.  

<img width="275" alt="스크린샷 2021-08-19 오후 10 34 41" src="https://user-images.githubusercontent.com/55477102/130078361-583fc63d-3fbe-4d56-8c4c-e312463751dd.png">
Content Compression Resistance Priority 알아보기 위해 두개의 UILabel을 생성하고 Constraint를 주었다.  
왼쪽 Label에는 긴줄의 Text를 넣었고 오른쪽 Label에는 짧은 줄의 Text를 넣었다.  
당연히 두 UI는 서로에게 Constraint로 의존을 하고 있는 상태이지만, 두 UI모두 크기가 가변이므로 너비를 정하지 못하고 Constraint에 오류가 생겨있다.  




[Autolayout2]: https://github.com/jaeminKim0523/Library/blob/main/Autolayout2.md "Read Autolayout2"
