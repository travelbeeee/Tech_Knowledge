# Join_종류

 서로 다른 테이블을 연결하는 Join의 5가지 종류에 대해서 정리해보겠습니다.

### 1) Cross Join

 `Cross Join`은 `Cartesian Product`를 의미합니다. 즉, A테이블의 모든 행와 B테이블의 모든 행을 서로 연결시키는 `Join`으로 A테이블의 행이 M개, B테이블의 행이 N개라면 M * N 개의 행이 생기게 됩니다.

![cross](https://user-images.githubusercontent.com/59816811/136323820-b42fb8be-9d0f-4d02-b504-678dfb9ccc89.png)



### 2) Inner Join

 `Inner Join`은 교집합 연산과 같습니다. `Join Key` 컬럼 값이 양쪽 테이블 데이터 집합에서 공통적으로 존재하는 데이터만 조인해서 결과 데이터 집합으로 추출하게 됩니다.

![inner](https://user-images.githubusercontent.com/59816811/136323824-ca122b60-0b37-4447-a16e-174fe8426de8.png)

### 3) Left Outer Join

 `Left Outer Join`은 교집합 연산 결과와 차집합 연산 결과를 합친 것과 같습니다. `Left` 라는 이름 그대로 왼쪽 테이블에만 존재하는 데이터도 결과 데이터 집합으로 추출하고, `Inner Join`의 결과인 교집합 데이터도 결과 데이터 집합으로 추출합니다.

![left](https://user-images.githubusercontent.com/59816811/136323825-5336e9c8-9eb9-43b1-9221-f44e27a4b283.png)

### 4) Right Outer Join

 `Right Outer Join`도 `Left Outer Join`과 동일하게 교집합 연산 결과와 차집합 연산 결과를 합친 것과 같습니다. `Right` 라는 이름 그대로 왼쪽 테이블이 아닌, 오른쪽 테이블에만 존재하는 데이터도 결과 데이터 집합으로 추출하고, `Inner Join`의 결과인 교집합 데이터도 결과 데이터 집합으로 추출합니다.

![right](https://user-images.githubusercontent.com/59816811/136323826-5e10f51a-dd1e-4b0f-bd21-322fc755f229.png)

### 5) Full Outer Join

 `Full Outer Join`은 `Left Outer Join`과 `Right Outer Join`을 합친 것으로 합집합 연산과 같습니다. 조인 키 컬럼 값이 양쪽 테이블에 존재하는 데이터와 한 쪽 테이블에만 존재하는 데이터들을 모두 추출하게 됩니다.

 오라클에서는 `Full Outer Join`을 지원하지만, MySQL에서는 `Full Outer Join`을 지원하지 않는다. `Left Outer Join` 결과와 `Right Outer Join` 결과를 `Union`해서 구할 수 있다.

![full](https://user-images.githubusercontent.com/59816811/136323827-1e8ed06d-e167-4772-b3fe-eafdfae955e2.png)

