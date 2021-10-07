# DependencyInjection: 의존성 주입
의존성 주입은 말 그대로 어떠한 오브젝트 간에 의존성을 가진채로 선언되는게 아닌 선언 될 때 넣어주는 개념이라고 생각하면 될 것 같다.  
이 구조는 모든 개발자들이 좋아하는 테스트에 용이하고, 재사용성을 높이고, 의존관계를 낮추는 등의 여러 주제들을 조금, 혹은 많이 해결해주는 설계이다.  

DI를 알기전에 알아야 할 기초 정보를 하나 알고 가야하는데, 바로 의존성과 의존성을 주입한다는 것이 무엇인지를 알고 가야한다.  

### 의존성
의존성은 어떤 객체가 다른 객체에 의존을 하게 되는 것을 말한다.  
이 상황을 보여주기위해 MVVM의 구조중 ViewController와 ViewModel을 가지고 정말 짧고 간단한 예제를 만들어보았다.  

```Swift
class ViewModel {

  var list: [String] = ["a", "b", "c"]
  
}

class ViewController: UIViewController {

  var viewModel: ViewModel = ViewModel()
  
}
```
너무 별거 없지만, 이게 의존성이다.  
ViewController에서 ViewModel의 list를 쓰기위해 이미 선언 될 때 ViewModel에 대한 의존성을 가진채로 생성이 된다.  
간단한 예제를 예시로 들었지만, 의존관계가 이렇게 한몸처럼 묶여있다는 것은 그만큼 테스트와 재사용성이 떨어진다는 것을 의미한다.  

### 의존성 주입
의존성 주입은 위의 코드에서 크게 달라지는게 없다.  
말 그대로 객체를 선언 할 때 다른 객체를 전달해주는게 의존성 주입이다.  
```Swift
class ViewModel {

  var list: [String] = ["a", "b", "c"]
  
}

class ViewController: UIViewController {

  var viewModel: ViewModel
  
  init(withDependencyInjection value: ViewModel) {
    self.viewModel = value
    
    super.init(nibName: nil, bundle: nil)
  }
  
  required init?(coder: NSCoder) {
    fatalError("fatalError")
  }
  
}

// 의존성 주입
let vm = ViewModel()
let vc = ViewController(withDependencyInjection: vm)
```
선언 할 때 ViewController가 필요로 하는 ViewModel 객체를 주입했다.  
이게 의존성 주입이다.  

확실히 첫번째 예제보다는 나아보인다.  
하지만 우리가 원하는 것은 이것보다 더 의존성이 낮고, 결합도도 낮고, 재사용성도 더 높고, Swift 다운 DI를 원한다.  

### 의존관계의 역전
두번째 예제보다 의존관계를 더 낮추는 방법은 이 의존관계를 역전 시키는 것이다.  
역전이라니 무슨소리인가 싶겠지만, 객체 지향 언어의 원칙중 '의존관계 역전의 원칙'이 있다.

#### 의존관계 역전의 원칙
1. 상위 모듈은 하위 모듈에 의존해서는 안된다. 상위 모듈과 하위 모듈 모두 추상화에 의존해야 한다.  
2. 추상화는 세부 사항에 의존해서는 안된다. 세부사항이 추상화에 의존해야 한다.

핵심만 말하면, 
- ViewController는 ViewModel이 아닌 추상화에 의존해야 한다.  
- ViewModel 또한 추상화에 의존해야한다.  

눈치가 빠른 사람이라면 바로 알아차렸을 것이다.  
Swift에서 추상화는 Protocol이다.  

이 '의존관계 역전의 원칙'을 지켜 위에서 보았던 예제들을 수정해보자.  

```Swif
// 핵심에서 말했던, protocol을 선언해준다.
protocol ListDataProvider {
  var list: [String] { get set }
}

// ViewModel은 protocol에 의존한다.
class ViewModel: ListDataProvider {
  var list: [String] = ["a", "b", "c"]
}

class ViewController: UIViewController {

  // ViewController 또한 protocol에 의존한다.
  var viewModel: ListDataProvider
  
  // protocol을 통해 의존성을 주입받는다.
  init(withDependencyInjection value: ViewModel) {
    self.viewModel = value
    
    super.init(nibName: nil, bundle: nil)
  }
  
  required init?(coder: NSCoder) {
    fatalError("fatalError")
  }
  
}

// Dependency Injection
let vm = ViewModel()
let vc = ViewController(withDependencyInjection: vm)
print(vc.viewModel.list)

// 출력: ["a", "b", "c"]
```

![DI1](https://user-images.githubusercontent.com/55477102/136395512-1e18f9b0-0bf1-4bf9-81aa-6b9c49cb3198.png)  

위의 그림과 같은 의존성을 갖게 되었다.  
2번째 예제에서는 ViewController가 주입받는 타입은 오직 ViewModel 클래스였다.  
하지만, 마지막 예제에서는 ListDataProvider protocol만 만족한다면 어떠한 Class든 Struct든 넘겨 받을 수 있게된다.  
이 말은 즉, 위에서 몇번이나 말했던 테스트, 재사용성, 의존관계 등에 정말 좋은 설계라는 것이다.  
