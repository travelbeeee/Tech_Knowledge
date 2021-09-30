# DB_Index

##### 1) Index란

 Index는 색인 또는 목차를 의미하는 단어로, 우리가 책에서 찾고자하는 정보가 담긴 페이지를 빠르게 찾을 때 주로 사용한다.

 마찬가지로, DB에서의 Index도 테이블에서 원하는 정보를 빠르게 찾을 때 사용하는 자료구조이다. 자주 검색하는 테이블의 Column 정보를 미리 미리 인덱스로 만들어두어, 조회를 할 때 더 빠르게 조회를 할 수 있게 하는 원리이다.

 즉, 인덱스란 추가적인 쓰기 작업과 저장 공간을 활용하여 데이터베이스 테이블의 검색 속도를 향상시키는 자료구조이다.

 우리가 책의 목차를 생각해보면, 단순히 책만 쓰는 것이 아니라 책에 대한 목차를 추가로 만들어야된다. DB Index도 마찬가지로 추가적인 쓰기 작업과 저장 공간을 활용해야한다. 

<br>

##### 2) Index의 관리

 DBMS는 index를 항상 최신의 정렬된 상태로 유지해야 원하는 값을 빠르게 탐색할 수 있다. 그렇기 때문에 인덱스가 적용된 컬럼에 INSERT, UPDATE, DELETE가 수행된다면 각각 다음과 같은 연산을 추가적으로 해주어야 하며 그에 따른 오버헤드가 발생한다.

- INSERT : 새로운 데이터에 대한 인덱스를 추가해야한다.
- DELETE : 삭제하는 데이터에 대한 인덱스 정보를 갱신해야한다.
  - 이때, 인덱스는 실제로 삭제되는 것이 아니라, 사용하지 않는다고 표시만 한다. 따라서, 메모리 낭비가 발생한다.
- UPDATE : 수정하는 데이터에 대한 인덱스 정보를 갱신해야한다.
  - 이때, 인덱스가 실제로 갱신되는 것이 아니라, 기존의 인덱스를 삭제하고 갱신된 정보에 대한 새로운 인덱스가 추가된다.

<br>

##### 3) Index가 왜 필요할까

 사용자가 디스크로부터 데이터를 가져와야할 때, 기본적으로 `Full Table Scan` 원리로 작동하게 된다.

 100만, 1000만개의 데이터를 모두 순회하며 데이터를 찾는 것은 비효율적이고 굉장히 오랜 시간이 걸리는 작업이다. 따라서, 자주 찾는 데이터는 Index를 만들어 관리하는 것이 좋다.

> 실제로 테이블의 정보들은 물리 저장 공간에 뿔뿔이 흩어져있음! 
>
> --> Index가 없다면, 원하는 정보를 찾으려면 전체를 다 조회하는 방법 밖에 없음!

<br>

##### 4) Index 를 항상 사용하는 것이 좋을까

 INSERT, DELETE, UPDATE 쿼리는 인덱스를 사용해서 오히려 추가 작업과 메모리 낭비가 생기게 된다. 또, SELECT 쿼리로 전체 데이터를 다 불러오는 경우에는 INDEX가 당연히 도움이 되지 않는다.

 따라서, 상황에 맞춰서  **Index에 따른 오버헤드 vs Index를 만들어서 생기는 이점** 을 비교하고 판단해야한다.

 **무조건, Index 를 만드는 것이 옳은 것은 아니다.**

<br>

##### 5) Index 구조와 원리

 Index에서 가장 많이 사용되는 구조는 B-Tree 자료구조 ( B-Tree = Balanced Tree )

![그림5](https://user-images.githubusercontent.com/59816811/131294135-b15d805c-ea32-4cea-94d0-3e1da0b9750f.png)

그림과 같이 Balanced Tree 자료구조를 이용해 logN 복잡도로 원하는 정보를 탐색할 수 있게 내부적으로 동작합니다. 

그림만 보더라도 추가적인 메모리와 인덱스를 관리하기 위한 작업이 필요한 것을 확인할 수 있습니다.

> 트리 구조에서는 한 쪽에 데이터가 몰릴 수 있음! 
>
> --> 그러면, 트리 구조보다는 리스트에 가깝에 되고 복잡도가 높아짐
>
> --> 균형이 맞는 B-Tree를 이용하자.

<br>

##### 6) Index Column 설정 기준

 `Cardinality`가 높은 것을 기준으로 삼는게 좋다. 즉, 정보를 잘게 세분화할 수 있는 Column을 기준으로 삼는 것이 좋다.

- Cardinality : 종류의 수 / 분별력
  - 성별은 남/여 2가지 종류 --> Cardinality = 2
  - 서울내의 대학교 약 100가지 종류 --> Cardinality = 100

> 개발자가 특정 기준으로 Index를 만들어놓기만 하면, 현대의 DataBase가 알아서 Index를 관리해준다.

<br>

> 참고한 블로그
>
> https://beelee.tistory.com/37
>
> https://mangkyu.tistory.com/96