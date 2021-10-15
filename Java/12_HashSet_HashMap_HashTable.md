# HashS

### 1) HashSet, HashMap, HashTable, Dictionary

자바 기준으로 `HashSet`, `HashMap`, `HashTable`을 정리해보겠습니다.

- HashSet
  - `HashSet`은 데이터의 집합을 표현한 `Set` 인데 내부적으로 `해싱`을 이용한 자료구조입니다. 
  - 저장 순서를 유지하지 않습니다.
  - hashCode() 를 이용해서 객체마다 `hashing`을 진행하고 집합에 저장합니다.
  - `null`을 저장할 수 있지만 하나만 가능하다.
  - 내부적으로 `HashMap`을 이용하는데 `Key-Value`에서 `Value`에는 Dummy data를 넣어둡니다.
- HashMap
  - `HashMap`은 `Key-Value` 형태로 데이터를 저장하는 자료구조로 내부적으로 `해싱`을 이용합니다. 
    - Key로 넘어온 객체의 hashCode() % M 으로 먼저 해싱을 처리하고 보조 해시 함수를 한 번 더 사용합니다.
  - 자바8부터는 보조 해시 함수 + `Seperate Chaining` 기법을 이용해서 해시 충돌을 해결하고 있습니다.
    - `Seperate Chaining` 기법에서 8개 이상의 데이터가 하나의 버킷에 연결되게되면 빠른 조회를 위해 `Linked List` 구조를 `Tree`구조로 바꿔서 저장하고 있습니다. 다시 6개 이하의 데이터로 데이터가 작아지면 `Tree`구조를 `Linked List` 구조로 바꿔서 저장합니다.
    - `Tree`는 `RedBlackTree`를 이용해서 빠르게 동작하도록 구현되어있습니다.
  - 자바8부터는 버킷의 크기 M의 0.75배 만큼 데이터가 쌓이면 버킷의 크기를 2배씩 늘립니다. 
    - 버킷의 크기를 늘리는 행위는 새로운 메모리를 할당받고 다시 해시테이블을 만들어야되므로 처음에 초기 버킷 크기를 잘 할당하는 것이 성능에 중요합니다. ( 자바에서는 초기 M = 16 )
- HashTable
  - `HashTable`은 `HashMap`과 동일한 기능을 제공하는데 보조 해시 함수를 사용하지 않아 해시 충돌 가능성이 조금 더 높고, 동기화를 지원하기 때문에 성능이 조금 더 느립니다.
  - JDK 1.0부터 있던 Java API이고 변경 사항이 거의 없었기 때문에 오래된 프로그램에서 자주 사용합니다.
- Dictionary
  - 자바에서 Dictionary는 인터페이스입니다. `HashTable`은 `Dictionary`인터페이스의 구현체입니다. Python `Dictionary` 자료구조와 비슷한 것은 자바의 `HashMap` 자료구조입니다.

### 2) HashMap 자료구조의 Best와 Worst

 자바8 이상에서는 HashMap 자료구조를 Seperate Chaining 기법으로 처리하고 있습니다. 따라서, N개의 데이터와 M개의 버킷에 대해서 모든 데이터에 대해서 해시 충돌이 발생한다면 최악의 경우 O(N)의 복잡도를 가지게 됩니다. 최선의 경우에는 O(1) 복잡도를 가지게 됩니다.