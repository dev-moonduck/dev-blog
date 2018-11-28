# Hello world

여기서는 Compiled Scala code 작성을 위주로 다룰 것이다.

먼저 Hello world 구문을 보자
```scala
object Greeting {
    def main(args: Array[String]) = println("Hello world!")
}
```
Java의 main 메소드와 비슷하게 생겼지만 몇 가지 다른 점이 눈에 띈다. 하나씩 살펴보자.

## No static method
Java에서는 아래와 같이 main method 시그니처가 정해져있다.
```java
public static void main(String[] args) {...}
```
`static`은 java의 특별한 키워드인데, Scala에서는 `static` 개념이 없다. Scala는 모든 것이 `object`에 정의되어 있다. Java도 그렇다고 하지만 `static`은 객체와는 별도로 움직임으로써, OOP를 해치는 요소로 지목받아왔다. Scala의 `object`는 언어 레벨에서 보장하는 싱글톤 객체이다. 즉 언어레벨에서 정의된 형태로 인스턴화 해주며, **java의 main method는 static인 것과는 다르게 scala는 인스턴스 method라는 점**이 차이점이다.(그러나 scala 코드를 컴파일하면 scala main method 역시 java main method로 변환된다)

## No return type
엄밀히 말하면 Scala의 모든 메소드는 값을 리턴한다. 단지 Scala compiler가 리턴 타입을 추론하기때문에 명시하지 않아도 되는 것일뿐이다. 스펙상으로 **scala의 main method는 `Unit`을 리턴**한다. **`Unit`은 Java의 void와 같이 아무것도 없다는 의미**이다. 컴파일러가 추론한 타입에 따라 다시 작성한 scala의 main method는 아래와 같다.
```scala
object Greeting {
    def main(args: Array[String]): Unit = println("Hello world!")
}
```

## No access level modifier
Java에서는 `public`이라는 접근제한자가 붙어있고 default가 package scope지만, scala는 `public`이다. 따로 명시하지 않으면 다 public이라는 소리다. 명심하자

## 기타
함수 정의 키워드인 `def`가 별도로 있다는 점도 확인할 수 있을 것이다.

# Reference
- [Joel Abrahamsson's article for learning scala](http://joelabrahamsson.com/)

# Index
## [Next](./2018-11-28-var-definition.md)