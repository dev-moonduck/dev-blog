## [Referential Transparency(참조 투명성)](https://ko.wikipedia.org/wiki/%EC%B0%B8%EC%A1%B0_%ED%88%AC%EB%AA%85%EC%84%B1)

함수형 프로그래밍에서 함수의 속성은 참조 투명성을 만족시켜야 한다는 것이다. 여기서는 참조 투명성이 무엇인지 알아본다.  

```
표현식은 프로그램의 의미 변화없이 그 표현식의 결과를 대체할 수 있어야한다.
(the expression can be replaced by its result without changing the meaning of
the program)
```

말이 좀 어렵다.  


아래의 예제로 확인해보자.

```scala
class ReferentialTransparentTest {
    var acc = 0

    def add(n1: Int, n2: Int): Int = n1 + n2

    def inc(n: Int): Int = {
        acc += n
        acc
    }
}
```

위 코드에서 참조 투명성을 만족시키는 함수는 무엇일까? 정답은 add이다.

add는 