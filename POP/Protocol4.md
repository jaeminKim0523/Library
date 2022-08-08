# Protocol - 4장
이번장에서는 이전 장들과는 다르게 내용이 많은 주제인, Protocol과 Delegate를 알아보려고 한다.

### Delegate Pattern
들어가기에 앞서 우리는 Delegate 패턴에 대해 알아야 할 필요가 있다.  

Delegate Pattern은 iOS를 개발해본 개발자라면 한번쯤은 사용해보았을 것이다.  
우리는 많이 사용되는 코드를 통해서 Delegate Pattern을 알아보고자 한다.  
```Swift
class ViewController: UIViewController {
    
    var tableView: UITableView = UITableView(frame: .zero)
    
    // MARK: - Lifecycle Methods
    override func viewDidLoad() {
        super.viewDidLoad()
        
        // Configure
        self.configureTableView()
        
    }
    
    // MARK: - Configuration Method
    func configureTableView() {
        self.tableView.delegate = self
    }
}
```
위의 코드에서 ```self.tableView.delegate = self``` 코드를 작성하면,  
"Cannot assign value of type 'ViewController' to type 'UITableViewDelegate?" 라는 오류가 발생하게 된다.  

왜 오류가 발생하는 것일까?  
이 질문에 대한 근본적인 궁금증을 해소시키기 위해서는 TableView에 대해 자세하게 알아 볼 필요가 있다.  
그리고 지금부터 알아보고자 하는 방법은 이후에 iOS를 개발하면서 많은 궁금증들 또한 해소시켜 줄 수 있는 방법이다.  

<img width="654" alt="스크린샷 2022-08-08 오후 8 26 13" src="https://user-images.githubusercontent.com/55477102/183407675-78f04697-7531-49a7-80d7-228e378e6fbe.png">

UITableView를 오른쪽 클릭하여 목록을 열고 "Jump to Definition" 메뉴를 선택하면 아래와 같은 메뉴가 추가로 나온다.  

<img width="497" alt="스크린샷 2022-08-08 오후 8 27 19" src="https://user-images.githubusercontent.com/55477102/183408126-ee7076c2-7d65-4bf1-a9cc-a6d9abc41c61.png">

우리는 UITableView를 알아볼 것이기 때문에 "UITableView" 메뉴를 선택한다.  
그러면 화면이 아래와 같이 이동된다.   

<img width="1112" alt="스크린샷 2022-08-08 오후 8 30 06" src="https://user-images.githubusercontent.com/55477102/183408368-7a2d07c7-63fc-4dfd-ac76-071d3a0b34cb.png">

우리는 위 스크린샷의 코드중 알아보고자 하는 469번째 줄의 UITableView의 Delegate에 마우스 커서를 올려보자.  

<img width="254" alt="스크린샷 2022-08-08 오후 8 31 49" src="https://user-images.githubusercontent.com/55477102/183408654-a9d3da09-fefa-4b8c-bbef-dce95b1587c9.png">

다른 내용을 보기보다는 Declaration영역에서 UITableViewDelegate가 어떤 형식으로 선언되어있는지를 알수있다.  
UITableViewDelegate는 protocol로 선언되어 있는 것을 알 수 있다.  

다시 위에서 보았던 코드로 돌아가보자.
```Swift
class ViewController: UIViewController {
    
    var tableView: UITableView = UITableView(frame: .zero)
    
    // MARK: - Lifecycle Methods
    override func viewDidLoad() {
        super.viewDidLoad()
        
        // Configure
        self.configureTableView()
        
    }
    
    // MARK: - Configuration Method
    func configureTableView() {
        self.tableView.delegate = self
    }
}
```
위에서 작성된 ```self.tableView.delegate = self``` 코드를 보면, 이제 어느정도 감이 올 것이다.  
우리는 UITableView에 선언된 delegate의 이름을 가진 protocol에 ViewController를 넣어주고 있었던 것이다.  

그리고 [Protocol - 1장]에서 알아보았던, protocol에서 요구하는 값, 함수를 ViewController에서도 구현해야 하지만, 현재 위의 코드에서는 protocol에서 요구하는 값 또는 함수를 구현하지 않았기 때문에 오류가 발생하고 있는 것이다.  
그러면 UITableViewDelegate protocol에서 요구하는 값 또는 함수를 선언해보자.  



[Protocol - 1장]: https://github.com/jaeminKim0523/Library/blob/main/POP/Protocol1.md ""
