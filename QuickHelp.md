# QuickHelp
### 목차  
1. Quick Help란?  
2. Quick Help 결과물  
3. Quick Help 결과물 코드  
4. Quick Help 설명

## Quick Help란?
QuickHelp는 사용자가 선언한 변수, 상수, class, struct, protocol 등등 거의 모든 곳에 작성된 주석을 보여주는 기능이다.  

![quickhelper1](https://user-images.githubusercontent.com/55477102/128331926-e53e2bbf-3f99-4009-870c-797b764477ae.png)  
위의 스크린샷처럼 주석이 작성된 곳을 클릭하면 Inspactor(Xcode 우측 메뉴)의 (?)메뉴에서 보이는 설명들이 Quick Help다.  

Quick Help는 Github의 .md파일 처럼 Markdown을 사용하여 주석을 꾸밀 수 있다.

바로 본론으로 들어가서 아래와 같은 결과물을 만들어 볼 것이다.  
### Quick Help 결과
![스크린샷 2021-08-05 오후 8 23 48](https://user-images.githubusercontent.com/55477102/128342171-49d9f6bd-0daf-4cef-ac20-fbbbdd413c37.png)  

### Quick Help 결과물 코드
```Swift
// MARK: - TestFunc
/**
     [홈페이지](https://github.com/jaeminKim0523/)
     
     두번째 줄은 항상 Discussion 주석 입니다.

     # 기능
     `name` 파라메터 값이 *false* (비어있지 않다)면,
     **Hello World**를 출력합니다.
     만약 비어있다면 에러를 반환합니다.
     ```
     // 에러 반환
     throw MyError.Failure
     ```
     
     # 함수 흐름
        1. **name**의 값이 비어있는 문자열인지 검사한다.
            - 비어있다면 오류를 반환하고 함수를 종료한다.
        2. 비어있지 않다면 **Hello World**를 출력한다.
        3. *true*를 반환하고 함수를 종료한다.
     
     - parameters:
        - name: 이름
        - age: 나이
     - returns:
        - Bool: 논리값
     - throws:
        - **name**의 문자열 값이 빈값일 때 오류 반환
     - Author: Kim jaemin
     - Since: 2020-12-04
     - ToDo: age값에 따른 예외 처리 필요
     - Copyright: [jaeminGithub](https://github.com/jaeminKim0523)

*/
    func testFunc(name: String, age: Int) throws -> Bool {
        // TODO: 나이에 대한 예외처리 필요
        guard name.isEmpty == false else {
            throw MyError.AutoLoginFailure(className: "TestClass", funcName: "TestFunc")
        }
        
        // FIXME: Hello World -> Hello Jaemin
        print("Hello World")
        
        return true
    }
```

### Quick Help 설명
```Swift
// 일반적으로 알고 있는 주석이며, Quick help에 나타나지 않는 주석이다.
var test: String = "Jaeminkim0523"
```
```Swift
/**
    여러 줄 주석을 사용 할 때 사용한다.
    (Quick Help에 표시된다.)
*/
var test: String = "Jaeminkim0523"
```
```Swift
/// 한줄씩 표현하고자 할때 사용한다. (Quick Help에 표시된다.)
var test: String = "Jaeminkim0523"
```  
위의 예시처럼 기본적으로 Swift에서는 주석으로 작성되는 방식이 3가지가 있다.  
첫번째로 Quick Help에서는 보이지 않지만, Minimap에서 보여지거나 코드상에 표시하기 위한 주석을 알아 볼 것이다.  
<img width="136" alt="스크린샷 2021-08-05 오후 7 28 57" src="https://user-images.githubusercontent.com/55477102/128335669-8b77a9bf-8b0c-40e1-8156-be4773550dfc.png">  
Minimap이란, 위와 같이 Xcode에서 코드와 Inspactor의 사이에 보여지는 해당 파일의 지도이다.  
만일 Minimap이 없는 사람은 아래와 같은 메뉴에서 Minimap 체크를 하면 보여질 것이다.  
<img width="256" alt="스크린샷 2021-08-05 오후 7 29 23" src="https://user-images.githubusercontent.com/55477102/128335713-94ad3245-9287-4b29-97a6-ed402c09ccde.png">  

***
#### 첫번째로 3가지 Minimap 주석을 알아보자.  
1. MARK
<img width="641" alt="스크린샷 2021-08-05 오후 7 35 50" src="https://user-images.githubusercontent.com/55477102/128336630-04b21687-7eb3-4ea8-9597-49c2c2729977.png">  
MARK 주석은 말 그대로 표시를 할 때 사용하는 주석이다.  
Minimap상에서 해당 주석의 콜론( : ) 뒤에 작성된 글이 보이는 것을 확인 할 수 있다.  
작성법: "// MARK: 주석" 이다.  

2. TODO
<img width="636" alt="스크린샷 2021-08-05 오후 7 36 04" src="https://user-images.githubusercontent.com/55477102/128337060-3eb38885-596d-4ba9-a761-e4dd6cf12e80.png">  
TODO 주석은 추후에 추가적으로 해야 할 작업이 있다는 것을 표시하기 위한 주석이다.  
Minimap상에서 글자는 표시되지 않지만, 스크린샷을 보면 얇은 회색 실선으로 표시된다.  
작성법: "// TODO: 주석" 이다.  

3. FIXME  
<img width="638" alt="스크린샷 2021-08-05 오후 7 36 20" src="https://user-images.githubusercontent.com/55477102/128337293-9fbac304-b46f-40af-86c7-bb9a58ad3ce6.png">  
FIXME 주석은 수정해야 할 곳을 표시하는 주석이다.  
TODO주석과 같이 Minimap상에 글자가 표시되지 않지만 동일하게 얇은 회색 실선이 표시된다.  
작성법: "// FIXME: 주석" 이다.  

***  

#### 두번째로 Quick Help에서 주석을 보여주고 꾸밀 수 있는 Markdown을 알아보자.  
1. Summary 주석
<img width="199" alt="스크린샷 2021-08-05 오후 7 51 53" src="https://user-images.githubusercontent.com/55477102/128338523-175f9302-a468-4c4a-89cb-5238ec1f1cc9.png">  
```Swift
/**
  주석의 첫줄은 Summary에 작성됩니다.
*/
```

******  
2. Discussion 주석
<img width="210" alt="스크린샷 2021-08-05 오후 7 55 11" src="https://user-images.githubusercontent.com/55477102/128338920-9a031451-8936-43db-90cc-3ff0ea099de2.png">  
```Swift
/**
  주석의 첫줄은 Summary에 작성됩니다.

  두번째 줄은 항상 Discussion 주석입니다.
*/
```

***  
3. 기울기 텍스트
<img width="94" alt="스크린샷 2021-08-05 오후 8 02 49" src="https://user-images.githubusercontent.com/55477102/128340364-d66e95a9-ad38-4010-9857-542395af19be.png">  
```Swift
/**
  기울기 *텍스트* 작성
*/
```

***  
4. 볼드(굵은) 텍스트
<img width="83" alt="스크린샷 2021-08-05 오후 8 02 36" src="https://user-images.githubusercontent.com/55477102/128340365-bad3ed5d-7ccc-46f0-93bc-8bb0139b3d64.png">  
```Swift
/**
  볼드 **텍스트** 작성
*/
```

***  
5. 순서 없는 목차
<img width="62" alt="스크린샷 2021-08-05 오후 8 03 35" src="https://user-images.githubusercontent.com/55477102/128340363-69ff3211-b67f-4ef6-86d4-cbc16b636a72.png">
```Swift
/**
  * 목차1
  * 목차2
  * 목차3
*/
```

***  
6. 순서 있는 목차
<img width="54" alt="스크린샷 2021-08-05 오후 8 03 56" src="https://user-images.githubusercontent.com/55477102/128340360-d8d01546-47fb-4a15-a491-1b3dae98e1ba.png">  
```Swift
/**
  1. 목차1
  2. 목차2
  3. 목차3
*/
```

***  
7. 코드 작성
<img width="408" alt="스크린샷 2021-08-05 오후 8 05 34" src="https://user-images.githubusercontent.com/55477102/128340358-40d1955c-be4f-42ab-8e80-d7a5d97c0e90.png">  
~~~Swift
/**
  ```
    let name: String = "Kim jaemin"
    let jop: String = "iOS developer"
  ```
*/
~~~

***  
8. Parameters(인자값)
<img width="84" alt="스크린샷 2021-08-05 오후 8 05 55" src="https://user-images.githubusercontent.com/55477102/128340357-76291bc8-f463-46c4-af22-55b4fdca5b19.png">
```Swift
/**
  - parameters:
    - name: 이름
    - age: 나이
*/
```

***  
9. Author(작성자))
<img width="73" alt="스크린샷 2021-08-05 오후 8 06 02" src="https://user-images.githubusercontent.com/55477102/128340355-288f4dc7-a1bc-44d4-81ff-fad89b613870.png">  
```Swift
/**
  - Author: Kim jaemin
*/
```

***  
10. Since(작성일)
<img width="82" alt="스크린샷 2021-08-05 오후 8 06 05" src="https://user-images.githubusercontent.com/55477102/128340352-0275b411-416a-4509-8a56-f1cd82581587.png">  
```Swift
/**
  - Since: 2020-12-04
*/
```

***  
11. ToDo (해야 할 것 메모)
<img width="137" alt="스크린샷 2021-08-05 오후 8 06 08" src="https://user-images.githubusercontent.com/55477102/128340349-6d18618e-9cd2-4471-886f-a535bc74834b.png">
```Swift
/**
  - ToDo: age값에 따른 예외 처리 필요
*/
```

***  
12. Copyright (저작권) + URL Link
<img width="88" alt="스크린샷 2021-08-05 오후 8 06 11" src="https://user-images.githubusercontent.com/55477102/128340347-16faa1c9-d97e-499e-8f22-979fada5829d.png">
```Swift
/**
  - Copyright: [jaeminGithub](https://github.com/jaeminKim0523)
*/
```

***  
13. Throws
<img width="219" alt="스크린샷 2021-08-05 오후 8 06 16" src="https://user-images.githubusercontent.com/55477102/128340343-d26d119c-182b-4d33-89a8-dcc12f00319d.png">
```Swift
/**
  - throws:
    - **name**의 문자열 값이 빈값일 때 오류 반환
*/
```

***  
14. Return
<img width="97" alt="스크린샷 2021-08-05 오후 8 06 18" src="https://user-images.githubusercontent.com/55477102/128340328-4fbc142b-4d62-4575-8106-d4185524ab5f.png">  
```Swift
/**
  - returns:
    - Bool: 논리값
*/
```
***  
