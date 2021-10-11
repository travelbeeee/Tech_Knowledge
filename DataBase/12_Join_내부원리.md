# Join  원리

 `Join` 은 두 개 이상의 테이블을 하나의 집합으로 만드는 연산입니다.

 정리하기 전에 용어를 먼저 정하겠습니다.

- 반복문의 외부에 있는 테이블을 `선행 테이블` 또는 `Outer 테이블`이라고 합니다.
- 반복문의 내부에 있는 테이블을 `후행 테이블` 또는 `Inner 테이블` 이라고 합니다.

<br>

### NL 조인 ( Nested Loops )

 하나의 테이블을 기준으로 각 row를 추출 할 때마다 순차적으로 상대 테이블의 연관된 모든 row들을 조인에 의해 추출하는 연결 방식입니다. 중첩 반복문가 유사한 방식으로 조인을 수행합니다.

 아이돌 그룹 테이블과 아이돌 테이블이 있을 때, 아이돌 그룹을 Outer 테이블, 아이돌 테이블을 Inner 테이블로 `NL Join`을 해보겠습니다. 아이돌 그룹과 아이돌 테이블을 연결시켜주는 Column은 `Group_Name` 이라고 하겠습니다. 

 NL 조인은 다음과 같이 동작합니다. 아이돌 그룹에서 A그룹 row를 선택하고, 아이돌 테이블에서 A그룹에 해당되는 아이돌을 선택합니다. B그룹 row를 선택해 마찬가지로 아이돌 테이블에서 B그룹에 해당되는 아이돌을 선택하고, 이 과정을 반복해 모든 아이돌 그룹 테이블에 있는 row(그룹) 마다 아이돌 그룹을 순회하게 됩니다.

 만약에 두 테이블을 연결시켜주는 `Group_Name` Column으로 만들어진 Index 가 아이돌테이블에 없다면 `Full Scan` 으로 동작하게 될 것입니다. 즉, 굉장히 비효율적으로 동작할 수 밖에 없습니다.

 또, 아이돌 테이블을 Outer 테이블로, 아이돌 그룹 테이블을 Inner 테이블로 `NL Join`을 한다고 생각해보겠습니다. 아이돌마다 아이돌 그룹 테이블을 순회하게 되므로 성능에 좋지 않을 것을 알 수 있습니다.

 정리하면 다음과 같습니다.

- Inner Table 에 적절한 Index가 없다면 Outer Table의 모든 요소마다 Full scan을 진행하게 되므로 굉장히 비효율적이다.

- 조건을 만족하는 행의 수가 적은 쪽을 Outer Table로 지정하는 것이 성능에 좋다.

 NL 조인은 대량의 테이블이 얽힌 상황에서는 좋지 않은 방식이고, 1 : M 관계에서 1에 해당되는 테이블을 Outer Table로 설정하는 것이 성능에 유리하다.

<br>

### Sorted Merge Join

 NL 조인과 내부 동작 방식은 유사하나 Join Column 을 기준으로 먼저 다 Sorting 시키고 Join을 진행하는 방식입니다.

 `Outer 테이블`과 `Inner 테이블`을 먼저 Join Column을 기준으로 Sorting 시킨 후, NL 조인과 같은 방식으로 동작합니다. 따라서, Inner Table에 적절한 Index가 없다면 NL 조인보다 Sorted Merge Join 이 더 유리하다. 하지만 Sorting을 하기 때문에 성능이 그리 좋지 않습니다.

<br>

### Hash Join

  `NL Join` 의 `Random Aceess` 와 `Sort Merge Join` 의 정렬 작업의 부담을 해결하기 위한 대안으로 등작한 Join 입니다. `Hash Function`과 `Hash Table` 을 이용해서 Join을 수행합니다.

 먼저, `Outer 테이블`에서 조건을 만족하는 행을 찾아 Join Column 값을 `Hash Function`을 적용해 `Hash Table`을 생성합니다. `Inner 테이블`에서 조건을 만족하는 행을 찾고 Join Column 값을 `Hash Function`을 적용해 미리 만들어 둔 `Hash Table` 에서 값을 찾아 Join을 진행하는 방식입니다. 

-  대용량 테이블을 Join 할 때 쓰면 좋은 Join 방식이다.
- 적절한 Index가 없어도 `Hash Function`을 이용하기 때문에 성능상에 문제가 없다.
- `Hash Function`을 이용하기 때문에 중복되는 값이 많은 Column을 Join Key로 삼으면 성능이 좋지 않다.
- `Hash Table`을 만들기 위해 메모리를 추가로 사용해야한다.

<br>

### Q.  무조건 Hash Join 기법이 좋은가 

 상황에 따라 NL Join, Sorted Merge Join, Hash Join 을 맞게 사용하는 것이 좋다. 

```sql
select * from Member inner join Team on Member.team_id = Team.id
	where Member.name = '홍길동'
```

 위의 쿼리는 결국 이름이 '홍길동'인 사람에 대해서만 Join이 일어나게되므로 `Sorted Merge Join` 을 이용해 Sorting을 하거나, `Hash Join`을 이용해 추가적인 메모리를 사용하지 않아도 `NL Join`으로도 금방 조회가 가능할 것이다. 이처럼 속도, 메모리를 모두 생각해서 상황에 맞는 조인 전략을 사용해야한다.