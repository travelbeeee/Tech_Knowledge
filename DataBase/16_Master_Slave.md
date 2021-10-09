# Master_Slave

### 1) Replication

 DB는 용량을 2~3배 써서라도 데이터를 잘 보관해 안정성을 유지하는 것이 중요합니다. 이렇게 데이터를 백업해 두는 방식으로는 `Replication`이 있습니다.

![다운로드](C:\Users\sochu\바탕 화면\다운로드.png)

 `Replication`은 단방향으로 일어납니다. `Replication`을 하는 DB를 `Master`라고 하고, `Replication`을 통해 만들어진 DB를 `Slave` 라고 합니다. `Slave`는 `Master`와 동기화되지만, `Master`는 `Slave`와 동기화하지 않습니다.



Q. Master-Slave를 사용하는 이유

 하나의 DB에 `Request`가 모두 몰리게되면 과부하가 걸릴 수 있습니다. 따라서, `Master-Slave`로 DB를 나눠서 Insert, Update, Delete Query는 `Master` 에서 관리하고 Select Query는 요청은 `Slave` 에서 처리하도록 구현하면 `Request` 요청을 분산시킬 수 있습니다. 또, `Master-Slave` 방식을 사용하면 같은 정보가 여러 DB에 저장되면서 추가적인 용량은 발생하지만 데이터를 더욱 안전하게 보관할 수 있습니다. 

