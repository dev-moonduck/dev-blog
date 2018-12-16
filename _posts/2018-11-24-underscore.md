# 멘붕의 underscore in Scala
Scala를 처음 접한이들에게 가장 멘붕오게 하는 것은 광범위하게 쓰이는 underscore이다. 작성자 역시 Scala를 공부하던 중 계속 튀어나와서 코드를 난해하게 하는 이 underscore의 쓰임을 보고 멘붕하여 이 Post를 작성하게 되었다.

# Underscore의 용례
#### Default value for variable
```scala
class Foo {
    var bar:String = _
}
```
 변수의 default값으로 초기화 할 경우에 사용할 수 있다. Type에 따라서 default값은 달라질 수 있다. `Object`인 경우는 `null`이 될 것이며, `Int`인 경우는 0, `Boolean`의 경우는 `false`가 될 것이다.  

 그렇다면 다음 몇 가지에 대해 어떻게 될지 생각해보자.
 1. `val`에 underscore 할당이 가능한가?
 2. 타입 지정이 명시적으로 지정되지 않은 경우엔 어떻게 될까?

 정답은 둘 다 Error다.

 ```scala
class Foo {
    val bar1:String = _ //Error, 변수에만 할당이 가능하다.
    
    var bar2 = _ //Error, Default value를 지정해야 하기때문에 명시적인 타입 지정이 필요하다.
}
```

#### Existential types
```scala
def foo(l: List[Option[_]]) = ...
```

#### Higher kinded type parameters
```scala
case class A[K[_],T](a: K[T])
```

#### Ignored variables
```scala
val _ = 5
```

#### Ignored parameters
```scala
List(1, 2, 3) foreach { _ => println("Hi") }
```

#### Ignored names of self types
```scala
trait MySeq { _: Seq[_] => }
```

#### Wildcard patterns
```scala
Some(5) match { case Some(_) => println("Yes") }
```

#### Wildcard imports
```scala
import java.util._
```

#### Hiding imports
```scala
import java.util.{ArrayList => _, _}
```

#### Joining letters to punctuation
```scala
def bang_!(x: Int) = 5
```

#### Assignment operators
```scala
def foo_=(x: Int) { ... }
```

#### Placeholder syntax
```scala
List(1, 2, 3) map (_ + 2)
```

#### Partially applied functions
```scala
List(1, 2, 3) foreach println _
```

#### Converting call-by-name parameters to functions
```scala
def toFunction(callByName: => Int): () => Int = 
```
callByName _


# Reference
- "[What are all the uses of an underscore in Scala?](https://stackoverflow.com/questions/8000903/what-are-all-the-uses-of-an-underscore-in-scala)" in stackoverflow
- [Scala dreaded underscore
](https://www.slideshare.net/normation/scala-dreaded) in slideshare