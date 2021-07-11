# escaping과 nonescaping의 차이
@escaping 어노테이션을 쓰고 안쓰고으 차이는 간단하게 closure값의 전달이 비동기로 전달되는지에 따라 달라진다.

### ex)1
``` Swift
func escapingTest(completion: (String) -> ()) {
  completion("1") // A
}

escapingTest { str in
  print(str)
}

// 출력
// 1
```
위의 코드에서 볼점은 'A' 위치다.
completion으로 1이라는 텍스트를 전달하게 된다.
현재 코드에서는 escaping 어노테이션을 쓸 이유가 없다.
해당 값을 전달함에 있어 비동기로 작동 할 이유가 전혀 없기 때문이다.

### ex)2
``` Swift
func escapingTest(completion: (String) -> ()) {
  // A
  DispatchQueue.main.async {
    completion("1") // A
  }
  //
}

escapingTest { str in
  print(str)
}
```
만일 위처럼 코드가 변경되었다면 결과는 어떻게 될까?
답은 오류가 발생하게 된다.

"Escaping closure captures non-escaping parameter 'completion'"
오류의 전문이다.

closure의 발생이 비동기이기 때문이다.
``` Swift
func escapingTest(completion: @escaping (String) -> ()) {
  DispatchQueue.main.async {
    completion("1")
  }
}
```
그렇게 때문에 이처럼 메소드 또는 함수를 선언 할 때 closure에 escaping 어노테이션을 사용해야만 하는 것이다.
조금 복잡 할 수 있지만, 정말 간단하게 편한대로만 생각해보면 단순히 비동기를 비동기로 대응하는 것이다.
