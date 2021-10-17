# Interface 실험실 

### 1) 서로 다른 Interface에 동일한 메소드가 있다면?

 `AInterface`와 `BInterface`가 동일한 메소드가 있을 때, 다중 인터페이스 구현을 하면 어떻게 될까?

```java
public interface AInterface {
    void move(int x, int y);
    void attack(int damage);
}

public interface BInterface {
    void move(int x, int y);
    void defense(int damage);
}

public class ABClass implements AInterface, BInterface {
    @Override
    public void move(int x, int y) {

    }

    @Override
    public void defense(int damage) {

    }

    @Override
    public void attack(int damage) {

    }
}
```

 `ABClass` 에서 2개의 Interface를 상속하고 있는데, 동일한 메소드인 move() 는 한 번만 구현하면 됩니다.

<br>

### 2) 서로 다른 Interface에서 동일한 메소드를 default로 구현해놓고 있다면?

```java
public interface AInterface {
    default void move(int x, int y){
        System.out.println("AInterface.move");
        return;
    }
    void attack(int damage);
}
public interface BInterface {
    default void move(int x, int y){
        System.out.println("BInterface.move");
        return;
    }
    void defense(int damage);
}
public class ABClass implements AInterface, BInterface {
    @Override
    public void attack(int damage) {

    }

    @Override
    public void defense(int damage) {

    }
}
```

![image-20211017140850766](C:\Users\sochu\AppData\Roaming\Typora\typora-user-images\image-20211017140850766.png)

 에러가 발생하는 것을 확인할 수 있습니다.

<br>

### 3) 서로 다른 Interface에서 동일한 메소드를 하나는 default로 구현해놓고, 하나는 추상화해놓는다면?

```java
public interface AInterface {
    void move(int x, int y);
    void attack(int damage);
}

public interface BInterface {
    default void move(int x, int y){
        System.out.println("BInterface.move");
        return;
    }
    void defense(int damage);
}

public class ABClass implements AInterface, BInterface {
    @Override
    public void move(int x, int y) {
        System.out.println("ABClass.move");
    }

    @Override
    public void attack(int damage) {

    }

    @Override
    public void defense(int damage) {

    }
}
```

 당연히 `AInterface` 에서 추상화해놓은 메소드를 구현해야되기 때문에 `ABClass`에서 `move()` 메소드를 구현해야되고, 이는 `BInterface` 에서 구현해놓은 메소드를 덮어버립니다.

<br>

### 4) 나의 생각...?

 자바8부터 `Interface`에서 `default` 지시자로 메소드를 구현할 수 있게 됐습니다. 그러면... 인터페이스랑 추상 클래스랑 기능적으로 이제 뭐가 다른거지....? 라는 생각이 들었고 이런 실험까지 오게 됐습니다... 결론은 기능은 위에서 실험한 것 처럼 비슷해진 것이 맞는 것 같습니다. 다만, 둘은 기능적인 부분이 아니라 쓰임새의 차이 때문에 등장한 개념이니까 쓰임새에 초점을 맞추는 것이 좋지 않을까라는 결론을 내려보았습니다..(맞으려나)

 결론을 내려보자면 기능적으로는 비슷해졌으나, `Interface`는 어떤 클래스의 공통 스팩 껍데기를 만드는데에 초점을 두고, `Abstract Class`는 공통 기능을 가진 클래스를 사전에 정의해두는 것이라 쓰임새가 다릅니다!