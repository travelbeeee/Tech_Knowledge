# Bubble Sort

서로 인접한 두 원소를 비교하여 크기가 순서대로 되어 있지 않으면 서로 교환 해나가는 정렬 알고리즘.

보통 앞에서부터 차례대로 인접한 두 원소를 비교해나아가며 뒤에서부터 정렬된다.

#### 코드 구현

```c++
void bubble_sort(int list[], int n) {
	int temp;
	for (int i = n - 1; i > 0; i--) {
		for (int j = 0; j < i; j++) {
			// 정렬되어있지않다면 SWAP 한다.
			if (list[j] > list[j + 1]) {
				temp = list[j];
				list[j] = list[j + 1];
				list[j + 1] = temp;
			}
		}
	}
}
```

<br>

#### 시간 복잡도

N 개의 원소가 있다고 가정하고 버블 정렬의 복잡도를 분석해보겠습니다.

버블 정렬은 처음에 ( N - 1 ) 번의 비교 연산이 발생하고, 선택 정렬과 마찬가지로 정렬이 완성된 부분이 하나씩 늘어가며 비교 연산이 1회씩 감소합니다.

**따라서, 총 ( N - 1 ) + ( N - 2 ) + ⋯ + 1 번 비교 연산이 발생합니다.**

**등차수열의 합으로 O ( N ^ 2 ) 의 시간 복잡도를 가지는 것을 확인할 수 있습니다.**

**배열이 이미 정렬이 되어있더라도 항상 똑같은 비교 과정을 거치므로 최선, 평균, 최악의 경우에 대해서 시간 복잡도가 동일합니다.**

<br>

#### 예시

[ 2 3 5 1 4 ]  를 정렬해보자.

##### Step 1)

<img src="https://user-images.githubusercontent.com/59816811/104244988-d59be480-54a6-11eb-8891-35e306d92d11.png" alt="1" width="500" />

##### Step 2)

<img src="https://user-images.githubusercontent.com/59816811/104244993-d6cd1180-54a6-11eb-987b-0a1b31f25b4d.png" alt="2" width="500"/>

##### Step 3)

<img src="https://user-images.githubusercontent.com/59816811/104244995-d765a800-54a6-11eb-809b-04aa57f9b11f.png" alt="3" width="500" />

##### Step 4)

<img src="https://user-images.githubusercontent.com/59816811/104244997-d7fe3e80-54a6-11eb-97de-5e184a95a2d4.png" alt="4" width="500" />