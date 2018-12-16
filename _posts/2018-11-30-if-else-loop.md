# 조건문과 반복문
로직의 기본 토대를 이루는 것 중 하나가 조건문과 반복문이다. 이를 통해 우리는 프로그램의 흐름을 제어할 수 있다. Scala의 조건문과 반복문은 Java와 유사하지만 다른 점이 좀 있다.

## 조건문
Scala의 조건문은 Java등 다른 언어와 크게 다르지 않다.
```scala
if (condition)
    ...
else if (condition)
    ...
else
    ...
```

### Scala의 if-else는 expression이다.
반면 Java의 if-else는 statement이다. statement는 구문을 서술한 것이고, expression은 값을 return한다. 
```java
//Java의 if-else 구조
if (conditionA) {
    return a;
} else {
    return b;
}
```
위의 Java 조건문은 conditionA가 참일 경우 a를 return하라는 흐름을 탄다. 그렇지 않을 경우 else 블럭에 있는 흐름을 탄다. 이를 expression으로 바꾸면 다음과 같다.
```java
return conditionA ? a : b
```

Scala에서는 `if-else`가 java의 삼항연산자 `?:`에 더 가깝다고 할 수 있다.
```scala
if (conditionA) {
    a
} else {
    b
}
```
condition이면 a를, 그렇지 않으면 b다!라는 느낌을 얻어야한다. Scala의 조건문은 Java의 if-else와 똑같지만, 성격은 삼항연산자와 같다고 볼 수 있다.

### Statement, expression... 그게 중요해?
아래의 Java 코드와 Scala 코드를 비교해보자
```scala
//Scala code
val cond = true
val value = if (cond) {
    "it is true"
} else {
    "unfortunately it is not true"
}
```
```java
//Java code
val cond = true
String value = null;
if (cond) {
    value = "it is true"
} else {
    value = "unfortunately it is not true"
}
```
값을 statement안에서 할당해야하는 Java와는 달리 Scala가 더 깔끔하고 직관적으로 떨어지는 것을 알 수 있다.
자세한 부분은 아래 Reference의 stackoverflow 질문글을 보며 이해하길 권장한다.

## 반복문
Scala의 반복문은 다양하게 구현될 수 있다.
### 전통적인 방법
#### for
```scala
//for-each, type은 생략이 가능하다
for (l:SomeType <- list) {
    ...
}
```
#### while
```scala
var mutation = 0
while (mutation < 10) {
    ...
    mutation += 1
}
```
#### do-while
```scala
var mutation = 0
do {
    ...
    mutation += 1
} while(mutation < 10); //Semicolon이 있어야 한다.
```
java의 for-each와 다를 바 없다.
근데 index로 접근하는건 어떻게 하죠?

그런데 가능하면 이렇게 loop를 돌리지 말자. 보다 scala스럽게, 함수형스럽게 접근하는 법을 보자.

### 원초적인 반복문 재귀
재귀는 어떤 사람들에겐 어렵지만, 이게 가장 기계적이고 원초적이며, 때로는 재귀가 더 이해하기 쉽고 간결할 때가 있다.
```scala
def someMethod(i: Int) = {
    if (i < 0) ...
    else someMethod(i - 1)
}
```
그냥 for 쓸래...라는 생각이 든다면

### Scala의 for
사실 Scala의 `for`문은 statement가 아니라 expression이므로 `for`식이 맞다. Scala community에서는 *for comprehension* 혹은 *for expression*이라고 한다. 이 `for`식의 생김새는 다음과 같다.
```
for(elementName <- generator1; generator2... filter) yield yieldExpr
```
Java와는 다르게 뭔가 많아 보인다. 하나씩 살펴보자

1. 요소(elementName)와 Generator
위의 elementName과 generator만으로도 `for`문을 구성할 수 있는데 그렇게 되면 java의 for-each구문과 매우 흡사하다
```java
//java
for (Object o : objects) {...}
```
```scala
//scala
for (o <- objects) {...}
```
단지 Scala에서는 타입을 명시할 필요가 없다는 점과 colon이 arrow로 바뀐게 다른 점이다. Scala Compiler가 추론해준다. 여기서 잠깐, o는 `var`일까 `val`일까?
정답은 `val`, o의 값을 바꾸는 멍청한 로직은 짜지 않길 바란다.(어차피 compile error다) 굳이 타입 명시를 해도 문제가 되진 않는다.
```scala
for (o: Object <- objects) {...}
```
Generator는 <-(arrow)옆에 위치한 순회 가능한 데이터인데, 중첩일 경우 semicolon으로 구분한다. 흔히 java에서 아래와 같은 이중 loop문은
```java
//java
for (int i = 0; i < 10; i++) {
    for (int j = 0; j < 10; j++) {
        ...
    }
}
```
아래와 같이 Scala에서 표현할 수 있다.
```scala
for (i <- 1 to 10; j <- 1 to 10) {...}
```
1 to 10과 같은 표현은 일단은 그냥 받아들이자. 이중 loop 같아 보이지 않을 수도 있다. 한 번 더 익숙해지라고 이번엔 2차원 List를 순회하는 예제를 보여주겠다.
```scala
val secondDim = List(List(1, 2, 3), List(4, 5, 6))
for (firstDim <- secondDim; elem <- firstDim) print(s"$elem") //123456
```

`for` 안에 filter를 넣을 수도 있다.
```scala
for (e <- List(3, 2, 5, 4, 1) if e >= 3)
  print(e) //result : 354
```  
`yield`는 `for comprehension`의 일부로 `for`식을 통과한 새로운 리스트를 반환한다.

```scala
var newList = for (e <- List(3, 2, 5, 4, 1) if 3 >= 3) {
    ...
} yield e
newList.foreach(e => print e) //354
```

참고로 `yield`가 없는 경우는 `Unit`을 리턴한다


`yield`가 포함되어 있고, 이를 `for comprehension`이라고 한다. 이 역시 expression으로 값을 리턴한다.

음... 값을 리턴한다? Scala의 Loop는 반복"문"(statement)가 아닌 반복"식"(expression)이 맞다. 아래의 값을 보자
```scala
class People(val name: String)

val names = for(p <- List(
    new People("Moon"), 
    new People("Johan"), 
    new People("K"))) yield p.name //names equal to List("Moon", "Johan", "K")
```
for loop는 3Person이 있는 List를 순회하며 p.name을 뽑은 새 List를 반환한다.

그렇다면 `yield`가 없는 경우라면?
```scala
val result = for (num <- 1 to 10) {}
```
정답은 result는 `Unit`이다.

# Reference
- [Why if as a expression is cool? in Stackoverflow](https://stackoverflow.com/questions/35399135/why-if-as-expression-not-statement-is-cool)