# 05_Oracle_정렬

### 1) 정렬 - ORDER BY

`ORDER BY` 구를 사용하면 검색 결과의 정렬 순서를 지정할 수 있습니다.

```sql
SELECT 열명 FROM 테이블명 WHERE 조건식 ORDER BY 열명;
```

 을 지정하면 ORDER BY 뒤에 명시된 열을 기준으로 검색 결과의 행 순서가 변경됩니다. 

기본적으로 오름차순으로 설정되어있고, 내림차순으로 정렬하려면 다음과 같이 추가 지정을 해줘야합니다.

```sql
SELECT 열명 FROM 테이블명 WHERE 조건식 ORDER BY 열명 DESC; // 내림차순정렬 
SELECT 열명 FROM 테이블명 WHERE 조건식 ORDER BY 열명 ASC; // 오름차순정렬 -> Default 값
```

문자열형 데이터의 대소관계는 사전식 순서에 의해 결정됩니다.

또, ORDER BY는 결과만 정렬해서 보여주는 것이지 테이블에 영향을 주지 않습니다.

```sql
SELECT 열명 FROM 테이블명 WHERE 조건식 ORDER BY 열명1, 열명2, ... ;
```

다음과 같이 복수의 열을 정렬 기준으로 설정할 수 있고, 열명1 값이 같다면 열명2를 기준으로 정렬하고, ....

마찬가지로 열마다 내림차순, 오름차순 기준을 정해줄 수 있습니다.

```sql
SELECT 열명 FROM 테이블명 WHERE 조건식 ORDER BY 열명1 DESC, 열명2 ASC;
```

> #####  NULL 값을 어떻게 정렬될까
>
> NULL 값을 가지는 행은 가장 먼저 표시되거나 가장 나중에 표시되는데 데이터베이스 제품에 따라 기준이 다릅니다.

<br>

### 2) 결과 행 제한하기 - ROWNUM

ROWNUM은 클라이언트에 결과가 반환될 때 각 행에 할당되는 행 번호입니다. 단, ROWNUM 으로 행을 제한할 때는 WHERE 구로 지정하므로 정렬하기 전에 처리됩니다.

```sql
SELECT 열명 FROM 테이블명 WHERE ROWNUM <= 5;
```

5개의 행이 출력됩니다.

그러면, 정렬 후에 5개의 행을 가지고 오려면 어떻게 해야될까요??

```sql
SELECT 열명 FROM 테이블명 WHERE ROWNUM <= 5 ORDER BY 열명;
```

ORDER BY 는 WHERE 구 이후에 진행되므로 위의 쿼리는 5개의 행을 뽑고 나서 정렬을 하게 됩니다.

따라서, 정렬 후 결과에서 5개를 뽑으려면 다음과 같이 쿼리를 날려야됩니다.

```sql
SELECT 열명 FROM ( SELECT 열명 FROM 테이블명 ORDER BY 열명 ) WHERE ROWNUM <= 5;
```

<br>

### 3) 열에 별명 지정하기

SELECT 결과로 나온 열에 새롭게 별명을 지정해 줄 수 있습니다.

| no   | price | quantity |
| ---- | ----- | -------- |
| 1    | 100   | 10       |
| 2    | 150   | 24       |
| 3    | 1980  | 1        |

다음과 같은 `item_table` 이 있다고 하겠습니다.

```sql
SELECT price * quantity from item_table;
```

쿼리를 수행하면 다음과 같은 결과가 나옵니다.

| price * quantity |
| ---------------- |
| 1000             |
| 3600             |
| 1980             |

이때, 새롭게 만든 열이 `price * quantity` 열에 별명을 지정할 수 있습니다.

```sql
SELECT price * quantity amount from item_table;
SELECT price * quantity AS amount from item_table;
```

`AS`예약어를 사용해도 되고, 생략해도 됩니다.

##### -3-1) 주의!

처리순서 : WHERE 구 --> SELECT 구

```sql
SELECT price * quantity AS amount from item_table where amount >= 100;
```

은 에러가 발생합니다!

WHERE구 --> SELECT구 순으로 내부적으로 처리하기 때문에 아직 정의되지 않은 amount를 참조하므로 에러가 발생합니다.

##### -3-2) 주의!

처리순서 : WHERE 구 --> SELECT 구 --> ORDER BY 구

반대로 ORDER 구는 SELECT 구 뒤에서 처리되기 때문에 지정한 별명을 마치 그런 열이 존재하는 것처럼 사용할 수 있습니다.

```sql
SELECT * price * quantity AS amount from item_table order by amount desc;
```

