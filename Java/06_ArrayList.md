# 06_ArrayList

#### [ ArrayList ]

자바에서 제공해주는 `Collection Framework` 에는 `ArrayList` 가 있습니다.

`ArrayList` 는 중복을 허용하고 순서를 유지하며 인덱스로 원소들을 관리한다는 점에서 배열과 상당히 유사합니다. 배열은 크기가 고정되지만, `ArrayList`는 크기를 동적으로 맞춰가며 원소들을 추가, 삭제 할 수 있습니다.

인덱스로 원소를 관리하기 때문에 원소를 참조하는 작업은 O(1) 만에 가능합니다.

원소를 삭제하는 작업은 `ArrayList`의 중간에 있는 원소를 삭제하게 된다면 나머지 부분을 다 하나하나 옮겨야되므로 O(N)의 복잡도를 가지게 됩니다.

원소를 삽입하는 작업도 마찬가지로 `ArrayList`의 중간에 원소를 삽입하려면 나머지 부분들 하나하나 뒤로 밀어야되므로 O(N)의 복잡도를 가지게 됩니다.

- 검색 : O(1)
- 삽입, 삭제 : O(N)

<br>

#### [ ArrayList 코드 뜯어보기 ]

`ArrayList` 코드를 뜯어보도록 하겠습니다.

먼저, 생성자 코드를 뜯어보겠습니다.  3가지 생성자 코드가 있는 것을 확인할 수 있습니다.

초기 용량이 주어지면, 초기 용량 크기에 맞춰서 Object 배열의 크기를 선언하고 있는 것을 확인할 수 있습니다. `DEFAULT_CAPACITY`는 10으로 설정되어있고, 초기 용량을 입력하지 않고 생성하게 되면 10 크기를 가지는 배열이 선언되는 것을 확인할 수 있습니다. 

- `아무 것도 매개변수로 받지 않는 생성자`
- `초기 용량을 매개변수로 받는 생성자`
- `Collection 타입을 매개변수로 받는 생성자`

```java
public class ArrayList<E> extends AbstractList<E>
        implements List<E>, RandomAccess, Cloneable, java.io.Serializable
{
    private static final long serialVersionUID = 8683452581122892189L;
    private static final int DEFAULT_CAPACITY = 10;
    private static final Object[] EMPTY_ELEMENTDATA = {};
    private static final Object[] DEFAULTCAPACITY_EMPTY_ELEMENTDATA = {};
    private int size;

    public ArrayList(int initialCapacity) {
        if (initialCapacity > 0) {
            this.elementData = new Object[initialCapacity];
        } else if (initialCapacity == 0) {
            this.elementData = EMPTY_ELEMENTDATA;
        } else {
            throw new IllegalArgumentException("Illegal Capacity: "+
                                               initialCapacity);
        }
    }

    public ArrayList() {
        this.elementData = DEFAULTCAPACITY_EMPTY_ELEMENTDATA;
    }

    public ArrayList(Collection<? extends E> c) {
        Object[] a = c.toArray();
        if ((size = a.length) != 0) {
            if (c.getClass() == ArrayList.class) {
                elementData = a;
            } else {
                elementData = Arrays.copyOf(a, size, Object[].class);
            }
        } else {
            // replace with empty array.
            elementData = EMPTY_ELEMENTDATA;
        }
    }
}
```

원소를 추가하다가 공간이 부족할 경우에는 동적으로 크기를 관리해준다고 했는데, 구체적으로 어떻게 동작할까요.

**원소를 추가하면 크기를 늘리는 것이 아니라, 용량이 꽉 찼을 경우에만 더 큰 용량의 배열을 만들어 옮기는 작업을 하게 됩니다.**

```java
private Object[] grow(int minCapacity) {
    return elementData = Arrays.copyOf(elementData,
                                       newCapacity(minCapacity));
}

private Object[] grow() {
    return grow(size + 1);
}

private int newCapacity(int minCapacity) {
    // overflow-conscious code
    int oldCapacity = elementData.length;
    int newCapacity = oldCapacity + (oldCapacity >> 1);
    if (newCapacity - minCapacity <= 0) {
        if (elementData == DEFAULTCAPACITY_EMPTY_ELEMENTDATA)
            return Math.max(DEFAULT_CAPACITY, minCapacity);
        if (minCapacity < 0) // overflow
            throw new OutOfMemoryError();
        return minCapacity;
    }
    return (newCapacity - MAX_ARRAY_SIZE <= 0)
        ? newCapacity
        : hugeCapacity(minCapacity);
}
```

`Arrays.copyOf` 메소드가 실행되는 것을 보면, 결국 `ArrayList`도 내부적으로 용량이 부족하면 배열을 복사하고 더 큰 크기의 배열을 할당받도록 동작하는 것을 확인할 수 있습니다.

새로운 크기는 기존 크기 * 1.5 인 것도 확인할 수 있습니다.

```java
int newCapacity = oldCapacity + (oldCapacity >> 1);
```

#### [ ArrayList 정리 ]

 이처럼 `ArrayList`  도 동적으로 크기를 할당할 수는 있지만, 결국에는 기존 배열을 복사해 더 큰 새로운 배열을 만드는 것이므로, 용량이 부족한 상황이 많이 생기게 된다면 성능이 떨어질 수 밖에 없습니다.  **따라서, 초기 용량을 잘 성정하는 것이 중요하고, 원소 참조는 O(1) 이므로 원소 참조를 많이 하는 경우에 사용하는 것이 좋다.**

