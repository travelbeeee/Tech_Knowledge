# Quick Sort

퀵 정렬은 리처드 호어가 개발한 정렬 알고리즘으로 **평균적으로** 앞에서 다룬 Selection, Bubble, Insertion Sort 알고리즘보다 **빠른 수행 속도**를 자랑한다.

전체 리스트를 2개의 부분 리스트로 분할하고, 각각의 부분 리스트를 다시 퀵 정렬하는 **분할 정복 방법**을 사용한다.

먼저, 리스트 안에 한 요소를 피벗(Pivot)으로 선택하고 피벗을 기준으로 피벗보다 작은 요소들은 피벗의 좌측, 큰 요소들은 피벅의 우측으로 옮겨서 피벗을 기준으로 2개의 부분 리스트로 분할한다. 분할된 리스트도 마찬가지로 퀵 정렬을 재귀적으로 수행한다. ( 우리는 리스트의 맨 앞 요소를 피벗으로 선택하겠다. )

<br>

#### 코드 구현

```c++
int partition(int list[], int left, int right) {
	int pivot, temp, low, high;
	low = left;
	high = right + 1;
	pivot = list[left];
	do {
		do {
			low++;
		} while (low <= right && list[low] < pivot);
		do {
			high--;
		} while (high >= left && list[high] > pivot);
		if (low < high) { //SWAP진행.
			temp = list[low];
			list[low] = list[high];
			list[high] = temp;
		}
	} while (low < high);

	temp = list[left];
	list[left] = list[high];
	list[high] = temp;
	return high;
}

void quick_sort(int list[], int left, int right) {
	if (left < right) {
		int q = partition(list, left, right);
		quick_sort(list, left, q - 1);
		quick_sort(list, q + 1, right);
	}
}
```

<br>

#### 시간 복잡도

퀵 정렬은 평균적으로 빠른 정렬 알고리즘이다. 이는 리스트 상태에 따라 퀵 정렬의 속도가 크게 영향받기 때문이다. 피벗을 기준으로 분할할 때 아래 그림과 같이 **분할 리스트들의 크기가 반으로 줄어든다면 log(n) 번 분할이 발생하고, 각 분할마다 평균 n번 정도의 비교가 이루어지므로 O ( nlog(n) ) 시간 복잡도**를 가지게 된다.

<img src="https://user-images.githubusercontent.com/59816811/104382061-d305c180-5570-11eb-8e66-0d283b15952a.png" alt="1" width="700" />

하지만, 아래 그림과 **같이 불균형하게 나누어지는 경우에는 최대 n 번의 분할이 발생하고, 각 분할마다 평균 n번 정도의 비교가 이루어지므로 O ( n^2) 의 시간 복잡도**를 가지게 된다

<img src="https://user-images.githubusercontent.com/59816811/104382062-d4cf8500-5570-11eb-93ec-eddbba830354.png" alt="2" width="600" />

<br>

#### 예시

[ 5 3 8 4 9 1 6 2 7 ] 이 나열되어있는 리스트에서 퀵 정렬을 진행해보자.

<img src="https://user-images.githubusercontent.com/59816811/104382339-48719200-5571-11eb-9e90-f2497984fef7.png" alt="3" width="600"/>

<img src="https://user-images.githubusercontent.com/59816811/104382343-49a2bf00-5571-11eb-8578-1d06d10e9a9f.png" alt="4" width = "600"/>



<img src="https://user-images.githubusercontent.com/59816811/104382345-4ad3ec00-5571-11eb-8c83-724a14466d97.png" alt="5" width="600" />

<img src="https://user-images.githubusercontent.com/59816811/104382351-4c051900-5571-11eb-9d6b-ad5e50552f64.png" alt="6" width = "600"/>

최종적으로 각 분할리스트들이 모두 퀵 정렬이 수행되고, 전체 리스트가 정렬이 된다.

<img src="https://user-images.githubusercontent.com/59816811/104382354-4d364600-5571-11eb-8267-a7481fc9864a.png" alt="7" width="600"/>