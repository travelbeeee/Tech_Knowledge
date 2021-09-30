# 테이블 생성, 삭제, 변경 명령어

테이블은 RDBMS 에서 가장 많이 사용되는 객체입니다. 객체를 생성하고 삭제하고 변경하는 명령어는 DDL 로 모두 같은 문법을 사용합니다.

<br>

### 1) 테이블 생성

```sql
CREATE TABLE 테이블명 ( 
	열 정의1,
	열 정의2,
	...
);
```

CREATE 명령어를 이용하면 객체를 생성할 수 있습니다. TABLE을 생성하기 위해서는 CREATE TABLE 과 테이블명, 테이블의 열을 지정해주면 됩니다.

열 정의는 다음과 같습니다.

```
열이름 자료형 [DEFAULT 기본값] [NULL | NOT NULL]
```

DEFAULT는 기본값을 지정해주는 속성이고, NULL | NOT NULL 은 NULL 값 허용 여부를 지정해주는 속성이다. DEFAULT는 생략하면 NULL로 들어가고, NULL | NOT NULL 은 생략하면 NULL을 허용해준다.

<br>

### 2) 데이터 타입

먼저, Oracle 에서 지원해주는 데이터 타입 중 자주 쓰이는 데이터 타입부터 알아보겠습니다.

#### -문자형

```sql
// n은 바이트 수를 의미

CHAR(n) // 고정 길이 문자열 ( 최대 2,000 바이트까지 저장 가능 )
VARCHAR2(n) // 가변 길이 문자열 ( 최대 4,000 바이트까지 저장 가능 )
NCAHR(N) // 국제 문자셋을 지원하는 고정 길이 문자열
NVARCHAR2(N) // 국제 문자셋을 지원하는 가변 길이 문자열
```

문자형은 크게 고정 / 가변 길이 문자열로 나눠볼 수 있습니다. 고정 길이 문자열은 말그대로 입력된 바이트 수에 따라 항상 일정한 크기를 가지는 문자열이고, 가변 길이 문자열은 실제 입력되는 데이터 크기에 따라서 크기가 바뀌는 문자열입니다.

> 참고! 고정 길이 문자열의 남는 부분은 공백으로 채워진다.

<br>

#### -숫자형

```sql
// precision은 정밀도, scale은 소수점 이하 자릿수를 의미

NUMBER(precision, scale) // 최대 38자리 정밀도를 가진 숫자형
FLOAT(precision) // NUMBER 데이터 형식의 서브 타입
```

숫자형은 대부분 NUMBER형을 사용합니다. precision는 소수점을 포함한 전체 자릿수를 의미합니다. 가변숫자이므로 precision, scale을 입력하지 않으면 저장 데이터의 크기에 맞게 자동으로 조절됩니다.

Oracle은 ANSI SQL에서 소개하는 INT, INTEGER, FLOAT 등을 지원하지만 모두 NUMBER, FLOAT 자료형으로 바꿔서 저장합니다. 따라서, Oracle 에서 지원하는 NUMBER, FLOAT 자료형을 이용하는게 더 좋습니다.

<br>

#### -시간/날짜형

```sql
DATE // 7byte 고정길이 연도, 월, 일, 시간, 분, 초 표현
TIMESTAMP // 7byte or 11byte 고정길이 DATE와 유사하지만 초 단위를 소수점 9자리까지 표현 가능 
TIMESTAMP with TIME ZONE // 13byte 고정길이, 표준 시간대 정보를 저장
TIMESTAMP with LOCAL TIME ZONE // DBMS 환경에 따라 표준 시간대 정보 결정
INTERVAL YEAR TO MONTH // 5byte 고정길이 연, 월 기간 정보 저장
INTERVAL DAY TO SECOND // 11byte 고정길이 일, 시, 분, 초의 기간 정보 저장
```

시간/날짜형은 DATE 형을 제일 많이 사용합니다.

<br>

#### -Large Object

```sql
CLOB // 최대 4GB, XML 또는 문자 정보를 저장
NCLOB // 국제 문자 형식을 지원하는 CLOB
BLOB // 최대 4GB, 바이너리 형식의 문서/ 이미지 등의 정보 저장
BFILE // 최대 4GB, 바이너리 파일 저장
```

대용량 객체를 표현하는 자료형은 위와 같습니다.

<br>

### 3) 테이블 삭제

테이블을 삭제하는 명령어는 다음과 같습니다.

```sql
DROP TABLE 테이블명 [CASCADE CONSTRAINTS];
```

DROP 이라는 명령어를 이용하여 테이블을 삭제할 수 있고, CASCADE CONSTRAINTS 속성을 지정하면 삭제가 되는 테이블과 참조관계나 제약관계가 있는 하위 테이블까지 모두 삭제합니다.

<br>

### 4) 테이블 변경

ALTER 명령어를 이용하면 테이블을 변경할 수 있습니다.

#### 4-1) 테이블 컬럼 추가하기

```sql
ALTER TABLE 테이블명 ADD (열이름 자료형 [DEFAULT 기본값] [NULL | NOT NULL]);
```

ALTER TABLE ADD 명령어를 이용하면 테이블에 컬럼을 추가할 수 있습니다.

<br>

#### 4-2) 테이블 컬럼 수정하기

```sql
ALTER TABLE 테이블명 MODIFY (열이름 자료형 [DEFAULT 기본값] [NULL | NOT NULL]);
```

ALTER TABLE MODIFY 명령어를 이용하면 테이블 컬럼의 이름을 제외하고 수정할 수 있습니다.

```sql
alter table writing modify (views default 0); // 예시 writing table의 views 열 Default수정
```

<br>

#### 4-3) 테이블 컬럼 이름 수정하기

```sql
ALTER TABLE 테이블명 RENAME COLUMN 원래열이름 TO 수정할열이름;
```

ALTER TABLE RENAME COLUMN 명령어를 이용하면 테이블 컬럼의 이름을 수정할 수 있습니다.

<br>

#### 4-4) 테이블 컬럼 삭제하기

```sql
ALTER TABLE 테이블명 DROP COLUMN 컬럼명;
```

ALTER TABLE DROP COLUMN 명령어를 이용하면 테이블 컬럼을 삭제할 수 있습니다.

<br>

