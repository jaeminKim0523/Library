# EnvironmentObject란?
부모 또는 자식 뷰에서 하나의 객체를 공유하여 값이 변동에 대해 관찰 가능하도록 하는 property이다.
https://developer.apple.com/documentation/swiftui/environmentobject

# EnvironmentObject 전달

```swift
struct SwiftUITestApp: App {
    @ObservedObject var state = State()
    
    @SceneBuilder var body: some Scene {
        WindowGroup {
            NavigationView {
								// EnvironmentObject 전달
                MainView().environmentObject(state)
                    .sheet(isPresented: $state.isHidden) {
                        LoadingView().environmentObject(state)
                    }
            }
            
            
        }

        WKNotificationScene(controller: NotificationController.self, category: "myCategory")
    }
}
```

### 설명

- EnvironmentObject는 View의 environmentObject 메소드를 통해 전달 할 수 있다.


# EnvironmentObject 사용

```swift
struct MainView: View {
    
    @EnvironmentObject var state: State
    
    var body: some View {
				VStack{
		        Text("MainView: \(viewModel.state.isHidden.wrappedValue.description)")
		        
		        Toggle("Main environmentObject", isOn: $state.isHidden)
						
						SecondView()
				}
    }
}

struct SecondView: View {
    
    @EnvironmentObject var state: State
    
    var body: some View {
        Text("ThirdView: \(viewModel.state.isHidden.wrappedValue.description)")
        
        Toggle("Third environmentObject", isOn: $state.isHidden)
    }
}
```

### 설명

MainView의 @EnvironmentObject property로 선언된 state변수에 SceneBuilder에서 넣어준 state변수가 들어가있다.

그리고 SceneBuilder의 state를 MainView의 ChildView에서 같은 State type의 @EnvironmentObject property로 선언된 변수를 공유하게된다.

MainView에서 SecondView를 선언 할 때 MainView와 다르게 .environmentObject 메소드로 state를 넘겨주지 않았다.
하지만, SecondView의 state 변수의 타입이 MainView의 state와 타입이 같고 @EnvironmentObject property가 선언되었기 때문에 값을 공유하게된다.

### 작동

MainView와 SecondView의 Toggle SwitchUI를 작동시켜보면 UI의 상태가 함께 변경된다.
