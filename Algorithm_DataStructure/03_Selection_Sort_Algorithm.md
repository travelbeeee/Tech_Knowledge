# Selection Sort

여러 가지 정렬 알고리즘 중 하나로 **이름 그대로 정렬되지 않은 데이터에서 가장 작은 값을 뽑아 가장 앞의 데이터와 교환 해나가는 정렬 알고리즘** 

<br>

#### 코드 구현

```c++
void SelectionSort(int arr[], int N) {
    int min, temp;
    for(int i = 0; i < N - 1; i++) {
        min = i; // min 에 unsorted 중 가장 작은 값을 저장한다.
        for(int j = i+1; j < N; j++) 
            if(arr[j] < arr[min]) min = j;
        
        // 가장 앞과 SWAP 해준다.
        temp = arr[i];
        arr[i] = arr[min];
        arr[min] = temp;
    }
}
```

<br>

#### 시간 복잡도

크기가 N인 리스트라고 했을 때, 정렬되지 않은 부분을 순회하며 가장 작은 값을 찾고, 이를 N번 반복하므로 **O(N^2)의 시간 복잡도**를 가진다.

<br>

첫 번째 원소를 정렬하기 위해 N개의 모든 원소를 순회하며 가장 작은 값을 찾아야 합니다. 

즉, N - 1 번의 비교 연산이 발생합니다. 

두 번째 원소를 정렬하기 위해서는 ( N - 1 )개의 모든 원소를 순회하며 가장 작은 값을 찾아야 합니다.

즉, N - 2 번의 비교 연산이 발생합니다.

그다음은 ( N - 2 ) 개의 모든 원소를 순회하며 가장 작은 값을 찾아야 하고

**이 과정을 반복하면 총 (N - 1) + ( N - 2 ) + ⋯ + 1 번 비교 연산이 발생합니다.**

**등차수열의 합으로 O ( N ^ 2 ) 의 시간 복잡도를 가지는 것을 확인할 수 있습니다.**

**배열이 이미 정렬이 되어있더라도 가장 작은 값을 찾기 위해 항상 똑같은 순회, 비교 과정을 거치므로**

**최선, 평균, 최악의 경우에 대해서 시간 복잡도가 동일하게 O ( N ^ 2 )이 됩니다.**

<br>

#### 예시

다음과 같이 [ 2 8 6 1 5 3 4 7 ] 이 담겨있는 배열을 선택 정렬을 이용해서 정렬해보겠다.

##### 1)

아무것도 정렬되지 않았으므로 정렬되지 않은 전체 데이터에서 가장 작은 값인 1을 뽑아서 가장 앞의 데이터와 교환한다.

<img src="https://user-images.githubusercontent.com/59816811/104181580-6a282780-5452-11eb-973f-ef76d57d6402.png" alt="1" width="500" />

<br>

##### 2)

정렬되지 않은 부분에서 가장 작은 값인 2를 뽑아 가장 앞의 데이터와 교환한다.

<img src="https://user-images.githubusercontent.com/59816811/104181583-6bf1eb00-5452-11eb-8800-3fe792ace6e0.png" alt="2" width="500" />

##### 3)

같은 과정을 반복한다.

<img src="https://user-images.githubusercontent.com/59816811/104181585-6d231800-5452-11eb-9aba-e004b1d8502b.png" alt="3" width="500" />

##### 4)

<img src="https://user-images.githubusercontent.com/59816811/104181587-6e544500-5452-11eb-8639-b4153f50db97.png" alt="4" width="500" />

##### 5)

<img src="https://user-images.githubusercontent.com/59816811/104181596-6f857200-5452-11eb-86df-7873333cdaaa.png" alt="5" width="500" />

##### 6)

<img src="https://user-images.githubusercontent.com/59816811/104181603-71e7cc00-5452-11eb-87ed-70583c0c2bfe.png" alt="6" width="500" />

##### 7)

<img src="https://user-images.githubusercontent.com/59816811/104181609-72806280-5452-11eb-80b8-79191d925f94.png" alt="7" width="500" />

##### 8)

<img src="https://user-images.githubusercontent.com/59816811/104181612-73b18f80-5452-11eb-89f0-4bd77b70747a.png" alt="8" width="500" />

