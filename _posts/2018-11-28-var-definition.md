# 변수의 정의

```scala
val greeter = "Hello"
```
별로 특별할 것 없는 코드다. Scala에서 변수 선언은 `val`과 `var`로 한다. 둘의 차이는 reference를 바꿀 수 있느냐, 없느냐의 차이다.
```scala
var num = "Hello"
num = "Bye" //OK

val other = "Hello"
other = "Bye" //Error!
```
`val`은 java에서 final이 붙은 변수라고 생각하면 된다.

흔히 java에서처럼 반복문을 하려면 `var`를 사용해야한다. `val`은 반복자인 `i`를 바꿀 수 없기때문이다.
```scala
//OK
var i = 0
while(i < 10) {
    ...
    i += 1
}

//Error
val j = 0
while (j < 10) {
    ...
    j += 1
}
```

# Reference
- [Joel Abrahamsson's article for learning scala](http://joelabrahamsson.com/)

# Index
## [Previous](./2018-11-28-hello-world.md) | [Next](./2018-11-28-constructor.md)