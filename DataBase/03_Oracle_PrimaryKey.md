# Primary Key

Primary Key는 기본키라고 불리며 행을 고유하게 구분해 주는 최소의 정보입니다. 기본키는 다음과 같은 특성이 있습니다. **다른 항목과 절대로 중복되어 나타날 수 없는 단일성(Unique)와 절대 Null 값을 가질 수 없다는 특징**을 가지고 있습니다. 즉, 기본키를 가지고 우리는 데이터들을 구분할 수 있습니다.  

관계형 데이터베이스에서는 반드시 각 행이 고유하게 식별되어야 합니다. 따라서, 테이블을 생성할 때 기본키를 제약조건으로 추가하는 습관을 들이는 게 좋습니다.

<br>

### 1) Primary Key 선언

Primary Key는 테이블을 생성할 때 제약조건으로 추가할 수 있습니다.

```sql
CREATE TABLE TEST(
    Col1 Number(1, 0) PRIMARY KEY,
    Col2 Number(1, 0) CONSTRAINT pk2 PRIMARY KEY,
    Col3 Number(1, 0),
    CONSTRAINT pk3 PRIMARY KEY(Col3)
);
```

3가지 방법으로 Primary Key를 선언할 수 있습니다.

Col1 컬럼처럼 `PRIMARY KEY` 를 바로 선언할 수도 있습니다.

Col2 컬럼처럼 `CONSTRAINT 기본키별명 PRIMARY KEY` 로 서브네임을 줄 수도 있습니다.

마지막으로는 가장 많이 쓰이는 방법입니다.

Table 에 필요한 컬럼을 다 서술하고 마지막에 `CONSTRAINT 기본키별명 PRIMARY KEY(컬럼명)` 제약 조건을 걸어주는 방법입니다.

` CONSTRAINT pk PRIMARY KEY(Col1, Col2, Col3)` 여러 개의 컬럼을 기본키로 구성할 수도 있습니다.

> 참고!
>
> Oracle에서는 Primary Key는 1개만 지정 할 수 있습니다.
>
> Primary Key를 구성하는 컬럼이 복수일 수는 있으나, Primary Key는 1개만 가능합니다.

<br>

### 2) Primary Key 수정

ALTER 명령어를 이용해 Primary Key를 추가하거나 제거할 수 있습니다.

```sql
// Primary Key 추가
ALTER TABLE 테이블명 ADD CONSTRAINT PK별명 PRIMARY KEY(컬럼명);

// Primary Key 삭제
ALTER TABLE 테이블명 DROP CONSTRAINT PK별명 PRIMARY KEY;
ALTER TABLE 테이블명 DROP PRIMARY KEY;
```

