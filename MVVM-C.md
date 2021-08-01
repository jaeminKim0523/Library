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

간단한 코드를 예를 들면, 검색 버튼이 눌렸을 때 ViewModel을 통해 검색 데이터를 받아오느 코드.
```Swift
func searchBarSearchButtonClicked(_ searchBar: UISearchBar) {
        viewModel?.searchBook(search: searchBar.text ?? "")
}
```
테이블뷰의 셀 UI의 이미지를 표현하기 위한 데이터를 받아오느 코드.
```Swift
func tableView(_ tableView: UITableView, cellForRowAt indexPath: IndexPath) -> UITableViewCell {
        let cell = tableView.dequeueReusableCell(withIdentifier: "myCell", for: indexPath) as! MyCell
        let entity = viewModel?.entities[indexPath.row]
        cell.imageView = entity.image
}
```
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
ViewModel은 Model과의 데이터 송, 수신 및 Coordinator로 화면 구성 요청 등의 역할을 한다.  
자세한 역할은 코드와 함께 하나하나 짚어보도록 한다.  
***
첫번째로 Model과의 데이터를 주고받는 코드를 보면,  
```Swift
// 데이터 변화를 알리기 위한 Combine 
import Combine

// 검색 데이터를 알리 위한 PassthroughSubject
let searchSubject: PassthroughSubject = PassthroughSubject<Entity, Never>()

func searchBook(search keyword: String) {
        ApiService.loadSearchResult(keyword: keyword) { (data, response, error) in
                 guard let resultData = data else { return }
                 guard let resultJson = try? JSONSerialization.jsonObject(with: resultData, options: .mutableContainers) as? [String : Any] else {
                        return
                 }
                 
                 // Codable을 써도 됩니다.
                 let entity: Entity = Entity(dictionary: resultJson)
                 
                 // Combine의 PassthroughSubject를 통해 모델 전달
                 self.searchSubject.send(entity)
        }
}
```
위와 같은 코드로 표현 할 수 있다. (이해하기 쉽도록 View에서 사용한 코드와 연결되도록 코드 작성). 
간단하게 코드를 설명하면, ApiService Class를 통해서 검색 결과를 받아오는 함수이며, 이 결과를 Entity 구조체로 생성하고 그 entity를 Combine을 이용하여 전달하는 역할을 한다.  

Combine이 궁금하실 수 있지만 그 설명보다 MVVM에서 왜 RxSwift, Combine과 같은 비동기 데이터 전달이 필요한 이유가 중요하다.  
위에서 표현하기를 (View <-> ViewModel <-> Model) 이런 식으로 표현을 했지만, 이것은 데이터의 흐름일 뿐이다.  
의존성을 표현하자면, (View -> ViewModel -> Model + Coordinator) 이렇게 된다.  
View는 ViewModel을 알지만, ViewModel은 View를 모른다.  

ViewModel이 검색 결과를 Model로 부터 전달 받았다.  
하지만 ViewModel은 View를 모르기 때문에 View에게 검색 결과에 대한 데이터를 넘겨줄 방법이 없다.  
그럼 ViewModel이 View에게 데이터를 어떻게 받아야 할까?  

그 방법을 생각나는대로 적어보자면,
- @escaping closure를 이용하여 넘겨주는 방법
- View의 함수 자체를 넘겨주는 방법
- Rx, Combine과 같이 비동기 지원 라이브러리를 사용하는 방법
- Delegate를 이용하는 방법
등이 있을 것 같다. (더 많은 방법이 있을 수 있다.)

여러가지 방식중 MVVM 아키텍처를 사용하는 많은 곳에서 Rx, Combine과 같은 라이브러리를 사용하는 이유는.  
다른 방식보다 더욱 간편하게 구성이 가능하며, 코드를 간단하게 구성 할 수 있어 리딩하기에도 편리하다.
```Swift
func fetchSearchBook() {
        viewModel?.searchSubject.sink(receiveValue: { entity in
                print(entity)
        }).store(in: &cancelStore)
}
```
데이터의 변동에 대한 알림을 받고 데이터를 받아오는 것에 2줄이면 충분하게 끝나버린다.  
모든 라이브러리의 사용이 그러하듯 Rx와 Combine과 같은 비동기 데이터 바인딩을 도와주는 라이브러리 또한 기능을 구성하는 시간을 줄일 수 있기 때문에 사용하게 된다.  
***
두번째로 Coordinator를 이용하여 화면을 전환시켜보자.
```Swift
// ViewModel
func displayDetailViewController(entity: entity) {
        coordinator?.displayDetailViewController(entity: entity)
}
```
```Swift
// Coordinator
func displayDetailViewController(entity: entity) {
        let detailCoordinator: DetailCoordinator = DetailCoordinator(navigationController: navigationController)
        let detailViewModel: DetailViewModel = DetailViewModel(viewFlow: detailCoordinator, entity: entity)
        let detailViewController: DetailViewController = DetailViewController().create(viewModel: detailViewModel) as! DetailViewController

        navigationController.pushViewController(detailViewController, animated: true)
}
```
코드는 정말 별게 없다.
ViewModel은 Coordinator의 DetailViewController를 보여주기 위한 함수를 호출한다.  
여기서 중요한건 entity를 넘겨준다는 것이다.  
View는 Entity를 ViewModel로 부터 받아서 사용하기에 Entity를 가지고 있지 않는다.  
Coordinator는 DetailViewController를 보여주기 위해 필요한 데이터를 가진 Entity를 요구하고 그 Entity를 넘겨주기 위해 ViewModel을 통해 Coordinator를 사용하게 되는 것이다.  
***
### Model
Model은 오직 자기 자신만의 일을 한다.  
- API를 호출하기 위해 사용되는 Network Class  
- Keychain을 저장하기 위한 Class  
- Userdefaults를 사용하기 위한 Class. 
등등 수없이 많아질 수 있다.  

가장 많이 사용되는 Api 통신 Model을 간단한 코드로 표현하자면,  
```Swift        
class NetworkModel: NSObject {
        typealias CompletionHandler = (Data?, URLResponse?, Error?) -> Void

        private var urlSession: URLSession

        public var timeInterval: TimeInterval = 30

        func request(url: URL, completion: @escaping CompletionHandler) {
                var request = URLRequest(url: url)

                request.httpMethod = "get"
                request.timeoutInterval = timeInterval

                urlSession.dataTask(with: request, completionHandler: completion).resume()
        }
}
```
너무 간단하게 표현한 것 같지만, 이런 느낌일 것이다.  
request함수로 URL을 받아서 해당 url을 통해 데이터를 받아오 그 데이터를 @escaping closure를 통해 비동기로 리턴해준다.  
이러한 하나의 독립된 기능을 담당하는 것이 Model이 하는 역할이다.  

### 마무리
많이 부족하지만 MVVM-C에 대해 개인적으로 공부하고 리펙토링도 해보며 깨닳고 배운것들을 작성해보았다.  
MVVM-C에 대하여 공부하며 가장 많이 하게된 생각은 "이게 맞는가?" 라는 자기 자신에게 던지는 질문이었고  
두번째 생각은 결국 아키텍처에 맞춰 프로그래밍을 한다는 것은 나 혹은 같이 일하는 동료와의 약속을 잘 지켜야 한다는 생각이었다.  

사실 위에 작성한 것들이 정말 제대로 이해하고 작성했는지도 확신이 들지 않는다.  

문제점이나 수정해야 할 부분이 있다면, 메일 부탁드립니다.  
