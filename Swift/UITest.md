# UITest
UITest를 하는 방법을 알아보자.

<img width="533" alt="스크린샷 2021-10-23 오후 10 33 01" src="https://user-images.githubusercontent.com/55477102/138558752-7dbd3e24-ab6c-4b06-80be-087bd27e99e3.png">  

UITest를 시작하기 위해 프로젝트를 하나 만든다면, 프로젝트를 만들때 Include Tests를 체크해주어야 한다.  
만약 기존 프로젝트에 UITest를 추가해주어야 한다.  
방법은 File -> New -> Target -> iOS -> UI Testing Bundle 을 추가하면 된다.  

***  
### Storyboard
<img width="924" alt="스크린샷 2021-10-23 오후 10 26 51" src="https://user-images.githubusercontent.com/55477102/138559279-ac753ae0-76fc-456f-82f5-da36e36bc595.png">

테스트를 하고자 하는 스토리보드 구성이다.  
우리는 네모 박스의 1번과 2번 UI를 테스트 할 것 이고, 1번 리스트를 눌러서 다음 화면으로 넘어갔다가 다시 되돌아 오는 흐름으로 정말 간단한 UI 테스트를 해보자.  
(ViewController Class에 구현된 코드는 아무것도 없고 Storyboard로만 구성하였다.)

***
### UITest
<img width="1028" alt="스크린샷 2021-10-23 오후 11 01 44" src="https://user-images.githubusercontent.com/55477102/138559765-5877b9a1-30be-44a9-ba70-311aff00dbdf.png">  

UITests 파일을 보면 위와 같은 기본 코드가 있다.  
```setUpWithError()```는 각 테스트가 실행되기 전에 불리는 함수다.  
내용을 보면 아래와 같은 기본 설정 코드가 있다.  
```Swift
continueAfterFailure = false
```
이 설정은 테스트가 실패한 이후에 계속 할 것인지, 멈출 것인지에 대한 설정이다.  
true를 설정하면 실패하더라도 진행되며, false인 경우 실패하면 테스트가 멈추게된다.  

```tearDownWithError()```는 각 테스트가 실행되고 난 후에 불리는 함수다.

***
### Test Code
테스트 코드를 작성해보자.  
기본적으로 테스트 코드에서 UI를 작동하기 위해서 제공되는 것들이 있다.
```let app = XCUIApplication()```에서 app의 메소드를 보면 ```app.tables``` ```app.buttons``` ```app.navigationBars``` 등이 있다.  
이것을 이용하여 어떤 UI에 어떤 액션을 할지, 값을 넣을지 정하여 테스트를 하는 방식이다.  

간단한 테스트 함수를 통해 알아보자.
<img width="924" alt="스크린샷 2021-10-23 오후 10 26 51" src="https://user-images.githubusercontent.com/55477102/138559279-ac753ae0-76fc-456f-82f5-da36e36bc595.png">  
```Swift
func testMainTableView() {
    let app = XCUIApplication()
    app.launch()
  
    // 1
    app.tables.staticTexts["이동"].tap()
    // 2
    app.navigationBars["UIView"].buttons["Back"].tap()
}
```
너무 간단하지만... UITest를 하는 방법을 알려주기 위함이므로 부족하지는 않을 것 같다.  
코드에 주석으로 작성된 숫자가 코드 위의 스크린샷에 나오는 순서와 같고 해당 UI를 작동시키는 코드이다.  

![스크린샷 2021-10-24 오전 12 29 36](https://user-images.githubusercontent.com/55477102/138562521-68c340e8-d9bc-4d8c-b479-103a551ab5ce.png)  
위의 스크린샷 네모 박스에 있는 마름모에 마우스를 오버하면 재생 버튼이 활성화 된다.  
그 재생 버튼을 누르면 아래와 같이 자동으로 테스트가 진행되고 종료된다.  

https://user-images.githubusercontent.com/55477102/138562463-4e551e35-77cb-48f0-bbd1-d834cd75db71.mov
