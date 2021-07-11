# escaping과 nonescaping의 차이
@escaping 어노테이션을 쓰고 안쓰고으 차이는 간단하게 closure값의 전달이 비동기를 통하는지에 따른다.
간단한 코드를 보면,

``` Swift
func escapingTest(completion: (String) -> ()) {
  completion("1")
}

escapingTest { str in
  print(str)
}
```

위으 코드에서 볼점은 
