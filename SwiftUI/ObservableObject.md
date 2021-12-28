# ObservableObject를 이용한 데이터 바인딩

ObservableObject는  
```Swift
import SwiftUI

// MARK: - ViewModel
class ViewModel: ObservableObject {
    
    @Published var color: Color = .red
    
}


// MARK: - View1
struct ContentView: View {
    
    @ObservedObject var viewModel: ViewModel = ViewModel()
    
    var body: some View {
        VStack{
            Spacer()
            ContentView2(color: $viewModel.color)
            Spacer()
            ContentView2(color: $viewModel.color)
            Spacer()
        }
    }
}

// MARK: - View2
struct ContentView2: View {
    
    @Binding var color: Color
    
    var body: some View {
        VStack{
            ColorPicker("color", selection: $color)
        }
    }
}
```
