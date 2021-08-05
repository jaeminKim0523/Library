#QuickHelp
QuickHelp는 사용자가 선언한 변수, 상수, class, struct, protocol 등등  
거의 모든 곳에 작성된 주석을 보여주는 기능입니다.

![quickhelper1](https://user-images.githubusercontent.com/55477102/128331926-e53e2bbf-3f99-4009-870c-797b764477ae.png)  
위의 스크린샷처럼 개발을 하시면서 우측에 보이는 설명이 Quick Help 입니다.  

```Swift
// MARK: - TestFunc
/**
     [홈페이지](https://github.com/jaeminKim0523/)
     
     xCode의 Quick Help를 설명하기 위한 예제 함수입니다.

     # 기능
     `name` 파라메터 값이 *false* (비어있지 않다)면,
     **Hello World**를 출력합니다.
     만약 비어있다면 에러를 반환합니다.
     ```
     // 에러 반환
     throw FinupError.Failure
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
            throw FinupError.AutoLoginFailure(className: "TestClass", funcName: "TestFunc")
        }
        
        // FIXME: Hello World -> Hello Jaemin
        print("Hello World")
        
        return true
    }
```
위와 같이 작성하면,
