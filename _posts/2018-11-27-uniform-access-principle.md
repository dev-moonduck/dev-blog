# Uniform access principle in Scala
타 객체 지향 언어(e.g Java)를 공부하다가 Scala로 넘어온 사람에게는 당황스럽고 이해하기 힘든 부분이 많은데, 그 중에서 이번에는 변수처럼 정의하는 함수, 메소드, 변수였다가 함수였다가 헷갈리는 표현의 배경에 있는 Uniform access principle을 다뤄본다.

## Uniform Access Principle은 또 뭐냐?
Bertrand Meyer가 *Object-Oriented Software Construction*에서 처음 명시한 개념인데, 아래와 같다.
```
All services offered by a module should be available through a uniform notation, which does not betray whether they are implemented through storage or through computation.
```
구현된 서비스는 저장소로 되었든 로직상이든 동일한 표현 방법으로 사용이 가능해야한다는 의미다. 좀더 실용적인 의미로 설명하자면, 클래스의 기능을 이용하는 방법은 속성이든 메소드든 다르게 해서는 안된다는 의미다.(사실 그래도 뭔 말인지 모르겠다)  
예를 들면, Java에서 메소드를 호출하는 것과, 변수에 접근하는 방법은 다르다.
```java
class Foo {
    public int bar
    public int bar() {...}
    ...
}
new Foo().bar
new Foo().bar() //method는 괄호를 붙여주어야 함
```

따라서 불행하게도 Java나 C#은 이 원리를 지키지 않는다.

그러나 Scala는 그렇지 않는다는 말씀. Scala는 Uniform Access Principle을 지향한다.

## val과 var에 어떻게 접근할까?
아래의 간단한 Person class를 보자  
```scala
class Person {
    val name = "Johh Doe"
}
```

# Reference
- [Joelabrahamsson의 블로그](http://joelabrahamsson.com/learning-scala-part-nine-uniform-access/)