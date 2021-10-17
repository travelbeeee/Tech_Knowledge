# Singleton Pattern

 오늘은 스프링 주요 패턴인 `Singleton Pattern`에 대해서 정리해보겠습니다.

### 1) Singleton Pattern

 싱글톤 패턴은 `인스턴스가 오직 1개만 생성`되야 하는 경우에 사용되는 패턴입니다. 인스턴스가 1개만 생성되는 특징을 가진 싱글톤 패턴을 이용하면, 하나의 인스턴스를 메모리에 등록해서 여러 스레드가 동시에 해당 인스턴스를 공유하여 사용하게끔 할 수 있으므로, 요청이 많은 곳에서 사용하면 효율을 높일 수 있습니다. 요청이 많은 웹 환경에서는 싱글톤 패턴을 활용하면 효율을 높일 수 있습니다. 하지만, 싱글톤 패턴은 여러 스레드가 동시에 접근해서 사용하므로 `동시성 문제`를 고려해야합니다. 즉, `Stateless` 하게 설계하는 것이 중요합니다.

 자바에서의 객체의 범위는 클래스 로더가 기준이지만, 스프링에서는 `ApplicationContext`가 기준이 됩니다. 클래스 로더 기준이라는 것은 톰캣이 WAR 파일을 만들게 되면, WAR 파일 하나 당 클래스 로더 하나 1:1 식으로 배치가 되기 때문에 다른 WAR 파일은 참조가 불가능합니다. 반면, ApplicationContext 기준이라는 것은 web.xml 에서 root context 하나와 servlet cotnext 여러개를 등록할 수 있는데, 이 각각의 context 들이 싱글턴 범위가 됩니다.

<br>

### 2) Singleton Pattern in Java

 싱글톤 패턴을 구현하는 방법은 여러 방법이 존재합니다. 그 중, 대표적인 방법에 대해서 정리해보겠습니다.

- Eager Initialization(이른 초기화)

  ```java
  public class Singleton {
      // Eager Initialization
      private static Singleton singletonInstance = new Singleton();
  
      private Singleton() {}
  
      public static Singleton getInstance() {
        return singletonInstance; 
      } 
  }
  ```

  `private` 지시자를 이용해 생성자 사용을 막고, `static` 키워드를 이용해 클래스 로더가 초기화하는 시점에 인스턴스를 메모리에 등록하도록 합니다. 그 후, `getInstance()` 메소드를 이용해서 동일한 싱글톤 객체를 이용할 수 있도록 하는 방법입니다.

- Lazy Initialization with synchronized

  ```java
  public class Singleton {
      private static Singleton singletonInstance;
  
      private Singleton() {}
  
      // Lazy Initailization
      public static synchronzied Singleton getInstance() {
        if(uniqueInstance == null) {
           singletonInstance = new Singleton();
        }
        return singletonInstance;
      }
  }
  ```

  마찬가지로 `private` 지시자를 이용해 생성자 사용을 막습니다. 다만, 컴파일 시점에 인스턴스를 생성하는 것이 아니라, 인스턴스라 필요한 시점에 요청하여 인스턴스를 생성하는 방식입니다. 이를 위해, `synchronized` 키워드를 이용해 동기화 블록을 만들어 처리합니다. 인스턴스가 이미 만들어져있더라도 동기화 블록을 거치게 되므로 성능이 좋지 않은 방법입니다.

- Lazy Initialization with Double Checking Locking

  ```java
  public class Singleton {
      private volatile static Singleton singletonInstance;
  
      private Sigleton() {}
  
      // Lazy Initialization. DCL
      public Singleton getInstance() {
        if(singletonInstance == null) {
           synchronized(Singleton.class) {
              if(singletonInstance == null) {
                 singletonInstance = new Singleton(); 
              }
           }
        }
        return singletonInstance;
      }
  }
  ```

  인스턴스가 이미 만들어져있더라도 동기화 블록을 거치게 되는 문제를 해결한 방법입니다. 필요한 부분에만 동기화 블록을 만드는 방식입니다. `volatile` 키워드를 사용하지 않는다면 Main Memory에서 읽은 변수 값을 CPU Cache에 저장하게 되므로 불일치 문제가 발생할 수 있습니다.

- Lazy Initialization LazyHolder

  ```java
  public class Singleton {
  
      private Singleton() {}
  
      /**
       * static member class
       * 내부클래스에서 static변수를 선언해야하는 경우 static 내부 클래스를 선언해야만 한다.
       * static 멤버, 특히 static 메서드에서 사용될 목적으로 선언
       */
      private static class InnerInstanceClazz() {
          // 클래스 로딩 시점에서 생성
          private static final Singleton singletonInstance = new Singleton();
      }
  
      public static Singleton getInstance() {
          return InnerInstanceClazz.singletonInstance;
      }
      
  }
  ```

  가장 많이 사용되는 싱글톤 구현 방식입니다. `volatile` 키워드나 `synchronized`키워드 없이도 동시성 문제를 해결하기 때문에 성능이 뛰어납니다. 클래스의 내부의 클래스는 외부의 클래스가 초기화될때 초기화되지 않고, 클래스의 static 변수는 클래스를 로딩할 때 초기화되는 것을 이용한 기법이다. 싱글턴 클래스에는 InnerInstanceClazz 클래스의 변수가 없기 때문에, static 멤버 클래스더라도, 클래스 로더가 초기화 과정을 진행할때 InnerInstanceClazz 메서드를 초기화 하지 않고, getInstance() 메서드를 호출할때 초기화 됩니다. 즉, `동적바인딩(Dynamic Binding)` (런타임시에 성격이 결정) 의 특징을 이용하여 Thread-safe 하면서 성능이 뛰어납니다. InnerInstanceClazz 내부 인스턴스는 static 이기 때문에 클래스 로딩 시점에 한번만 호출된다는 점을 이용한것이며, final을 써서 다시 값이 할당되지 않도록 합니다.

