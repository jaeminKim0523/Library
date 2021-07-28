# MVVM, MVVM-C 아키텍처
### 목차
- MVVM + 한줄 설명
- MVVM-C + 한줄 설명
- 아키텍처 설명

## MVVM
한줄 설명: [M]odel - [V]iew - [V]iew[M]odel 글자를 모아서 부르는 아키텍처이다.  
각각의 자세한 역할은 아래에서 설명한다.  

## MVVM-C
한줄 설명: MVVM은 같지만 Coordinator가 추가로 붙은 아키텍처이다.  
Coordinator은 간단하게 화면의 생성과 흐름을 관리하는 역할을 한다고 생각하면 될 것 같다.  

## 설명
MVVM은 View - ViewModel - Model로 역할을 나누어 서로의 의존성을 낮추기 위해 사용하는 아키텍처이다.  
View - UI의 역할만을,  
ViewModel - 화면의 모델을 기능 역할을,  
Model - 자신의 고유한 역할만을 담당한다.

그렇다면 MVVM-[Coordinator]의 역할은 무엇일까?
Coordinator - 화면의 생성과 흐름을 담당한다.

***
<img width="415" alt="스크린샷 2021-07-22 오후 10 09 53" src="https://user-images.githubusercontent.com/55477102/126779960-8aae4996-fb50-4ed4-914c-4626040e1fad.png">
MVVM-C의 흐름을 표현한 간단하게 작성한 플로우 차트이다.   

Coordinator의 화살표가 애매하지만... 화살표의 정보에 중점을 두면 될 것 같다.  
Coordinator는 View를 create, present, push, pop, dismiss 등의 역할을 한다.  
View는 UserInteraction을 받아서 ViewModel에 Action을 보낸다.  
#### ViewModel의 대상이 Model 일때    
ViewModel은 Action에 해당하는 데이터를 Model에 요청한다.  
Model은 ViewModel이 요청한 데이터를 반환하여 준다.
ViewModel은 자신을 알고 있는 View들에게 데이터가 변한것을 알린다.
View는 알림을 받고 ViewModel로 부터 데이터를 가져와서 UI에 바인딩한다.
#### ViewModel의 대상이 Coordinator 일때  
ViewModel은 Coordinator에 필요로 하는 데이터를 전달하고 화면 전환을 요청한다.
Coordinator를 ViewModel이 알고 있는 이유는 전환하고자 하는 화면이 필요로하는 데이터를 전달하여야 하기 때문에 해당 데이터를 관리하는 ViewModel에서 Coordinator를 알고 있는 구조가 된다.
***
### Coordinator
#### Step 1
앱이 시작되면 맨 처음 AppDelegate에서 시작을 하기 때문에 첫 시작에 AppDelegate를 추가해두었다.  
당연한 이야기지만, AppDelegate에서 첫 화면을 생성해서 보여주어야 한다.
MVVM-C에서 화면을 생성하고 표현한다는 것은 곧 Coordinator가 필요하다는 말이 된다.
```Swift
func application(_ application: UIApplication, didFinishLaunchingWithOptions launchOptions: [UIApplicationLaunchOptionsKey: Any]?) -> Bool {
        
  let rootNavigationController = UINavigationController()
  appCoordinator = AppCoordinator(navigationController: rootNavigationController)
  window?.rootViewController = rootNavigationController
  window?.makeKeyAndVisible()

}
```
예를 들면 이러한 구성의 코드가 될 것 같다.  
첫 화면을 만들어주는 AppCoordinator를 만들고 AppCoordinator는 앱의 첫 화면을 갖게된다.

#### Step 2
첫 화면을 컨트롤하기 위한 Coordinator가 생성되었다면, Coordinator를 이용하여 첫 화면을 생성하고 넣어주어야 한다.  

```Swift
class AppCoordinator {
  
  let navigationController: UINavigationController

  init(navigationController: UINavigationController) {
      self.navigationController = navigationController
  }

  func start() {
      // SplashVC의 화면 흐름을 담당하기 위한 Coordinator 선언 및 RootVC를 SplashCoordinator에게 전달.
      let splashCoordinator: SplashCoordinator = SplashCoordinator(navigationController: navigationController)
      // SplashVC의 ViewModel에 SplashCoordinator를 전달한다.
      let splashViewModel: SplashViewModel = SplashViewModel(viewFlow: splashCoordinator)
      // SplashVC에 ViewModel을 전달한다.
      guard let splashVC = SplashViewController().create(viewModel: splashViewModel) else {
          return
      }

      navigationController.setViewControllers([splashVC], animated: false)
  }
}
```
Splash화면을 보여준다는 가정의 코드이다.
[Step1]에서 AppCoordinator에 RootViewController를 주었으므로 현재 AppCoordinator의 navigationController는 RootViewController가 된다.
AppCoordinator의 navigationController에 SplashViewController를 set한다는 것은 현재 화면에 SplashViewController가 보여지게 되는 것이다.

#### Step 3
코드를 보면 알겠지만,  
SplashVC(View) - ViewModel를 안다. 하지만 Model과 Coordinator는 모른다.
SplashVM(ViewModel) - Model, Coordinator를 안다. 하지만 View는 모른다.
SplashCoordinator(Coordinator) - 자신이 담당할 View를 안다. 하지만 Model, ViewModel은 모른다.

주의 할 점은  
Coordinator가 화면의 흐름을 담당한다고 해서 Coordinator에 자신이 담당해야 할 화면단이 아닌 이전에 보여주었던 화면등을 넘겨주지 않도록 설계 해야한다.

조금 복잡하지만 Coordinator를 간단하게 요약해보면,  
- AppDelegate 혹은 ViewModel 등에서 데이터를 받아 화면을 생성하거나 이동시키는 역할을 한다.  
- 현재 보여지고 있는 화면과 같은 수준의 화면만을 넘겨준다. (같은 수준이라 함은 Present의 기준이라고 생각하면 될 것 같다.)
- Coordinator는 ViewModel이 소유한다. 그 이유는 화면을 생성, 이동 시킬때 데이터를 넘겨주어야 할 수 있기 때문이다.

### View
View는 Swift에서 View와 ViewController, UserInteraction등의 UI요소를 담당한다.   
만약 TableView or CollectionView와 같이 리스트를 보여주게 될 때 해당 리스트의 데이터를 표현하는 것은 View에서 한다.  
표현하고자 하는 데이터는 ViewModel로 부터 받아오는 것이다.

이렇게 리스트가 보여질때는 Delegate 패턴을 통해 ViewModel로 부터 데이터를 받아오는 것은 쉽게 받아 올 수 있다.
View에서 검색한 결과를 받아온다는 상황을 가정해보자.
1. 검색어를 입력한다.
2. 검색 버튼을 누른다.
3. ViewModel에 검색 요청을 한다.
4. ViewModel은 Model을 통해 검색에 대한 데이터를 받아온다.
5. Model은 데이터를 ViewModel에게 전달한다.
6. ViewModel은 전해받은 데이터를 가지게 된다.
7. ViewModel은 데이터 변경의 정보를 알린다.

해당글 상단의 흐름도를 보면 View <-> ViewModel <-> Model의 상황이 되는 것이다.

View는 단순하게 ViewModel에 데이터를 요청하고 새로운 데이터를 받아와서 UI에 표현하는 역할만 한다.
ViewModel을 옵셔널로 선언하여 View만을 테스트 하고자 할 때 ViewModel을 사용하지 않을 수 있다.

### ViewModel

### Model








