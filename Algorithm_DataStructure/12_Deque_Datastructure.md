# 덱 Deque 

### 덱

자료구조 덱에 대해서 정리해보겠습니다.

덱은 Double-Ended Queue의 약자로 이름 그대로 양쪽이 모두 Queue인 자료구조입니다. 즉, 양쪽에서 삽입, 삭제가 다 이루어질 수 있는 자료구조입니다.

<img src="https://user-images.githubusercontent.com/59816811/105279101-32f00e00-5bea-11eb-90a4-c5d0279d32a4.jpg" alt="deque_1" width="800" />

<br>

### 덱 구현(C++)

C++ Standard Library에서 자료 구조 덱을 지원해주고 있습니다.

```c++
#include<deque>
```

deque 라이브러리를 이용하면 됩니다.

```c++
// 선언
deque<자료형> dq;

push_front(element); // 덱의 제일 앞에 element를 추가합니다.
push_back(element); // 덱의 제일 뒤에 element를 추가합니다.

pop_front(); // 덱의 제일 앞 element를 삭제합니다. 반환 값은 없습니다.
pop_back(); // 덱의 제일 뒤 element를 삭제합니다. 반환 값은 업습니다.

front(); // 덱의 제일 앞에 있는 원소를 반환합니다.
back(); // 덱의 제일 뒤에 있는 원소를 반환합니다.

empty(); // 덱이 비어 있으면 true, 아니면 false를 반환합니다.
size(); // 덱에 들어있는 원소의 개수를 반환합니다. ( const int 형 )
```

<br>

### 덱 구현(Java)

Java util 패키지에서 Deque 클래스를 지원해주고 있습니다.

```java
import java.util.ArrayDeque;
import java.util.Deque;
```

Deque는 인터페이스이기 때문에 구현체인 ArrayDeque를 이용합니다.

```java
// 선언
Deque<자료형> dq = new ArrayDeque<>();

void addFirst(E e); // element를 Deque의 맨 앞에 추가한다. 메모리가 부족하다면 IllegalStateException 을 발생시킨다.
void addLast(E e); // element를 Deque의 맨 뒤에 추가한다. 메모리가 부족하면 IllegalStateException 을 발생시킨다.

boolean offerFirst(E e); // element를 Deque의 맨 앞에 추가한다. 삽입에 실패하면 false를 반환한다.
boolean offerLast(E e); // element를 Deque의 맨 뒤에 추가한다. 삽입에 실패하면 false를 반환한다.

E removeFirst(); // Deque의 맨 앞의 원소를 삭제하고 반환한다. Deque이 비어있다면 NoSuchElementException 을 발생시킨다.
E removeLast(); // Deque의 맨 뒤의 원소를 삭제하고 반환한다. Deque이 비어있다면 NoSuchElementException 을 발생시킨다.
E pollFirst(); // Deque의 맨 앞의 원소를 삭제하고 반환한다. Deque이 비어있다면 null을 반환한다.
E pollLast(); // Deque의 맨 뒤의 원소를 삭제하고 반환한다. Deque이 비어있다면 null을 반환한다.

E getFirst(); // Deque의 맨 앞의 원소를 반환한다. Deque이 비어있다면 NoSuchElementException 발생.
E getLast(); // Deque의 맨 뒤의 원소를 반환. Deque이 비어있다면 NoSuchElementException 발생.
E peekFirst(); // Deque의 맨 앞의 원소를 반환. Deque이 비어있다면 null 반환.
E peekLast(); // Deque의 맨 뒤의 원소를 반환. Deque이 비어있다면 null 반환.

boolean contains(Object o ); // Deque에 Objects.equals(o, e) 를 만족하는 e가 있다면 true 반환.

int size(); // Deque에 들어있는 원소의 개수 반환
boolean isEmpty(); // Deque가 비어있으면 true 아니면 false
```

