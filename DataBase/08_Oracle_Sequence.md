# DB_Sequence

Sequence는 이름 그대로 순서를 나타내기 위한 객체입니다. 오라클에서 제공하는 객체로 자동으로 순차적으로 증가하는 순번을 반환합니다. 보통 PK 값을 할당하기 위해 사용합니다.



##### 시퀀스 생성

```sql
create sequence [시퀀스명] 
start with [시작숫자] 
increment by [증감숫자]
minvalue [최소값]
maxvalue [최대값]
cycle / nocycle -- cycle 설정시 최대값에 도달하면 다시 시작숫자부터 시작
cache / nocache -- cache 설정시 메모리에 시퀀스 값을 미리 할당.
```



##### 시퀀스 조회

```sql
select ex_seq.currval from dual; -- 해당 시퀀스 값 조회
select * from ex_seq; -- 전체 시퀀스 조회
```



##### 시퀀스 수정

```sql
ALTER SEQUENCE [시퀀스명]
INCREMENT BY [증가값]
NOMINVALUE OR MINVALUE [최소값] 
NOMAXVALUE OR MAXVALUE [최대값]
CYCLE OR NOCYCLE [사이클 설정 여부]
CACHE OR NOCACHE [캐시 설정 여부]
```



##### 시퀀스 삭제

```sql
drop sequence [시퀀스명]
```

