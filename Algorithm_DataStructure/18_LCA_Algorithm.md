# LCA 알고리즘

LCA 알고리즘은 Lowest Common  Ancestor의 약자로 트리에서 두 정점에서 가장 가까운 공통 조상을 찾는 알고리즘입니다. 그래프 탐색을 이용한 방법과Dynamic Programming을 이용한 방법 2가지를 정리해보겠습니다.

<br>

#### 1) 가장 가까운 공통 조상

먼저, 가장 가까운 공통 조상이 무엇인지부터 정의해보겠습니다.

- 조상 : 자기 자신을 포함해 모든 부모 노드를 의미
- 공통 조상 : 어떤 노드 x와 어떤 노드 y의 모든 조상 노드 중 공통된 노드
- 가장 가까운 공통 조상 : 어떤 노드 x와 어떤 노드 y의 공통 조상 노드 중 가장 가까운 노드

정의만 보면 우리가 알고있는 조상의 의미와 똑같아서 어려운게 없습니다. 다만, 자기 자신을 포함한다는 것만 주의하시면 됩니다.

<img src="https://user-images.githubusercontent.com/59816811/106975397-cf451380-6799-11eb-83f4-67a879410469.png" alt="LCA01" width="500" />

그림과 같은 트리가 있다고 해보겠습니다.

<img src="https://user-images.githubusercontent.com/59816811/106975395-ceac7d00-6799-11eb-9015-eb1a8f282bed.png" alt="LCA02" width="500" />



**Q . 노드 3번과 노드 10번의 공통 조상은 누구일까요??**

노드 3번의 조상은 1, 3, 5번 노드가 입니다.

노드 10번의 조상은 1,5,3,8, 10번 노드입니다.

따라서, 공통 조상은 1, 3, 5번 노드가 되고 가장 가까운 공통 조상은 3번 노드가 됩니다.

<img src="https://user-images.githubusercontent.com/59816811/106975386-cc4a2300-6799-11eb-9c02-675169e213fd.png" alt="LCA03" width="500" />



##### Q. 2번 노드와 9번 노드의 공통 조상은 몇 번 노드일까요?

공통 조상은 1, 5번 노드가 되고, 가장 가까운 공통 조상은 5번 노드가 됩니다.

<br>

#### 2) 그래프 탐색을 이용한 LCA

트리도 그래프의 일종이므로 그래프 탐색을 이용해서 가장 가까운 공통 조상을 찾을 수 있습니다.

<img src="https://user-images.githubusercontent.com/59816811/106975386-cc4a2300-6799-11eb-9c02-675169e213fd.png" alt="LCA03" width="500" />

2번 노드와 9번 노드의 가장 가까운 공통 조상을 그래프 탐색을 이용해서 찾아보겠습니다.

**1) 2번 노드와 9번 노드 비교**

<img src="https://user-images.githubusercontent.com/59816811/106976350-95750c80-679b-11eb-8236-537ad5267dac.png" alt="lca04" width="300" />

9번 노드가 깊이가 더 깊으므로 부모 노드를 찾아서 탐색을 진행한다.

<br>

##### **2) 2번 노드와 10번 노드 비교**

<img src="https://user-images.githubusercontent.com/59816811/106976356-973ed000-679b-11eb-887d-955875b6c8f7.png" alt="lca05" width="300"/>

10번 노드가 깊이가 더 깊으므로 부모 노드를 찾아서 탐색을 진행한다.

<br>

**3) 2번 노드와 8번 노드 비교**

<img src="https://user-images.githubusercontent.com/59816811/106976355-96a63980-679b-11eb-9c74-6414eeb1d8c3.png" alt="lca06" width="300"/>

8번 노드가 깊이가 더 깊으므로 부모 노드를 찾아서 탐색을 진행

<br>

**4) 2번 노드와 3번 노드 비교**

<img src="https://user-images.githubusercontent.com/59816811/106976354-96a63980-679b-11eb-9fc1-2be31e64d1c1.png" alt="lca07" width="300" />

깊이가 같으므로 2번 노드가 부모 노드를 찾아서 탐색 진행

<br>

**5) 5번 노드와 3번 노드 비교**

<img src="https://user-images.githubusercontent.com/59816811/106976353-960da300-679b-11eb-940f-36d5e0c84a75.png" alt="lca08" width="300"/>

3번 노드가 깊이가 더 깊으므로 부모 노드를 찾아서 탐색을 진행

두 노드가 같아졌으므로 최소 공통 조상은 5번 노드이고, 5번 노드부터 부모 노드를 쭉 탐색해나간다면 조상 노드를 모두 찾을 수 있습니다.

<br>

이처럼 우리는 노드의 깊이와 부모 정보만 있다면 그래프 탐색을 이용해서 공통 조상을 찾을 때까지 탐색을 진행할 수 있습니다.

<br>

#### 3) 코드 및 시간 복잡도

**[ 코드 C++ ]**

```c++
// x와 y 노드의 공통 조상 찾기
while (x != y) {
    if (depth[x] > depth[y])
        x = parent[x];
    else
        y = parent[y];
}
```

코드로 표현하면 되게 간단합니다. 두 노드가 같아질 때까지 깊이를 비교해 더 깊은 노드를 계속 부모 노드로 올려주며 탐색을 진행하면 됩니다. 

<br>

그러면 이 알고리즘의 복잡도는 어떻게 될까요??

**N개의 노드가 있다고 가정했을 때, N개의 노드를 하나하나 탐색을 진행하므로 O(N)의 복잡도를 가지는 것을 알 수 있습니다.**

<br>

#### **4) Dynamic Programming을 이용한 LCA 알고리즘**

DP 알고리즘을 이용하면 훨씬 더 빠르게 LCA를 찾을 수 있습니다. 하나 하나 부모를 찾아서 올라가는 것이 아니라 2^K 꼴로 부모를 찾아서 이동을 진행 합니다.

- DP[노드번호] [K] 

  : 해당 노드의 2^K번 째 부모 노드

DP배열의 정의는 다음과 같습니다. DP배열을 이용하면 어떻게 탐색 횟수를 줄일 수 있는지 아래 예시를 통해서 보겠습니다.

<img src="https://user-images.githubusercontent.com/59816811/106978422-990a9280-679f-11eb-9c05-3d42e8b2c91f.png" alt="lca09" width="500" />

똑같이 2번 노드와 9번 노드의 공통 조상을 찾겠습니다.

먼저, 2번 노드와 9번 노드의 깊이가 같아질때까지 9번 노드의 부모를 탐색해서 올려보겠습니다.

**1) 2번 노드와 9번 노드 비교**

9번 노드가 깊이가 더 깊고, DP[9] [2] = 5번 노드는 2번 노드랑 깊이 얕으므로 넘어가고, DP[9] [1] = 8번 노드는 2번 노드보다 깊이가 깊으므로 9번 노드를 한 번에 8번 노드로 올려줍니다.

<img src="https://user-images.githubusercontent.com/59816811/106978873-7c228f00-67a0-11eb-9632-2455a6692d6d.png" alt="lca10" width="500" />

<br>

**2) 2번 노드와 8번 노드 비교**

8번 노드가 깊이가 더 깊고, Dp[8] [1] = 5번 노드는 2번 노드보다 깊이가 얕으므로 넘어가고, Dp[8] [0] = 3번 노드는 2번 노드와 깊이가 같으므로 8번 노드를 3번 노드로 옮겨줍니다.

<img src="https://user-images.githubusercontent.com/59816811/106978869-7b89f880-67a0-11eb-874e-3b3906877dde.png" alt="lca11" width="500" />

<br>

**3) 2번 노드와 3번 노드 비교**

DP[2] [1] = DP[3] [1] = 1번 노드이므로 1번 노드는 공통 조상입니다.

DP[2] [0] = DP[3] [0] = 5번 노드이므로 5번 노드도 공통 조상입니다.

따라서, 가장 가까운 공통 조상은 5번 노드가 됩니다.

<img src="https://user-images.githubusercontent.com/59816811/106978868-7af16200-67a0-11eb-81a3-2bea67f59587.png" alt="lca12" width="500" />

<br>

이치럼 DP를 이용한 LCA 알고리즘은 2^K 꼴로 부모를 찾아서 이동하므로 그래프 탐색을 이용한 알고리즘보다 훨씬 효율적입니다.

<br>

#### **5) 코드 및 시간 복잡도**

먼저, DP배열과 깊이를 담고 있는 Depth 배열이 필요합니다.

```c++
// set(root노드번호, 0);

void set(int cur, int parent) {
    // 현재 높이는 부모 높이 + 1
	depth[cur] = depth[parent] + 1;
    // 나중에 사용하기 위해 가장 높은 높이를 따로 저장 
	maxDepth = max(maxDepth, depth[cur]);
    // dp[cur][0] = cur의 2^0번 째 부모이므로 바로 위 부모를 의미
	dp[cur][0] = parent;

	int length = depth[cur] - 1, k = 1, powK = 2;
	while (powK <= length) {
        // dp[cur][K] 는 cur의 2^K번 째 부모
        // 따라서 cur의 2^(k - 1)번 째 부모의 2^(k - 1)번 째 부모랑 동일
		dp[cur][k] = dp[dp[cur][k - 1]][k - 1];
		k++;
		powK *= 2;
	}
    // 자식 노드로 재귀적으로 진행
	for (int next : graph[cur]) {
		if (depth[next]) continue;
		set(next, cur);
	}
}
```

root노드번호부터 시작해서 재귀적으로 구할 수 있습니다. 

dp[cur] [k] := dp[ dp[cur] [k - 1] ] [ k - 1] 이 성립하므로 DP 배열을 채울 수 있습니다.

<br>

이제 필요한 셋팅을 다 진행했으므로 공통 조상을 구하면 됩니다.

```c++
// maxLevel은 Dp[i][k] 에서 K에 들어갈 수 있는 최대값을 의미합니다.
// maxDepth를 따로 구해놓았으므로 maxLevel은 log2 함수를 이용해 쉽게 구할 수 있습니다.
// maxLevel = (int)floor(log2(maxDepth - 1));

int LCA(int x, int y) {
	// 먼저 높이가 더 높은 노드를 올려서 높이를 맞추자
	if (depth[x] != depth[y]) {
		if (depth[x] < depth[y])
			swap(x, y);
		// x를 y랑 같은 높이까지 올려주자 
		for (int i = maxLevel; i >= 0; i--)
			if (depth[y] <= depth[dp[x][i]]) {
				x = dp[x][i];
			}
	}
    // 같은 높이에서 공통 조상을 계속 찾아보자.
	int res = x;
	if (x != y) {
		for (int i = maxLevel; i >= 0; i--) {
			if (dp[x][i] != dp[y][i]) {
				x = dp[x][i];
				y = dp[y][i];
			}
			res = dp[x][i];
		}
	}
	return res;
}

```

**DP 알고리즘을 이용해서 O(N)의 복잡도를 O(logN) 으로 줄일 수 있습니다.**