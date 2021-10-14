# Transaction Isolation Level

 ACID 원칙을 strict 하게 지키려면 동시성이 매우 떨어지기 때문에 DB 엔진은 ACID 원칙을 희생하여 동시성을 얻을 수 있는 방법을 제공합니다. 바로 transaction의 isolation level이다. **Isolation 원칙을 덜 지키는 level을 사용할수록 문제가 발생할 가능성은 커지지만 동시에 더 높은 동시성을 얻을 수 있다**. 

 Isolation Level은 동시에 여러 트랜잭션이 처리될 때, 특정 트랜잭션이 다른 트랜잭션에서 변경하거나 조회하는 데이터를 볼 수 있도록 어디까지 허용할지 정해놓은 기준으로 4가지 종류가 있습니다.

### 1) Transaction Isolation Level 종류

- Read Uncommitted
- Read Committed ( Oracle 기본 전략 )
- Repeatable Read ( MySql 기본 전략 )
- Serializable

Isolation Level에 따른 Transaction 문제 현상 3가지는 크게 다음과 같습니다.

- 아직 `COMMIT` 되지 않은 신뢰할 수 없는 데이터를 읽어옴(*dirty read*)
- 한 트랜잭션에서 동일한 `SELECT` 쿼리의 결과가 다름(*non-repeatable read*)
- 이전의 `SELECT` 쿼리의 결과에 없던 row가 생김(*phantom read*)

<br>

### 2)  Read Uncommitted

 이름 그대로 `Commit`이나 `Rollback` 여부에 상관 없이 다른 트랜젝션에서 값을 읽을 수 있는 격리 수준입니다. 데이터 정합성에 문제가 많은 격리 수준이기 때문에 사용하지 않는 것을 권장합니다.

```sql
A 트랜잭션에서 10번 사원의 나이를 27살에서 28살로 Update (아직 커밋하지 않음)
B 트랜잭션에서 10번 사원의 나이를 Select --> 28살 // Dirty Read
A 트랜잭션에서 문제가 발생해 ROLLBACK
B 트랜잭션은 10번 사원이 여전히 28살이라고 생각하고 로직을 수행함 --> 문제발생
```

트랜젝션 작업이 완료되지 않았는데도 다른 트렌젝션에서 볼 수 있으므로  `Dirty Read` 현상이 발생한다.

<br>

### 3) Read Committed

 이름 그대로 `Commit`이 완료되어야만 다른 트랜젝션에서 조회할 수 있는 격리 수준이다. 오라클 DBMS에서 기본으로 사용하고 있는 격리 수준입니다. 데이터를 실제 테이블에서 가져오는 것이 아니라 Undo 영역의 백업된 레코드에서 값을 가져옴으로써 트랜젝션 작업이 완료되지 않았을 때는 변경 사항을 조회할 수 없게 합니다.

```sql
B 트랜잭션에서 10번 사원의 나이를 Select --> 27살
A 트랜잭션에서 10번 사원의 나이를 27살에서 28살로 바꾸고 커밋
B 트랜잭션에서 10번 사원의 나이를 다시 조회(변경되지 않은 이름이 조회됨) --> 28살
```

 `Dirty Read` 문제에서는 벗어났으나 하나의 트랜젝션내에서 똑같은 조회 쿼리를 실행했을 때 다른 값이 나와 `Non-Repeatable Read` 문제가 발생합니다. 금전적인 처리를 하는 상황에서는 입금, 출금 처리가 진행되는 과정에서 다른 값이 조회되므로 큰 문제가 발생합니다.

<br>

### 4) Repeatable Read

 `Repeatable Read` 격리 수준은 트랜잭션이 시작되기 전에 커밋된 내용에 대해서만 조회할 수 있는 격리수준입니다. 처음으로 `read operation`을 수행한 시간을 기준으로 `consistent read`를 보장해주고 첫 `read` 의 `snapshot` 을 조회하게 됩니다. ( Snapshot이 저장되어 있는 영역을 Undo 영역이라고 한다. )

```sql
B 트랜젝션에서 10번 사원의 정보를 select ( 나이는 27살 이름은 travelbeeee )
A 트랜잭션에서 10번 사원의 이름을 travelbeeee에서 honeybeeeee 로 수정 후 커밋
B 트랜젝션에서 travelbeeee 이름을 가진 사원의 나이를 28살로 수정 // travelbeeee 이름을 가진 사원이 없으므로 반영되지 않는다.
```

 B트랜잭션 입장에서는 방금 자신이 `travelbeeee` 이름을 가진 사원을 조회하고 그 사원의 이름을 수정한건데 Update Query가 제대로 반영되지 않습니다.

```sql
B 트랜젝션에서 모든 사원의 정보를 select --> 0건 조회
A 트랜잭션에서 (1번 사원, travelbeeee, 27살) 을 insert 하고 커밋
B 트랜젝션에서 모든 사원의 정보를 select --> 0건 조회
B 트랜젝션에서 travelbeeee 이름을 가진 사원의 나이를 28살로 Update --> 1건 반영
B 트랜젝션에서 모든 사원의 정보를 select --> 1건 조회
```

 B트랜젝션에서 Update Query를 날리면 레코드에 대해 쓰기 잠금을 시도하고 Undo 영역이 아닌 실제 레코드에 접근합니다. 따라서, 이후에 갑자기 Select 쿼리 결과가 바뀌는 `Phantom Read` 문제가 발생하게 됩니다.

<br>

### 5) Serializable

 가장 단순하고 엄격한 격리 수준입니다. `Select` 작업에도 `공유 잠금`을 설정하게 되어 모든 문제에서 벗어나지만 동시에 다른 트랜젝션에서 레코드를 접근할 수 없으므로 동시 처리 능력이 떨어지고, 성능 저하가 발생하게 됩니다. 또, 데드락 문제도 자주 발생합니다.

##### S Lock : ( Shared lock, 공유 잠금 )

읽기 잠금(Read lock)이라고도 불린다.
어떤 트랜잭션에서 데이터를 읽고자 할 때 다른 shared lock은 허용이 되지만 exclusive lock은 불가하다.
쉽게 말해 리소스를 다른 사용자가 동시에 읽을 수 있게 하되 변경은 불가하게 하는 것이다.
=> 어떤 자원에 shared lock이 동시에 여러개 적용될 수 있다.
=> 어떤 자원에 shared lock이 하나라도 걸려있으면 exclusive lock을 걸 수 없다.

##### X Lock : ( Exclusive lock, 배타적 잠금 )

쓰기 잠금(Write lock)이라고도 불린다.
어떤 트랜잭션에서 데이터를 변경하고자 할 때(ex . 쓰고자 할 때) 해당 트랜잭션이 완료될 때까지 해당 테이블 혹은 레코드(row)를 다른 트랜잭션에서 읽거나 쓰지 못하게 하기 위해 Exclusive lock을 걸고 트랜잭션을 진행시키는 것이다.
=> exclusive lock에 걸리면 shared lock을 걸 수 없다. (shared lock은 아래에서 설명)
=> exclusive lock에 걸린 테이블,레코드등의 자원에 대해 다른 트랜잭션이 exclusive lock을 걸 수 없다.

```sql
A 트랜젝션에서 1번 사원의 정보를 select --> S Lock
B 트랜젝션에서 1번 사원의 정보를 select --> S Lock
B 트랜젝션에서 1번 사원의 정보를 update --> A 트랜젝션에 의해 S Lock이 있으므로 X Lock 실패 --> Update 실패
B 커밋
A 트랜젝션에서 1번 사원의 정보를 update --> B 트랜젝션에 의해 S Lock이 있으므로 X Lock 실패 --> Update 실패
A 커밋
--> 데드락 상황 발생 --> Timeout으로 실패 후 데이터는 원래대로 남아있게 된다.
```

