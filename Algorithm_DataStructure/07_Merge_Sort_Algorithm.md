# Merge Sort

Merge Sort 는 존 폰 노이만이 개발한 정렬 알고리즘이다.

하나의 리스트를 두 개의 **균등한 크기로 분할하고** 분할된 부분 리스트를 정렬한 다음, 두 개의 정렬된 부분 리스트를 합하여 전체가 정렬된 리스트가 되게 하는 **분할 정복에** 바탕을 두고 있다. 주어진 리스트를 크기가 1이 될 때까지 분할한 후 각각의 부분 리스트를 정렬하면서 합병해나가 최종적으로 정렬된 리스트를 얻는다.



퀵 정렬과 달리 합병 정렬에서 **실제로 정렬이 이루어지는 시점은 2개의 분할 리스트를 합병하는 단계**이다. 이때, **새로운 리스트를 추가로 필요**로 한다. 합병 알고리즘은 2개의 리스트의 요소들을 처음부터 하나씩 비교하여 두 개의 리스트의 요소 중에서 더 작은 요소를 새로운 리스트로 옮깁니다. 둘 중에서 하나가 끝날 때까지 이 과정을 되풀이하고 만약 둘 중에서 하나의 리스트 순회가 먼저 끝나게 되면 나머지 리스트의 요소들을 전부 새로운 리스트로 복사해 정렬된 합병 리스트를 얻습니다.

<br>

#### 코드 구현

```c++
int sorted[MAX_SIZE]; // 새로운 배열 C

void merge(int list[], int left, int mid, int right) {
	int i, j, k, l;
	i = left; j = mid + 1; k = left;

	while (i <= mid && j <= right) {
		if (list[i] <= list[j])
			sorted[k++] = list[i++];
		else
			sorted[k++] = list[j++];
	}
	if (i > mid)
		for (l = j; l <= right; l++)
			sorted[k++] = list[l];
	else
		for (l = i; l <= mid; l++)
			sorted[k++] = list[l];
	// 새로운 배열 C를 다시 원래 배열로 옮긴다.
	for (l = left; l <= right; l++)
		list[l] = sorted[l];
	return;
}

void merge_sort(int list[], int left, int right) {
	int mid = (left + right) / 2;
	if (left < right) {
		merge_sort(list, left, mid);
		merge_sort(list, mid + 1, right);
		merge(list, left, mid, right);
	}
	return;
}
```

merge_sort로 배열을 계속 반으로 분할해주고 분할이 완료되면 merge 함수를 호출해 정렬과 합병을 진행합니다.
<br>

#### 시간 복잡도

 합병 정렬은 절반씩 나눠지며 분할하므로 log(n) 번의 분할 연산이 진행됩니다. 그 후 merge 함수에서 비교 연산과 이동 연산이 수행되는데 **비교 연산은 최대 n 번 발생하고, 이동 연산은 2n번** 발생하는 것을 알 수 있습니다. 즉, **O( nlogn ) 시간 복잡도**를 가지게 됩니다. 

 퀵 정렬과 달리 절반으로 분할되서 최악의 경우에도 O ( nlogn ) 시간 복잡도를 가진다.

 단점으로는 임시 배열이 필요하므로 추가적인 메모리가 요구됩니다. 

<br>

#### 예시

[ 5 3 8 4 1 6 2 7 ] 을 Merge Sort 를 이용해서 정렬해보자.

<img src="https://user-images.githubusercontent.com/59816811/104383482-3d1f6600-5573-11eb-838f-06d7d06867ef.png" alt="merge_sort_1" width = "600"/>

먼저, 리스트를 절반으로 나눠가며 최종적으로 8개의 리스트를 얻는다.

<img src="https://user-images.githubusercontent.com/59816811/104383483-3e509300-5573-11eb-97f4-7a37aeba7335.png" alt="merge_sort_2" width = "600"/>

이를 Merge 하는 과정이다.

<br>