# Backtracking

백트레킹은 한국어로 번역하면 퇴각검색 이라고 한다. 영어 정의를 보는게 더 쉬울 것 같다... 

**Backtracking** is a general algorithm for finding all (or some) solutions to some computational problems, notably constraint satisfaction problems.

notably constraint satisfaction problems 라는 구문만 없으면 백트레킹 알고리즘은 Brute Force 알고리즘과 유사하다. 가능한 모든 경우를 다루는 알고리즘 기법인 것 같다.

<br>

그러면 constraint satisfaction problems 란 무엇일까??

**Constraint satisfaction problems** (**CSPs**) are mathematical questions defined as a set of objects whose state must satisfy a number of constraints or limitations.

<br>

해석해보면 여러 제약 또는 제한을 충족하는 상태의 개체 집합으로 정의된 수학 문제...! 라고 한다.

<br>

**내 식대로 정의를 내려보면 Backtracking 은 Finding all solutions ( 전수조사 ) 를 하는데 여러 조건을 만족하는 경우에만! 경우의 수를 살펴보는 알고리즘이다.**

이때, 보통 재귀적으로 DFS탐색처럼 알고리즘을 구현하고 Brute-Force 와 달리 중간에 조건에 부합하지 않는 경우의 수라고 판단이 되면 다시 Back 해서 다른 경우의 수 탐색으로 넘어가게 된다. 그래서 Backtracking 이라고 이름이 붙여진 것 같다.

<br>

가장 대표적인 N-Queen 예시를 통해서 조금 더 차이를 알아보자.

![20210107_075442](https://user-images.githubusercontent.com/59816811/103828060-a1e34800-50bd-11eb-9d26-221710e2e664.png)

N-Queen 문제는 N x N 체스판 위에 N개의 퀸을 서로 공격할 수 없게 놓는 경우의 수를 구하는 문제이다.  Brute-Force 관점에서 생각해보면 N개의 row 에 각각 N개의 퀸을 1개씩 놓는다고 생각해보면 1개의 퀸마다 N개의 경우의 수가 나온다. 따라서, 시간 복잡도는 **O( N^N )** 이 되고 이 뜻은 Brute-Force 하게 문제를 해결할 수 없다는 뜻이다.

그러면 Backtracking 관점에서 생각해보자.

마찬가지로 N개의 row에 각각 N개의 퀸을 1개씩 놓는다. **하지만, 현재 퀸을 놓을 자리는 다른 퀸들이 공격할 수 없는 자리! 라는 조건을 추가**하는 것이다. 즉, i번 째 row의 j번 째 col에 i번 째 퀸을 놓으려고 한다면 (0 ~ i - 1)번 째 row에 놓여져있는 퀸들의 공격 범위에서 벗어나는지를 먼저 조사하고 괜찮은 자리라면 그 경우의 수를 마저 이어나가보는 것이다.

<br>

이처럼 Brute-Force 와 Backtracking 은 비슷하지만 약간의 차이가 있다.

개인적으로는 Brute-Force 알고리즘을 이용해서 문제를 해결하는데 확실히 버려도 되는 경우의 수는 버리면서 최적화하자! 라는 뜻으로 받아들이고 사용하고 있다. 