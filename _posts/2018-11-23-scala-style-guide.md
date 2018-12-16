이 가이드는 Scala 공식문서의 내용을 작성자의 이해한 것으로 약간의 MSG가 첨가되어 있음을 알린다.

# [Scala naming convention](https://docs.scala-lang.org/style/naming-conventions.html)  
일반적으로 Scala는 [Camel case](https://en.wikipedia.org/wiki/Camel_case)를 사용한다. Camel case는 두 가지가 있다.  
1. UpperCamelCase : 각 단어의 첫 글자를 대문자로 시작한다.  
2. lowerCamelCase : 첫 단어는 모두 소문자로 하고, 나머지 단어는 첫 글자를 대문자로 한다.  

Underscore 즉, `_`는 scala에서 사용하는 키워드이므로 허용되지 않는다.  
## Classes/Traits  
UpperCamelCase로 명명한다.  
```scala
class MyClassDefinition
```

## Object  
classes/Traits와 동일하게 UpperCamelCase로 명명한다.  
```scala
object MyObjectDefinition
```
그러나 `object`를 package나 함수처럼 사용할 때는 예외로 할 수 있다.  
```scala
//mimickingPackage는 object이지만 다양한 정의가 존재하여 마치 package처럼 사용하는 경우
object mimickingPackage {
    sealed trait Expr
    case class neg(v: Number) extends Expr
    ...
}

//increase를 increase(3)과 같이 함수처럼 사용하려고 하는 경우
object increase {
    def apply(x: Int): Int = x + 1
}
```

## Packages  
scala `package`는 java와 동일하게 소문자만 허용되며 .(dot)를 통해 구분된다.  
`package`컨벤션은 java와 동일하다. scala의 `package` 역시 scala전체에 걸쳐 유일해야하며, 이를 domain 역순으로 함으로써 쉽게 구분된 패키지를 선언할 수 있다.
```scala
// wrong! 유일함을 보장할 수 없다.
package coolness

// right! 어플리케이션 도메인을 사용함으로써 유일함을 보장받을 수 있다.
package com.novell.coolness

// 이 package 선언은 바로 위 com.novell.coolness와 같으므로 이 방법 역시 유효하다.
package com.novell
package coolness

// right
package com.novell

/**
 * Provides classes related to coolness
 */
package object coolness {
    ...
}
```

`package`를 `import`할 때 `_root_`를 자주 사용하는 것은 장려되지 않는다.  

## Methods  
lowerCamelCase만 허용된다.  
```scala
def myDamnMethod = ...
```
### Accessors/Mutators  
Accessors는 흔히 getter로 알고 있는 데이터 접근자를 의미하며, Mutator는 흔히 setter로 알고 있는 데이터 변경자를 의미한다. Java에서는 lowerCamelCase로 보통 명명하며, `getBlah`, `setBlah`와 같이 get, set을 prefix로 붙이지만(boolean의 경우 is), scala는 이 규칙을 따르지 않고 아래와 같은 독창적인 규칙이 있다

1. Accessor는 property의 이름과 동일하다.
   ```scala
   class Foo {
       def bar(): String = ...//any expression to return value...
   }

   val foo = new Foo
   println(foo.bar)
   println(foo.bar())
   ```
   scala는 java와는 다르게 필드 이름과, 메소드 이름을 동일하게 할 수 없기때문에 사실 Accessor는 메소드가 아니고 그냥 public 필드에 접근하는 것과 같다. 즉 그냥 필드를 퍼블릭으로  

 2. Accessor가 



Accessors/Mutators
Scala does not follow the Java convention of prepending set/get to mutator and accessor methods (respectively). Instead, the following conventions are used:

For accessors of properties, the name of the method should be the name of the property.
In some instances, it is acceptable to prepend “`is`” on a boolean accessor (e.g. isEmpty). This should only be the case when no corresponding mutator is provided. Please note that the Lift convention of appending “_?” to boolean accessors is non-standard and not used outside of the Lift framework.
For mutators, the name of the method should be the name of the property with “_=” appended. As long as a corresponding accessor with that particular property name is defined on the enclosing type, this convention will enable a call-site mutation syntax which mirrors assignment. Note that this is not just a convention but a requirement of the language.

class Foo {

  def bar = ...

  def bar_=(bar: Bar) {
    ...
  }

  def isBaz = ...
}

val foo = new Foo
foo.bar             // accessor
foo.bar = bar2      // mutator
foo.isBaz           // boolean property
Unfortunately, these conventions fall afoul of the Java convention to name the private fields encapsulated by accessors and mutators according to the property they represent. For example:

public class Company {
    private String name;

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }
}
In Scala, there is no distinction between fields and methods. In fact, fields are completely named and controlled by the compiler. If we wanted to adopt the Java convention of bean getters/setters in Scala, this is a rather simple encoding:

class Company {
  private var _name: String = _

  def name = _name

  def name_=(name: String) {
    _name = name
  }
}
While Hungarian notation is terribly ugly, it does have the advantage of disambiguating the _name variable without cluttering the identifier. The underscore is in the prefix position rather than the suffix to avoid any danger of mistakenly typing name _ instead of name_. With heavy use of Scala’s type inference, such a mistake could potentially lead to a very confusing error.

Note that the Java getter/setter paradigm was often used to work around a lack of first class support for Properties and bindings. In Scala, there are libraries that support properties and bindings. The convention is to use an immutable reference to a property class that contains its own getter and setter. For example:

class Company {
  val string: Property[String] = Property("Initial Value")


  , 이는 Java와 scala의 차이에서 나온 것이라고 볼 수 있을 것 같다. 여기서 말하는 차이란, java에서는 아래와 같이 필드 이름과 메소드 이름을 같게 할 수 있지만, scala는 필드와 메소드를 구분하지 않기에 같은 이름으로 할 수 없다는 것이다.
```java
//java
class Foo {
    //It works!
    String bar
    String bar() {...}
}
```

```scala
//scala
class Foo {
    //Error!
    def bar
    def bar(): String = ...
}
```
그래서 scala는 변수의 이름은 `_varName`과 같이 prefix로 underscore를 붙이는 것이 관례다. 
