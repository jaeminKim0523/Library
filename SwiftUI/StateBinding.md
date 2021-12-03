# State를 이용한 바인딩
@State 어노테이션을 이용하여 데이터를 바인딩하는 방법을 알아본다.  

```Swift
struct ContentView: View {
    
    @State var text: String = "Hello, world"
    
    var body: some View {
        VStack{
            TextField("input", text: $text, prompt: nil)
            Text(text)
        }
    }
}
```
너무 양심 없게 짧지만, 조금 더 심화한 코드는 잠시뒤에 보기로 하고 바인딩 방식만 알아보자.  

첫번째: ``` @State var text: String = "Hello, world"```를 보면, 문자열 형식으로 text변수를 선언하였고 @State 어노테이션을 사용했다.  
두번째: TextField의 ```text: $text```가 핵심이다.  State로 선언한 변수 앞에 $를 붙이게되면, 해당 값이 바인딩하기 위한 값으로 변한다.  

너무 편하다... $만 붙여도 Binding<String>으로 파싱이 된다니...  

<img width="252" alt="스크린샷 2021-12-03 오후 7 41 40" src="https://user-images.githubusercontent.com/55477102/144589292-887c96c6-8769-45eb-a790-3655b73bfc5e.png">

무슨 저거로 파싱이라고 하냐 할 수 도 있지만, 커서를 옮겨보면 실제로 그렇다.  
사실 처음에는 &랑 같은 주소를 넘겨주는 개념인가? 했는데, 정말 $이거 하나로 파싱이 되어버린다.  
  
조금 더 심화시켜보자.
  
```Swift
struct ContentView: View {
    
    @State var color: Color = .red
    
    var body: some View {
        VStack{
            Spacer()
            ContentView2(color: $color)
            Spacer()
            ContentView2(color: $color)
            Spacer()
        }
    }
}

struct ContentView2: View {
    
    @Binding var color: Color
    
    var body: some View {
        VStack{
            ColorPicker("color", selection: $color)
        }
    }
}
```
