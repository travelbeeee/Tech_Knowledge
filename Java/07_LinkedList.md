# 07_LinkedList

자바에서 제공해주는 `Collection Framwork`에는 `LinkedList` 가 잇습니다.

`LinkedList`는 중복을 허용하고 순서를 유지하지만 인덱스로 원소를 관리하는 것이 아닌, `Node` 를 이용해서 원소를 관리합니다. 

코드를 보면 `Node` 클래스는 `item` 이름으로 원소를 가지고, 다음 노드와 이전 노드를 가리키는 포인터가 존재하는 것을 볼 수 있습니다.

즉, `LinkedList`는 양방향 연결 리스트로 구성되어 있는 것을 확인할 수 있습니다.

```java
public class LinkedList<E>
    extends AbstractSequentialList<E>
    implements List<E>, Deque<E>, Cloneable, java.io.Serializable
{
    transient int size = 0;

    /**
     * Pointer to first node.
     */
    transient Node<E> first;

    /**
     * Pointer to last node.
     */
    transient Node<E> last;
    
    private static class Node<E> {
        E item;
        Node<E> next;
        Node<E> prev;

        Node(Node<E> prev, E element, Node<E> next) {
            this.item = element;
            this.next = next;
            this.prev = prev;
        }
    }
}
```

양방향 연결 리스트로 구현되어있기 때문에, 삽입 또는 삭제는 하는 경우에 O(1) 만에 가능하게 됩니다.

하지만, 참조를 하는 경우에는 해당 원소를 찾기 위해 앞에서부터 순회를 진행해야되므로 O(N) 의 복잡도를 가지게 됩니다.

또, 삽입과 삭제를 중간에 하는 경우에는 해당 위치까지 앞에서부터 순회를 진행해야되므로 마찬가지로 O(N)의 복잡도를 가지게 됩니다.

장점으로는 크기가 커진다고 `ArrayList`와 달리 배열을 복사하는 상황이 생기는 것이 아닌, `Node` 만 하나 추가해주면 된다는 장점이 있습니다. 또, 맨 앞 또는 맨 뒤에서 삽입과 삭제가 자주 일어나는 상황에서는 좋은 성능을 가지게 됩니다.

- 삽입 
  - 맨 앞 / 맨 뒤 : O(1)
  - 중간 : O(N)
- 삭제
  - 맨 앞 / 맨 뒤 : O(1)
  - 중간 : O(N)
- 참조  : O(N)