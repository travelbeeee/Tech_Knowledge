# Weighted Union Find

Union-Find 를 정리하며 Union 함수 최적화를 위해 Size 배열을 이용했습니다.

하지만, Weighted Union Find 알고리즘을 이용하면 Size 배열 없이 Union 함수를 최적화 할 수 있습니다.

### Weighted-Union-Find

parent 배열 정의를 살짝 바꿔서 트리의 크기 정보까지 저장해보겠습니다.

![weighted_union_find1](https://user-images.githubusercontent.com/59816811/104554914-4ba77380-5680-11eb-85ca-adbb48be5d85.png)

루트 노드가 아니라면 마찬가지로 부모 노드의 번호를 가지고 있지만, 루트 노드라면 자기가 속한 집합의 크기에 - 를 붙인 음수값을 저장하겠습니다.

즉, 처음에는 모든 노드가 자기 자신만 속한 집합이므로 -1 값을 가지게 됩니다.

```c++
int find(int num) {
	if (p[num] < 0)
		return num;
	return p[num] = find(p[num]);
}

void merge(int num1, int num2) {
	int pNum1 = find(num1), pNum2 = find(num2);
	if (pNum1 == pNum2)
		return;

	if (p[pNum1] <= p[pNum2]) {
		p[pNum1] = p[pNum1] + p[pNum2];
		p[pNum2] = pNum1;
	}
	else{
		p[pNum2] = p[pNum1] + p[pNum2];
		p[pNum1] = pNum2;
	}
	return;
}
```

그러면 find 함수는 위와 같이 음수가 나올 때까지 찾아가면 됩니다.

또, merge 함수는 parent 배열에 -(집합의 크기) 가 담겨있으므로 집합의 크기를 바로 알 수 있고, 이에 따라 크기가 작은 집합을 큰 집합에 합집합 Union 시켜주면 됩니다.

<br>

parent 배열의 정의를 살짝 바꿔서 추가 메모리 없이 Find, Union 함수를 최적화 할 수 있습니다. 