# Type Alias  
Java에는 없는 키워드 중에 Scala에 있는 독특한 키워드가 있는 걸 보고 여기서 소개한다.

## 정의  
`Type alias`는 이미 정의된 `type`(`class`, `object`, `trait`)에 추가적인 타입 이름을 부여하는 것을 의미한다. 이 `Type alias`는 마치 새로운 `type`을 정의한 것처럼 사용할 수 있다.

## 사용

`object`, `class`, `trait`내부에서 선언한다.  
선언 규칙은 다음과 같다.  

```
type <identifier>[type parameters] = <type name>[type parameters]
```

예시를 보자  
```scala
object MyObject {
    type MyNum = Int
}
```

## 주의사항
`object`, `class`, `trait`내에서만 정의가 가능하다.

## 이게 왜 필요하죠?
사실 Java에는 없는 언어차원에서의 지원인데 잘만 써왔다. 그러나 이 `type alias`를 통해 얻을 수 있는 첫 번째 이점은 불필요한 type 추가정의를 명시적으로 할 필요가 없다는 것이다.

A type alias creates a new named type for a specific, existing type (or class). This new type alias is treated by the compiler as if it were defined in a regular class. You can create a new instance from a type alias, use it in place of classes for type parameters, and specify it in value, variable, and function return types. If the class being aliased has type parameters, you can either add the type parameters to your type alias or fix them with specific types.

Like implicit conversions, however, type aliases can only be defined inside objects, classes, or traits. They only work on types, also, so objects cannot be used to create type aliases.

You can use the type keyword to define a new type alias.