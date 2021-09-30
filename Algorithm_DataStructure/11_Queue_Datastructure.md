# 큐 Queue

### 큐

자료구조 큐에 대해서 정리해보겠습니다.

큐는 '줄서기'를 생각하면 쉽게 이해할 수 있습니다. 먼저 온 사람이 먼저 볼일을 볼 수 있습니다.

즉, 먼저 들어온 데이터들이 먼저 삭제되는 구조입니다.

<img src="https://user-images.githubusercontent.com/59816811/105168006-d561b000-5b5c-11eb-90ea-c151545481f9.png" alt="queue_img1" width="800" />



큐는 보통 앞 쪽에서 삭제(Pop)이 이루어지고, 뒤에서는 추가(Push)가 이루어진다고 표현합니다.

또, 앞 쪽은 front, 뒤 쪽을 back 이라고 표현합니다.

<br>

### 큐 구현 (C++)

C++은 Standard Library 에서 자료 구조 큐를 지원해주고 있습니다.

```c++
#include<queue>
```

queue 라이브러리를 불러와서 쉽게 사용할 수 있습니다.

```c++
// 선언
queue<자료형> q;

q.push(element); // 큐의 뒤쪽에 element를 추가한다.
q.pop(); // 큐의 앞쪽에 있는 원소를 삭제, 반환 값은 없다.

q.front(); // 큐 제일 앞에 있는 원소를 반환한다.
q.back(); // 큐 제일 뒤에 있는 원소를 반환한다.

q.empty(); // 큐가 비어있으면 true, 아니면 false 반환
q.size(); // 큐에 원소가 몇 개 들어있는지 반환한다. (const int)
```

<br>

### 큐 구현 (Java)

Java에서는 LinkedList를 활용하여 큐를 생성해야합니다.

> 참고! LinkedList 가 Deque 를 상속하고 있고, Deque는 Queue를 상속하고 있습니다.

```java
import java.util.LinkedList;
import java.util.Queue;
```

사용 방법은 다음과 같습니다.

```java
// 선언
Queue<자료형> q = new LinkedList<>();

// 큐의 뒤쪽에 element를 추가합니다.
// 성공하면 true를 반환하고 큐 메모리 초과로 실패하면 IleegalStateException 을 발생시킵니다.
boolean add(E e); 

// 큐의 뒤쪽에 element를 추가합니다.
// 성공하면 true를 반환하고 실패하면 false를 반환합니다.
boolean offer(E e); 

// 큐의 가장 앞쪽에 있는 element를 삭제하고 반환해줍니다.
// queue가 비어있으면 NoSuchElementException을 발생시킵니다.
E remove();

// 큐의 가장 앞쪽에 있는 element를 삭제하고 반환해줍니다.
// queue가 비어있으면 Null을 반환해줍니다.
E poll();

// 삭제 없이 큐의 가장 앞쪽에 있는 element를 반환해준다.
// queue가 비어있으면 NoSuchElementException을 발생시킵니다.
E element();

// 삭제 없이 큐의 가장 앞쪽에 있는 element를 반환해준다.
// queue가 비어있으면 Null을 반환해줍니다.
E peek();
```

