# JVM ( Java Virtual Machine )

### 0) JVM 등장 배경

 C/C++는 컴파일 플랫폼과 타켓 플랫폼이 다를 경우, 프로그램이 동작하지 않는 문제가 있습니다. 개발할 때는 문제가 없지만, 배포를 할 때 문제가 발생합니다. Linux에서 개발하고 Window에 배포하려면 추가적인 작업이 필요합니다. 이런 문제를 해결하기 위해 Java는 `JVM`을 이용합니다. `Java Compiler`가 컴파일하면 `Java Bytecode`가 만들어지고, JVM이 설치된 플랫폼이라면 모두 `Java Bytecode`를 실행시킬 수 있도록 만들었습니다.

( 플랫폼 : 운영체제 + CPU )

### 1) JVM 이란 

 JVM이란, 자바 가상 머신의 약자를 줄여 부르는 용어이다.

 일반적인 프로그램은 Windows또는 Linux같은 OS 위에서 실행된다. 하지만 자바 프로그램 같은 경우에는 OS 위의 JVM에서 실행이된다. 따라서, 자바 프로그램은 OS에 상관없이 실행시킬 수 있다는 장점이 있다. 예를들어 Windows에서 동작하도록 구현된 워드 프로그램은 Linux에서 동작하지 않는다. 이 워드 프로그램을 Mac환경에서 돌리기 위해서는 Mac기반으로 다시 구현해야 한다. 하지만 자바 프로그램 같은 경우 어떤 OS 에서도 그에 맞는 JVM 다운로드를 통해 자바 프로그램을 실행 실킬 수 있다.

<br>

### 2) JVM 역할

- 자바 애플리케이션을 클래스 로더를 통해 읽어 들여 자바 API와 함께 실행한다.
- JVM은 자바와 OS사이에서 중개자 역할을 수행하여 OS에 독립적인 플랫폼을 갖게 해준다. 즉 OS 의 메모리 영역에 직접적으로 접근하지 않고 JVM 이라는 가상 머신을 이용해서 간접적으로 접근한다.
- JVM은 프로그램의 메모리 관리를 알아서 해준다. C프로그램 같은 경우에는 직접 메모리 할당을 해주고 해지해줘야한다. 하지만 자바에서는 JVM이 자동으로 메모리 관리를 해주는 장점이 있다.

<br>

### 3) 자바 프로그램 실행 과정

 Java에서 프로그램을 실행한다는 것은 컴파일 과정을 통하여 생성된 Class 파일을 JVM으로 로딩하고 ByteCode를 해석(interpret)하는 과정을 거쳐 운영체제로부터 메모리 등의 리소스를 할당하고 관리하며 정보를 처리하는 일련의 작업들을 포괄한다. 컴파일러가 Class 파일을 `Java ByteCode`로 만들고 JVM이 운영체제와 CPU 플랫폼에 맞춰 이를 `기계언어`로 바꾸어서 동작한다.

![다운로드](https://user-images.githubusercontent.com/59816811/139841934-83eb3139-6203-4440-8149-0206006d1fe6.png)

- 프로그램이 실행되면 JVM은 OS로부터 이 프로그램이 필요로 하는 메모리를 할당받는다.
- 자바 컴파일러(javac)가 자바 소스코드(.java)를 읽어들여 자바 바이트코드(.class)로 변환시킨다.
- 클래스 로더를 통해 class 파일들을 JVM으로 로딩한다.
  - 프로그램이 필요한 클래스들을 로딩하여 런타임 영역인 JVM 메모리로 로딩한다.
  - 인터프리터로 명령어를 하나씩 해석하고 수행한다.
  - JLT 방식은 캐시를 사용해 기존에 해석한 바이트코드 전체를 기계어로 바꿔서 수행한다. 인터프리터의 단점을 보완하는 방식
- 로딩된 class 파일들은 Execution Engine을 통해 해석된다.
- 해석된 바이트코드(.class) 는 Runtime Data Areas에 배치되고 수행이 이루어지게 된다.

<br>

- Java Compiler : 자바 소스(.java) 코드를 Byte code(.class)로 변환하는 역할을 한다.
- Class Loader : JVM 내로 클래스를 로드하고 링크를 통해 배치하는 작업을 수행하는 모듈로써 런타임시 동적으로 클래스를 로드한다.
- Execution Engine : Class Loader를 통해 JVM 내의 런타임 데이터 영역에 배치된 바이트 코드를 실행한다. ( 자바 바이트 코드를 명령어 단위로 읽어서 실행한다. ) 자바 바이트 코드는 기계가 바로 수행할 수 있는 기계어가 아니기 때문에 Execution Engine 이 바이트 코드를 실제로 JVM 내부에서 기계가 실행할 수 있는 형태로 변경해준다.
  - Interpreter : 바이트 코드 명령어를 하나씩 읽어서 해석하고 실행한다. 하나씩 '읽고' '실행'하는 두 단계로 진행되는데, 하나하나의 해석은 빠르지만 실행은 느리다.
  - JIT(Just-In-Time) Compiler : 인터프리터의 단점을 보완하기 위해 도입되었다. 인터프리터 방식으로 실행하다가 적절한 시점에 바이트 코드 전체를 컴파일하여 네이티브 코드로 변환시킨다. 이후에는 해당 메서드를 더이상 인터프리팅 하지 않고 캐시에 있는 네이티브 코드를 실행한다. 한번 컴파일된 코드는 빠르게 수행된다.
- Garbage Collector : JVM은 Garbage Collector를 통해 메모리 관리 기능을 자동으로 수행한다. 애플리케이션이 생성한 객체의 생존 여부를 판단하여 더 이상 사용되지 않는 객체를 해제하는 방식으로 메모리를 자동 관리한다.
- Runtime Data Area : JVM이 운영체제 위에서 실행되면서 할당받는 메모리 영역이다. Class Loader에서 준비한 데이터들을 보관하는 저장소이다. 런타임 데이터 영역은 5개의 영역으로 나눌 수 있다. 이 중, PC Register, JVM Stack, Native Method Stack은 스레드마다 하나씩 생성되고, Heap, Method Area는 모든 스레드가 공유한다.<img src="https://user-images.githubusercontent.com/59816811/122667065-dec67580-d1eb-11eb-8e49-7d110e556dec.png" alt="image" style="width:500px;" />

  - Method Area : JVM의 클래스 로더가 클래스 파일을 읽어오면서 정보를 파싱해서 저장하는 공간입니다. 읽어들인 클래스와 인터페이스에 대한 런타임 상수 풀, 멤버 변수(필드), 클래스 변수(static 변수), 생성자와 메소드를 저장하는 공간이다. 이 영역에 있는 클래스 정보로 Heap 영역에 객체를 생성한다. ( 모든 쓰레드가 공유 )
    - Runtime Constant Pool : 클래스 파일 포맷에서 constant pool 테이블에 해당하는 영역이다. 메서드 영역에 포함되는 영역이긴 하지만, JVM 동작에서 핵심적인 역할을 수행하는 곳이다. 각 클래스와 인터페이스의 상수뿐만 아니라 메서드와 필드에 대한 모든 레퍼런스까지 담고있는 테이블이다.
      즉, 어떤 메서드나 필드를 참조할 때 JVM은 Runtime Constant Pool 을 통해 해당 메서드나 필드의 실제 메모리상 주소를 찾아서 참조한다.
    
  - Heap Area : JVM이 관리하는 프로그램 상에서 데이터를 저장하기 위해 런타임 시 동적으로 할당하여 사용하는 영역이다. New 연산자로 생성된 객체들을 저장한다. ( 모든 쓰레드가 공유, GC 가 일어나는 영역)

    - Permanent Generation

      생성된 객체들의 정보의 주소값이 저장되는 공간으로 `Class Loader`에 의해 load되는 Class, Method 등에 대한 Meta 정보가 저장되는 영역이다.

  - Stack Area : 각 쓰레드마다 하나씩 존재하며, 쓰레드가 시작될 때 할당되는 영역이다. 메소드를 호출할 때마다 프레임을 추가하고 메소드가 종료되면 해당 프레임을 제거하는 동작을 수행한다. 기본타입 변수는 스택 영역에 직접 값을 가지고, 참조타입 변수는 힙 영역이나 메소드 영역의 객체 주소를 가지게 된다.

  - PC Register : 현재 수행 중인 JVM 명령 주소를 가지고 있다. 연산 결과를 메모리에 전달하기 전에 저장하는 CPU 내의 기억 장치이다.

  - Native Method Stack Area : 자바 외 언어로 작성된 네이티브 코드를 위한 Stack 영역이다.

OS 위에 존재하는 JVM 위에서 실행되기 때문에 운영체제에 독립적으로 실행될 수 있습니다. 하지만, JVM을 사용하기 때문에 많은 메모리를 사용하고 실행 속도가 느리다는 단점이 있습니다. 

실행하는 과정에서 JVM이 반드시 완벽하게 로딩되어야하기 때문에 프로그램의 초기 시작 시간이 C/C++에 비해 2~3배 정도 느리다고 알려져있습니다. 단적인 예로, 콘솔 화면에 달랑 "Hello, World"를 찍는 프로그램이 실행되는 데에도 AWT, Swing, SQL 같은 불필요한 기능까지 모두 로딩된 후에 실행되기 때문에 속도가 느릴 수 밖에 없습니다.

<br>

### 4) 자바 PermGen / Metaspace

 자바에서는 `Permanent Generation Space`인 `PermGen` 영역이 있습니다. 특별한 `Heap` 공간으로 클래스 메타정보, 메소드 메타정보, `Static Object` 정보, `String Object` 정보 등 `MethodArea`의 정보들이 `PermGen` 영역에 할당되었습니다. 따라서, JVM의 메모리 영역은 크게 `Heap Space` 와 `PermGen Space`로 나뉘게 됩니다.

 프로젝트가 커지다보면 `PermGen` 영역에서 `OutOfMemory` 문제가 발생하게 됩니다. 특히, 개발자의 실수로 `Static Object`를 많이 생성하게 되는 경우 객체의 모든 부분이 `Perm Gen`에 저장되므로 문제가 발생하는 상황이 많았습니다.

 이런 문제를 해결하기 위해 자바8부터는 `PermGen`영역을 완전히 제거하고 `Metaspace` 라는 이름의 영역을 새롭게 만들었습니다.

 `Metaspace`영역은 `PermGen`영역과 다르게 `Normal JVM Heap` 영역 바깥에 있는 `Native Heap`에 할당되고, OS가 관리하므로 따로 `MaxSize`를 설정해주지 않아도 됩니다. JVM에 의해 관리되는 `Heap` 영역에서 클래스, 메소드 메타 정보들을 빼고 `Native` 영역으로 옮겼고, 개발자는 영역 확보의 상한을 크게 의식할 필요가 없습니다. 즉, 각종 메타 정보를 OS가 관리하는 영역으로 옮겨 Perm 영역의 사이즈 제한을 없앤 것이라 할 수 있다.

 또, `Static Object` 정보는 `MetaSpace` 영역이 아니라 `Heap`영역으로 옮겨 GC가 될 수 있도록 했습니다.

1) **Heap memory**: memory within the JVM process that is used to hold Java Objects and is maintained by the JVMs Garbage Collector.

2) **Native memory/Off-heap**: is memory allocated within the processes address space that is not within the heap and thus is not freed up by the Java Garbage Collector.

