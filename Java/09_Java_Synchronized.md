# Java Synchronized

### 1) Synchronized

 자바에서 `Synchronized` 키워드를 이용하면 `multi-thread`환경에서 스레드간 동기화 문제를 해결할 수 있습니다. 즉, `Synchronized` 키워드를 이용하면 현재 사용하고 있는 해당 스레드를 제외하고 나머지 스레드들은 접근할 수 없게 블록(blocked) 상태를 지원해줍니다.

 동기화 영역은 `Synchronized` 키워드로 표시됩니다. 동기화는 객체에 대한 동기화로 이루어지는데(synchronized on some object), 같은 객체에 대한 모든 동기화 블록은 한 시점에 오직 한 쓰레드만이 블록 안으로 접근하도록 합니다. 블록에 접근을 시도하는 다른 쓰레드들은 블록 안의 쓰레드가 실행을 마치고 블록을 벗어날 때까지 블록(blocked) 상태가 된다.

<br>

### 2) Synchronized 메소드

 `Synchronized` 키워드는 메소드에 사용할 수 있습니다. 메소드에 사용하게 되면 메소드를 포함한 해당 객체(this)에 Lock을 거는 것과 같습니다.

```java
public static synchronized void add(int value){
      count += value;
}
```

 `add` 메소드에 쓰레드가 접근하고 있다면 다른 쓰레드들은 해당 객체에 접근이 불가능합니다.  즉, 인스턴스 당 한 쓰레드만 이 메소드를 실행할 수 있다는 것을 의미합니다.

```java
public static void main(String[] args) {
    MyHero myHero = new MyHero();

    System.out.println("Test start!");

    new Thread(() -> {
        for (int i = 0; i < 1000000; i++) {
            myHero.batman();
        }
    }).start();

    new Thread(() -> {
        for (int i = 0; i < 1000000; i++) {
            myHero.superman();
        }
    }).start();

    System.out.println("Test end!");

    return;
}

public class MyHero {
    private String mHero;

    public synchronized void batman() {
        mHero= "batman";

        try {
            long sleep = (long) (Math.random()*100);
            Thread.sleep(sleep);

            if ("batman".equals(mHero) == false) {
                System.out.println("synchronization broken");
            }
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }

    public synchronized void superman() {
        mHero = "superman";

        try {
            long sleep = (long) (Math.random()*100); Thread.sleep(sleep);

            if ("superman".equals(mHero) == false) {
                System.out.println("synchronization broken");
            }
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }
}
```

 쓰레드를 2개 만들어 MyHero 객체의 batman() 메소드와 superman() 메소드를 각각 실행하도록 했습니다. batman() 메소드와 superman() 메소드는 `Synchronized` 키워드를 이용하고 있으므로, 먼저 접근한 쓰레드에서 메소드를 호출하면서 MyHero 객체에 Lock을 걸기 때문에, "synchronization broken" 메시지가 출력되지 않습니다.

 이처럼, `Synchronized` 키워드를 메소드에 이용하면 해당 객체에 Lock을 걸기 때문에 객체의 다른 부분도 사용할 수 없다는 단점이 있습니다.

<br>

### 3) Synchronized static 메소드

 static 메소드에도 `Synchronized` 키워드를 이용해 동기화 문제를 해결할 수 있습니다. 다만, static 메소드는 인스턴스가 아니라 같은 클래스에 대해서 오직 한 쓰레드만 동기화된 스태틱 메소드를 실행할 수 있습니다.

```java
public static synchronized void add(int value){
	count += value;
}
```

  `Synchronized` 키워드가 있는 static 메소드가 다른 클래스에 각각 존재한다면, 쓰레드는 각각 접근이 가능하지만 같은 클래스라면 여러 인스턴스에도 하나의 쓰레드만 접근이 가능합니다.

 즉, 클래스 당 한 쓰레드가 접근이 가능합니다.

<br>

### 3) Synchronized 블록 

 `Synchronized` 키워드를 메소드가 아닌 동기화를 원하는 코드 블록에도 활용할 수 있습니다. 코드 블록에 키워드를 사용할 때는 Lock을 걸 주체를 넘겨줘야합니다.

```java
public void add(int value){
    // Do Something
    synchronized(this){ // 동기화를 원하는 블록
       this.count += value;   
    }
    // Do Something
  }
```

 위의 예시를 보면 `Synchronized` 키워드에 `this` 를 전달하고 있는 것을 볼 수 있습니다. 이렇게 활용하면 객체에 Lock 을 거는 것과 동일합니다.

 만약, HashMap 객체인 mMap1, mMap2 를 동시 접근을 막고 싶으면 다음과 같이 `Synchronized` 키워드를 이용할 수 있습니다.

```java
public class SyncBlock { 
    private HashMap<String, String> mMap1 = new HashMap<>(); 
    private HashMap<String, String> mMap2 = new HashMap<>(); 
    private final Object object1 = new Object(); 
    private final Object object2 = new Object(); 
    
    public void put1(String key, String value) {
        synchronized(nMap1) {
            mMap1.put(key, value);
        }
    }
    
    public void put2(String key, String value) {
        synchronized(nMap2) {
            mMap2.put(key, value);
        }
    }
}
```

 이처럼 `Synchronized` 블록은 Lock 을 원하는 객체를 잘 설정한다면, 객체 전체 접근을 막는 것이 아니라 적절하게 동시 접근을 막을 수 있습니다.

<br>

참고

https://coding-start.tistory.com/68 

https://tourspace.tistory.com/54

