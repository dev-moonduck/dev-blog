# Scala의 Collection
Scala는 구조적으로 mutable(가변)과 immutable(불변) collection으로 구성되어 있다. Java나 일반 명령형 프로그래밍에서 흔히 써왔던 collection들이 mutable에 해당하며 scala에서도 역시 지원하고 있다. 그러나 이런 mutable한 collection은 functional programming에서 다루는 side effect를 가지고 있으며, 반면 immutable collection은 persistent한 특성을 가지고 있으며 side effect를 지니지 않도록 설계되었다.  

## Package 구성

#### `scala.collection`  
scala의 모든 collection은 이 package의 하위에 존재한다.  

#### `scala.collection.immutable`  
이 package에 속한 모든 자료구조는 immutable(불변)이 보장된다.

#### `scala.collection.mutable`

#### `scala.collection.concurrent`

#### `scala.collection.generic`

#### `scala.collection.parallel`

#### `scala.collection.script`


