# 생성자
## Class의 선언
```scala
class Greeter {
    def SayHi() = println("Hello world!")
}
```
`class` 선언은 java와 동일하지만 메소드 선언이 좀 달라보인다.
메소드는 `def`로 정의하며 java와 다르게 마치 변수 할당처럼 `=`으로 body를 할당한다.(Java처럼 할당해도 문법 오류는 아니지만 지양한다) 

위 `class`는 너무 단순하다. 생성자도 없고... 생성자 있는 `class`선언은 어떻게 할까?
```scala
class Greeter(msg: String) {
    def SayHi() = println(msg)
}

new Greeter("Hey!")
```

여기서부터 혼란이 오기 시작한다. `class` 이름 옆에 바로 생성자 파라미터가 오는 것까지는 좋다.(개인적으로는 더 직관적이라고 생각한다), 변수 이름이 먼저 오고 타입이 나중에 오는 것은 그냥 받아들이도록 하자.  
근데 생성자에서 parameter로 받은 `msg`를 별다른 필드 할당 없이 어떻게 `SayHi`메소드에서 사용할 수 있지?라는 의문이 들 수 있다.  
또 생성자가 여럿이면 어떻게 하지?라는 의문이 들 수도 있으며, 생성시 처리할 특별한 로직은 어떻게 하지?라고 생각할 수도 있다.

#### 1. 생성자로 받은 parameter는 자동으로 필드가 된다.
엄밀하게 말하면 파라미터로 받은 값이 곧 필드라는 의미는 아니다. 무슨 말이냐면, 파라미터는 필드가 아니지만, 
1. Scala 컴파일러가 파라미터와 매칭되는 필드를 만들고(`_msg`)
2. 파라미터와 동일한 이름으로 Getter를 만든 후 (`public String msg()`)
3. 필드에 파라미터 값을 세팅해준다(`this._msg = msg`)는 의미다.(Compiler가 열일함...) 즉 위 코드는 아래의 java코드와 같다
```java
public class Greeter {
    public final String _msg;

    public Greeter(string msg) {
        this._msg = msg;
    }

    public String msg() {
        return this._msg
    } 
    ...
}
```
final인 이유는 기본적으로 파라미터 값은 `val`로 할당하기때문이다. 그럼 변수로 할당은 어떻게 할까? `var`를 붙이면 끝.
```scala
class Greeter(var message: String) {
    def SayHi() = println(message)
}

val abc = new Greeter("hello")
abc.message = "Good to see you"
```

다음으로 넘어가서, 생성시 특별한 로직을 넣고 싶을 때는 어떻게 할까?

#### 2. class body에 생성 로직을 넣을 수 있다.
```scala
class Greeter(message: String) {
    println("A greeter is being instantiated")
    
    def SayHi() = println(message)
}
```
Java가 익숙한 이들에게는 언뜻 보기에 메소드와 필드, 그리고 static 블록만 정의될 수 있는 java와는 다르게 로직이 `class` body에 섞인게 매우 어색해 보일 수 있다. 그러나 다르게 생각하면, 함수와 생성자가 똑같이 생긴 java보다는 더 직관적으로 보일 수 있다. `class`가 생성될 때 body를 한 번 훑고 지나간다는 의미에서 생성 로직이 실행되는 것이다. 위 `println`으로 시작되는 코드 블럭은 `new`를 통해 인스턴스화될 때 실행된다.

자 이제 마지막으로, 생성자를 여러개 정의하고 싶으면 어떡하지?라는 의문만 남았다. 여기서 잠시 두 가지 단어만 알아두자.
1. Primary constructor : 기본 생성자
2. Auxiliary constructor(또는 secondary constructor) : 기본 생성자 이외의 생성자

#### 3. 두 번째 생성자는 class 정의 블록 내에 선언하며 몇 가지 반드시 지켜야하는 규칙이 있다.
기본 생성자를 제외한 나머지 생성자는 `this`라는 메소드를 정의하듯 하면 된다.
```scala
class Greeter(message: String, secondaryMessage: String) {
    def this(message: String) = this(message, "")
    
    def SayHi() = println(message + secondaryMessage)
}
```
두 번째 줄의 `this`가 두 번째 생성자이다. 별로 어려운 건 없어 보인다. 

그런데 이 생성자를 정의하는 규칙이 Java보단 까다롭다. Java에서는 생성자가 다른 생성자를 호출할 수도 있고, 안 할 수도 있고 자유로웠지만, Scala에서는 그렇지 않다. 기본 생성자를 제외한 모든 생성자는 먼저 정의된 다른 생성자를 호출하여야 한다. 이 말은 결국 모든 생성자가 직/간접적으로 기본 생성자를 호출한다는 의미다. 또한 타 생성자 호출은 생성자 body의 첫 줄에 위치해야 한다. 두 줄 정리해보자.
1. 모든 생성자는 Overloading을 통해 **먼저 정의된 타 생성자 중 1개는 반드시 호출**해야한다.
2. 타 생성자 호출은 **생성자 첫 줄**에서 진행되어야 한다.


그럼 임의로 에러코드를 한 번 만들어보자
```scala
class Greeter(message: String) {
    def this(message: String, secondaryMessage: String) {
      println("abc") //error! 'this expected'
      new Greeter("")
    }

    def SayHi() = println(message)
  }
```
여기서 알 수 있는건 보조 생성자는 무조건 첫 줄을 `this`로 시작해야한다는 것이다.

임의로 에러코드를 만들어봤다. IntelliJ 2018.3 버전에서는 에디터 상에서는 에러가 없다. 그러나 코드를 빌드하면 컴파일 에러가 발생한다.
```scala
class Greeter(message: String) {
    def this(num: Int) = this("abc", "cda")
    def this(message: String, secondaryMessage: String) = this(3)

    def SayHi() = println(message)
  }
```
위 코드는 기본 생성자를 참조하지 않고 보조 생성자끼리 호출하게 했다. 보조 생성자의 첫 줄은 무조건 `this`이기때문에 순환호출이 될 수 밖에 없는 구조다.
이 코드의 에러 내용은 다음과 같다.
```
called constructor's definition must precede calling constructor's definition
```
먼저 정의된 생성자를 호출해야하는데 그렇지 않은 상황이라는 말이다.

마지막 하나, 그럼 보조 생성자 블럭에서 this를 두 번 이상 호출하면 어떨까?
이 때는 두 가지 케이스가 있다.
두 번째 this는 생성자 호출이 아닌 `apply`메소드 호출이다. 그렇기 때문에 apply 메소드가 제대로 정의되어 있을 경우 문제 없으나, 그렇지 않은 경우 에러가 난다.

1. `apply` method가 있는 경우
```scala
class Greeter(message: String) {
    def this(num: Int) = this("abc")
    def this(message: String, secondaryMessage: String) {
      this(3)
      this(5) //Int를 파라미터로 받는 apply method가 없어 에러가 난다.
    }

    def SayHi() = println(message)
  }
```

2. `apply` method가 없는 경우
```scala
class Greeter(message: String) {
    def this(num: Int) = this("abc")
    def this(message: String, secondaryMessage: String) = {
      this(3) //이건 new Greeter(3)과 같음
      this(5) //이건 new Greeter(5)가 아닌 this.apply(5)다
    }

    def apply(num: Int) = println("erwe")

    def SayHi() = println(message)
}
```
이번엔 apply method를 정의했고 this를 호출했다. 적합한 apply가 있기때문에 에러가 나지 않는다.

#### Bonus) 생성자의 기본 값은 지정
```scala
class Foo(num: Int = 0, str:String = "") {
    ...
}
```
참 쉽죠?

#### Bonus) 안전하게 생성자 파라미터에 바인딩하기
파라미터가 많다면 순서가 헷갈릴 수 있다. 가령 유리수를 표현하는 class가 있다고 하고, 첫 번째 인자가 분자, 두 번째 인자가 분모라고 하자.
```scala
class Rational(nom: Int, denom: Int) {
    ...
}
```
개발자는 3/4을 표현하고 싶었지만 생성자에 순서를 잘못 넣어 4/3을 만들어버렸다.
```scala
new Rational(4, 3) //Logical Error not Compile error
```

하지만 우리의 짱짱맨 scala는 명시적으로 파라미터 이름을 지정할 수 있다.
```scala
//모두 같은 값의 유리수를 표현한다.
new Rational(denom = 4, nom = 3)
new Rational(nom = 3, denom = 4)
new Rational(3, 4)
```



# Reference
- [Joel Abrahamsson's article](http://joelabrahamsson.com/)
- [Primary constructor](https://blog.knoldus.com/constructor-in-scala/)
- [Auxiliary constructor](https://dzone.com/articles/auxiliary-constructor-in-scala)

# Index
## [Previous](./2018-11-28-var-definition.md) | [Next](./2018-11-28-param-field.md)