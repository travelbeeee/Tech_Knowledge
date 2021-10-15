# 스택 Stack

대표 자료구조인 스택에 대해서 정리해보겠습니다.

먼저, 자료구조는 쉽게 얘기하면 자료(데이터)를 저장하고 관리하는 구조입니다. 그 중, 스택 자료구조는 어떻게 데이터를 담고 있고 어떤 특징이 있는지 알아보겠습니다.

### 스택

스택은 쉽게 얘기하면 쓰레기통을 생각할 수 있습니다. 쓰레기통에 쓰레기를 버리면 쓰레기통의 가장 위에 쌓이게 되고, 쓰레기를 꺼낼 때는 가장 최근에 버린(위에 있는) 쓰레기부터 꺼낼 수 있습니다.

스택도 마찬가지입니다. 스택은 **LIFO ( Last In First Out )** 형식의 자료 구조로 한 쪽 끝에서만 자료를 넣거나 뺄 수 있는 구조입니다. 즉, 마지막에 들어온 자료가 가장 먼저 나가는 자료구조이고, 자료를 넣는 것을 **Push**, 자료를 빼는 것을 **Pop** 이라고 표현합니다.

<img src="https://user-images.githubusercontent.com/59816811/104684056-86baad00-573b-11eb-93ed-4a823e856f77.png" alt="stack1" width="300" height="400" />

<br>

### 스택 구현 (C++)

C++은 Standard Library 에서 자료 구조 스택을 지원해주고 있습니다.

```c++
#include<stack>
```

stack 라이브러리를 불러와서 쉽게 사용할 수 있습니다.

```c++
// 선언
stack<자료형> s; // 자료형 변수들을 담는 s라는 이름의 stack 선언

s.push(element); // 스택의 가장 상단에 원소를 추가
s.pop(); // 스택의 가장 상단에 있는 원소를 제거
s.top(); // 스택의 가장 상단에 있는 원소를 반환
s.empty(); // 스택이 비어있으면 true 아니면 false를 반환
s.size(); // 스택의 크기를 반환(원소 갯수)
```

<br>

### 스택 구현 (Java)

Java는 java.util 패키지에서 stack 클래스를 지원해주고 있습니다.

```java
import java.util.Stack;
```

자주 쓰이는 메소드만 정리해보겠습니다.

```java
// 선언
Stack<자료형> s = new Stack<>(); // 자료형 변수들을 담는 s라는 이름의 stack 선언

public Element push(Element item); // 스택의 가장 상단에 원소를 추가
public Element pop(); // 스택의 가장 상단에 있는 원소를 제거하고 반환
public Element peek(); // 스택의 가장 상단에 있는 원소를 반환
public boolean empty(); // 스택이 비어 있으면 true 아니면 false 반환

 // 스택에 해당 원소가 있으면 찾아서 최상단으로부터의 거리를 반환해주고 없으면 -1을 반환해준다.
public int search(Element item);
```

<br>

### Q. Stack 2개로 Queue 구현하기.

 Stack 2개를 이용하면 Queue를 구현할 수 있습니다. Stack A는 Queue의 Insert 연산을 담당하고, Stack B는 Queue의 Pop 연산을 담당하면 됩니다. 

- Insert() 는 Stack A에 데이터를 추가합니다.
- Pop() 은 Stack B에 데이터가 있다면 Stack B에서 Pop을 하고, Stack B가 비어있다면 Stack A의 모든 데이터를 Pop해서 B로 Insert 합니다.
