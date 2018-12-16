# Scala에서 Main method를 정의하는 두 가지 방법

## main method를 object안에 두자
```scala
object MyApp {
    def main(args: Array[String]): Unit {
        ...
    }
}
```
일반적인 방법이다. Singleton object안에 main method의 시그니처를 명시해주면 된다.

그렇다면 꼭 `object`로만 선언해야할까? `class`로하면 안될까?

그렇다. 반드시 `object`로 선언해야한다. 왜일까?

관련 주제로 찾아보면, 다음과 같이 정리될 수 있다.

Java의 `static`이 `object`의 메소드에 대응된다.
Java의 method 시그니처는 `public static void main`이다. 이에 1:1대응되는 코드가 scala의 `object`의 `main` method라는 것이다. 오해하지 말아야할게 scala의 object의 method가 static은 아니다. 단지, object의 main method를 바이트 코드로 컴파일 했을 때 java의 static method로 컴파일된다는 의미다.

그러나 작성자가 생각하기엔, 단순히 이 이유만은 아닌 것 같다.(틀렸다는 말이 아니라 다른 이유가 더 있을 것이라는 말임)

stackoverflow를 찾아보며 내가 내린 결론은 다음과 같다.
사실 main method의 시그니처는 C부터 내려온 일종의 컨벤션이라고 할 수 있다. return하는 값이 없고, 문자열을 인자로 받아 실행하며, 프로그램이 시작되는 위치의 시크니처가 컨벤션화된 것이다. Scala역시 이 컨벤션을 따른다.  
또한 Scala는 Full Object oriented language를 지향하는데, [static이라는 java의 개념은 object oriented의 개념에 위배된다.](https://stackoverflow.com/questions/22890024/why-does-having-static-members-make-a-language-less-object-orientated) scala는 이러한 이유로 static이 아닌 Singleton object에 main method를 바인딩하는 방법을 채택했다. 

## App trait을 상속받자
또다른 방법으로는 `App` trait을 상속받는 방법이 있다.

```scala
object test extends App {
    ...statement...
    //you can access arguments using args
    println(args(0))

    //or override main method. but overriding main method is deprecated in Scala 2.11.0
    override def main(args: Array[String]): Unit = {
      ...
    }
}
```

`main` method를 override해도 되고 바로 로직을 작성해도 된다.(하지만 이 방법은 2.11부터 deprecated이다) 

`main` method를 재정의하지 않을 경우 argument는 `args`라는 미리 정의된 object를 통해 접근할 수 있다. App trait을 까보면, `args`는 App trait의 main method에서 초기화된다.

근데 method로 정해놓은 방법만 있어도 충분할텐데 이 방법은 왜 쓰는걸까?

