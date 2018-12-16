# Method vs function in scala

## Method  
Scala에서 `method`는 Java에서처럼 `class`의 한 부분을 담당한다. 이름과 시그니처를 가지고 있으며 선택적으로 `annotation`을 가질 수도 있다.

## Function  
`Method`와는 다르게 `function`도 object이다. Scala에는 `argument` 수에 따라 대표되는 `function` `trait`들이 있다. `Function0`, `Function1` 등이 그것이다. 이 `function` `trait`들의 구현체는 `apply`와 같은 `method`를 가지고 있다. 요약하면 `Function`도 내부적으로 `method`를 가지고 있는 `object`이다.

## Method vs Function  
아래의 코드를 보자. 어느 것이 `method`이고 어느 것이 `function`일까?
```scala
//test.scala
class test {
    def m1(x:Int) = x+3
    val f1 = (x:Int) => x+3
}
```
정답은 `m1`이 `method`이고 `f1`이 `function`이다. 이 file을 compile하면 두 `class`가 생긴다. 하나는 `test.class`이고, 다른 하나는 `test$$annofun$1.class`이다. 후자가 `f1`이다.

`javap`로 test.scala를 돌려보면 두 `class`가 어떻게 구현되었는지 알 수 있는데 그 내용은 다음과 같다.
```
Compiled from "test.scala"
public class test extends java.lang.Object implements scala.ScalaObject{
    public test();
    public scala.Function1 f1();
    public int m1(int);
    public int $tag()       throws java.rmi.RemoteException;
}

public final class test$$anonfun$1 extends java.lang.Object implements scala.Function1,scala.ScalaObject{
    public test$$anonfun$1(test);
    public final java.lang.Object apply(java.lang.Object);
    public final int apply(int);
    public int $tag()       throws java.rmi.RemoteException;
    public scala.Function1 andThen(scala.Function1);
    public scala.Function1 compose(scala.Function1);
    public java.lang.String toString();
}
```
위 결과에서 알 수 있는 점은, `m1`은 test의 member로 최종적인 값인데 반해, `f1`은 compile되면서 별도의 `Object`, `ScalaObject`, `Function` `class`/`trait`의 구현체가 된 `object`이며, `apply`, toString을 비롯한 여러 `method`를 가지고 있다는 사실이다.  

좀 더 심화과정으로 넘어가서 이번엔 `method`를 변수에 넣으면 어떻게 될지 확인해보자  
```
class test {
    def m1(x:Int) = x+3
    val f1 = (x:Int) => x+3
    val f2 = m1 _
}
```
작성자도 아직 scala가 익숙하지 않아서 그런지 `f2`와 같은 선언을 볼 때마다 화가 난다.

## Reference  
- [Jim McBeath's blog](http://jim-mcbeath.blogspot.com/2009/05/scala-functions-vs-methods.html)
- [Difference between method and function in Scala](https://stackoverflow.com/questions/2529184/difference-between-method-and-function-in-scala)