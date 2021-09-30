# Select

Select 명령어를 이용하면 데이터를 조회할 수 있습니다.

<br>

#### 0) DESC

```sql
DESC 테이블명;
```

DESC 명령어를 이용하면 테이블 구조를 참조할 수 있습니다.

<br>

#### 1) SELECT FROM

SQL에서 "Hello World" 역할을 하는 명령어로 테이블에서 모든 데이터를 조회하는 SQL Query 입니다.

```sql
SELECT * FROM 테이블명;
```

*은 '모든 열' 이라는 의미의 메타 문자입니다.

```sql
SELECT 열1, 열2 ... FROM 테이블명;
```

위처럼 조회를 원하는 열을 지정할 수도 있습니다.

열 지정 순서는 임의로 정할 수 있고, 존재하지 않는 열을 지정하거나 열을 전혀 지정하지 않으면 에러가 발생합니다.

<br>

#### 2) SELECT FROM WHERE

WHERE 구를 이용하면 많은 데이터 속에서 원하는 조건에 해당하는 데이터만 뽑아올 수 있습니다.

```sql
SELECT * FROM 테이블명 WHERE 조건식;
```

조건식은 '열과 연산자, 상수고 구성되는 식' 입니다.

NULL 을 검색할 때는 = 연산자로 검색할 수 없고 IS NULL / IS NOT NULL 을 이용해야 합니다.

```sql
SELECT * FROM 테이블명 WHERE 열 IS NULL;
SELECT * FROM 테이블명 WHERE 열 IS NOT NULL;
```

<br>

> 참고!
>
> - = 연산자 
>
>   좌변과 우변의 값이 같은 경우 참이 된다.
>
> - <> 연산자
>
>   좌변과 우변의 값이 같지 않을 경우 참이 된다.
>
> - <, >, <=, >= 연산자
>
>   좌변과 우변의 값을 비교하는 연산자

<br>

#### 3) 조건 조합하기

여러 조건을 조합하여 더 세밀하게 검색할 수 있도록 AND, OR, NOT 논리 연산자를 지원해주고 있습니다.

`AND`  논리 연산자는 '모든 조건을 만족하는 경우 조건식은 참이 된다' 이고,

`OR` 논리 연산자는 '조건 중 하나만 만족하는 경우 조건식은 참이 된다' 이고,

`NOT` 논리 연산자는 단항 연산자로 조건식의 반대 값을 반환합니다.

<br>

#### 4) 패턴 매칭에 의한 검색

`LIKE` 술어를 사용하면 문자열의 일부분을 비교하는 '부분 검색' 을 할 수 있습니다.

`=` 연산자를 이용하면 열 값이 완전히 일치할 때 참이 되지만, `LIKE` 술어를 사용하면 부분적으로 일치하는 경우에도 참이 됩니다.

```sql
열명 LIKE '패턴'
```

방식으로 사용합니다. 패턴을 지정할 때는 `% _` 메타 문자를 사용할 수 있습니다.

`%`는 임의의 문자열을 의미하며, `_`는 임의의 문자 하나를 의미합니다.

<br>

예시를 통해서 익혀보겠습니다.

<img src="https://user-images.githubusercontent.com/59816811/113669977-7c330280-96ef-11eb-98a4-a7d2170623b5.png" alt="sql_select" width="500" />

다음과 같은 `sample1` 테이블이 있을 때 여러 쿼리를 날려보겠습니다.

```sql
select * from sample1 where text LIKE '%SQL%';
```

모든 4개의 행이 결과로 select 됩니다.

```sql
select * from sample1 where text LIKE 'SQL%';
```

 No 1 / No 4 행이 결과로 select 됩니다.

```sql
select * from sample1 where text LIKE '%SQL';
```

No 3 행이 결과로 select 됩니다.

```sql
select * from sample1 where text LIKE '_SQL_';
```

결과로 select 되는 행이 없습니다.

```sql
select * from sample1 where text LIKE 'SQL_';
```

결과로 select 되는 행이 없습니다.

```sql
select * from sample1 where text LIKE 'SQL__';
```

No 4 행이 결과로 select 됩니다.

```sql
select * from sample1 where text LIKE '_SQL';
```

No 3 행이 결과로 select 됩니다.

> LIKE 로 % 혹은 _ 를 검색해야하는 경우
>
> '\%' 와 같이 \ 을 % 앞에 붙이면 됩니다.
>
> '\ _' 와 같이 \ 을 _ 앞에 붙이면 됩니다. 

<br>



