# Scala의 Syntactic sugar
이 문서에서는 Scala에서 쓰는 구문을 더 간결하게(가끔은 나쁠 정도로) 사용하는 방법을 다룬다.

#### List를 선언하는 여러 방법
```scala
List(1, 2, 3)
1 :: 2 :: 3 :: Nil
```

#### 
- HOF(Higher Order Function)의 경우 파라미터가 1개일 경우 함수 이름만 명시