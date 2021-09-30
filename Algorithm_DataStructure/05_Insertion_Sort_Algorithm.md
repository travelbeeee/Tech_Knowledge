# Insertion Sort

자료 배열의 모든 요소를 앞에서부터 차례대로 이미 정렬된 배열 부분과 비교하여, 자신의 위치를 찾아 삽입함으로써 정렬을 완성하는 알고리즘.

<br>

#### 코드 구현

```c++
void insertion_sort ( int list[], int n ){
  int temp;
  for ( int i = 1; i < n; i++ )
  {
    temp = list[i];
    int j = i - 1;
    while(j >= 0 && temp < list[j]){
    	list[j + 1] = list[j];
        j--;
    }
    list[j + 1] = temp;
  }
}
```

<br>

#### 시간 복잡도

N개의 원소가 있다고 가정하고 삽입 정렬의 복잡도를 분석해보겠습니다.

삽입 정렬은 버블 정렬, 선택 정렬과 다르게 **최선의 경우, 최악의 경우 시간 복잡도가 다릅니다.**

 

먼저, 최선의 경우(이미 배열이 정렬되어 있는 경우)에 대해서 생각해보겠습니다.

이미 자기 위치에 있는 원소는 옆에 있는 원소와 **비교 연산 1회만 진행**한 후 다음 STEP 으로 넘어가게 됩니다.

즉, 최선의 경우 N - 1 개의 원소에 대해서 비교 연산을 1번씩만 진행하면 되므로 O( N )의 복잡도를 가지게 됩니다.

 

그다음으로는 최악의 경우(배열이 역순으로 정렬되어 있는 경우)에 대해서 생각해보겠습니다.

**모든 STEP마다 정렬되어 있는 배열의 모든 원소들과 비교 연산을 진행해야 하므로**

**총 ( N - 1 ) + ( N - 2 ) + ⋯ + 1 번 비교 연산이 발생합니다.**

**등차수열의 합으로 O ( N ^ 2 ) 의 시간 복잡도를 가지는 것을 확인할 수 있습니다.**

<br>

#### 예시

[ 3 1 2 5 4 8 7 6] 을 정렬해보자.

<img src="https://user-images.githubusercontent.com/59816811/104246447-5eb41b00-54a9-11eb-9d34-3e04aaf4e5d2.png" alt="1" width="500" />
<img src="https://user-images.githubusercontent.com/59816811/104246451-5fe54800-54a9-11eb-86fd-54bb2a671ba3.png" alt="2" width="500" />
<img src="https://user-images.githubusercontent.com/59816811/104246454-607dde80-54a9-11eb-8c1f-c5b499590bb3.png" alt="3" width="500" />
<img src="https://user-images.githubusercontent.com/59816811/104246456-61167500-54a9-11eb-9a30-b7933e4b80b4.png" alt="4" width="500" />
<img src="https://user-images.githubusercontent.com/59816811/104246458-6247a200-54a9-11eb-9cec-a04ada314c3f.png" alt="5" width="500" />
<img src="https://user-images.githubusercontent.com/59816811/104246461-62e03880-54a9-11eb-8790-c45298cc1261.png" alt="6" width="500" />
<img src="https://user-images.githubusercontent.com/59816811/104246466-65429280-54a9-11eb-92a2-6c95fe0636c2.png" alt="7" width="500" />

