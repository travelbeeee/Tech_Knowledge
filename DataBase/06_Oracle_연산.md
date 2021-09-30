# 06_Oracle_연산

#### 1) ROUND 함수

ROUND 함수는 반올림을 함수입니다. 매개변수가 없으면 소수점 첫째자리에서 반올림하고, 반올림 자릿수를 지정할 수도 있습니다.

```sql
SELECT ROUND(price) from 테이블; // 소수점 첫째자리에서 반올림
SELECT ROUNT(price, 1) from 테이블; // 소수점 둘째자리에서 반올림!
SELECT ROUNT(price, -1) from 테이블; // 일의 자리에서 반올림
```

<br>

#### 2) CONCAT 함수

문자열을 결합하는 함수로 Oracle 에서는 `||` 연산자를 이용합니다.

```sql
'ABC' || '1234' --> 'ABC1234'
```

<br>

#### 3) SUBSTR 함수

SUBSTR 함수는 문자단위로 시작위치와 자를 길이를 지정하며 문자열을 자릅니다.

```sql
SUBSTR("문자열, "시작위치", "길이"); --> 문자열의 시작위치부터 길이만큼 잘라서 반환
```

<br>

#### 4) SUBSTRB 함수

SUBSTRB 함수는 바이트 단위로 문장려을 자를때 사용합니다. 한글 같은 경우 문자단위로 자를 때 깨지는 경우가 있어서 등장한 함수다.

```sql
SUBSTRB("문자열", "시작위치", "길이"); --> 문자열이ㅡ 시작위치부터 길이만큼 바이트를 잘라서 반환
```

<br>

#### 5) CURRENT_TIMESTAMP 함수

CURRENT_TIMESTAMP 함수는 현재 시간을 연/월/일/시간/분/초/밀리초 까지 반환합니다.

```sql
SELECT CURRENT_TIMESTAMP from dual;
```

<br>

#### 6) SYSDATE 함수

SYSDATE 함수는 현재 시간을 연/월/일 까지 반환합니다.

```sql
select sysdate from dual;
```

