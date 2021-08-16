# Closure 심화
중괄호"{}"로 감싸져서 사용되는 [함수]를 Closure라고 한다.  
우리가 사용하는 일반적인 함수는 이 Closure에 이름을 붙여서 사용하는 것일 뿐이다.  

조금 TMI 일 수 있지만, [Value, Reference Types]를 알고 있다면 추측이 가능하다.  
Function은 Reference Type이다.  
그렇다면 당연히 Closure도 Reference Type이 되는 것이다.  

다시 본론으로 들어가서 Closure는 호출된 함수에게서 인자를 받아서 함수로 작업을 한다.  
말로는 설명이 어려우니 간단하게 그림으로 표현해보자.  

<img width="347" alt="스크린샷 2021-08-16 오후 4 26 38" src="https://user-images.githubusercontent.com/55477102/129526670-468105f3-5712-489a-af16-5d453ab0f2e7.png">  
Closure는 Function과 같은 Reference Type이므로 Stack에 주소를 Heap에 해당 함수가 할당된다.  

우리가 Closure를 호출한다는 것은 이 함수를 호출한다는 것이다.  

```Swift
// 1번 코드
func closureTest(closure: (Bool) -> Void) {
  closure(true)
}

func main() {
  closureTest(closure: { (bool) in
    if bool {
      print("True")
    } else {
      print("False")
  })
}
```
위으 코드를 보았을때,
```Swift
// 2번 코드
{ (bool) in
  if bool {
    print("True")
  } else {
    print("False")
}
```
이 부분이 Function이 되는 것이고 위의 그림을 기준으로 Heap의 5000번이다.  
closureTest는 closure라는 이름의 함수를 전달 받겠다고 선언되어있는 것이다.  

<img width="359" alt="스크린샷 2021-08-16 오후 4 47 43" src="https://user-images.githubusercontent.com/55477102/129529254-905bdd18-4260-4680-96f4-389136678c71.png">  
그림이 이해를 도울 수 있을지 지신이 없다...  

하지만 별게 없다.  
closure는 Bool Type을 파라메터로 하는 함수를 파라메터로 받도록 선언되었고 그 함수가 2번 코드인 것이다.  
우리는 그저 함수를 파라메터로 넘겨주고 있었을 뿐이다.  

예시 코드 기준으로  
closureTest에 우리가 생성한 이름이 없는 함수를 넘겨주었고 closureTest함수는 파라메터로 받은 이름 없는 함수에 closure라는 이름을 붙인것이고 그 closure는 Bool Type을 받도록 선언되었기 때문에 closure함수를 호출 할 때 true를 넘겨준 것 뿐이다.  
우리는 단순히 Bool Type을 파라메터로하는 함수 하나를 closureTest함수에서 호출했을 뿐이다.  
그것이 closure의 전부다.  

Escaping closure는 [Escaping] 여기 링크를 참고하면 될 것 같다.  

[Value, Reference Types]: https://github.com/jaeminKim0523/Library/blob/main/ValueReferenceTypes.md "Read ValueReferenceTypes"
[Escaping]: https://github.com/jaeminKim0523/Library/blob/main/Escaping.md "Read Escaping"
