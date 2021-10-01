# WITH RECURSIVE

Mysql 5.7 위의 버전부터는 `WITH RECURSIVE` 를 이용해 가상의 테이블을 만들 수 있다.

이름 그대로 재귀 커리를 이용하여 실제로 테이블을 생성하거나 데이터 삽입을 하지 않고 가상 테이블을 만드는 방법이다.

```mysql
WITH RECURSIVE 테이블명 AS(
	SELECT 초기값 AS 컬럼별명
    UNION ALL
    SELECT 컬럼별명 계산식 FROM 테이블명 WHERE 제어문)
```

```mysql
-- HOUR 컬럼이 있고 0 ~ 23까지 값이 들어있는 24개의 레코드를 가지고 있는 테이블
WITH RECURSIVE timeTable  AS (
    SELECT 0 AS HOUR FROM DUAL
    UNION ALL
    SELECT HOUR + 1 FROM timeTable
    WHERE HOUR < 23
)

-- HOUR 컬럼과 COUNT 컬럼을 가지고 있고, HOUR 컬럼에는 0 ~ 23까지 값이 들어있고 COUNT 컬럼에는 0이 들어있는 24개의 레코드를 가지고 있는 테이블 
WITH RECURSIVE timeTable  AS (
    SELECT 0 AS HOUR, 0 AS COUNT FROM DUAL
    UNION ALL
    SELECT HOUR + 1 , 0 FROM timeTable
    WHERE HOUR < 23
)
```

