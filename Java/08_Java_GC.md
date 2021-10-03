# Java_Garbage Collection

자바 `Garbage Collection`에 대해서 정리해보겠습니다.

<br>

### 1) Garbage Collection 이란

 프로그램을 개발 하다 보면 유효하지 않은 메모리인 가바지(Garbage)가 발생하게 된다. 자바에서는 개발자가 메모리를 직접 해제해주지 않아도 JVM의 가비지 컬렉터가 불필요한 메모리를 알아서 정리해준다. 대신 Java에서 명시적으로 불필요한 데이터를 표현하기 위해 일반적으로 null을 선언해준다.

 Java에서는 가비지 컬렉터가 주기적으로 검사하여 사용이 되지 않는 메모리를 청소해준다.

 모든 자바 애플리케이션은 `JVM` 환경에서 작동하므로 `Garbage Collection` 은 자바 애플리케이션의 응답 시간가 성능에 밀접한 관계를 가지게 됩니다.

 자바의 가비지 컬렉터는 그 동작 방식에 따라 매우 다양한 종류가 있지만 공통적으로 다음 2가지 작업을 수행한다.

- Heap 내의 객체 중에서 Garbage를 찾아낸다.
- 찾아낸 Garbage를 처리해서 Heap 의 메모리를 회수한다.

> 자바에서도 System.gc() 를 이용해 가비지 컬렉션을 호출할 수 있지만, 시스템의 성능에 매우 안좋은 영향을 끼친다.

<br>

### 2)  `Young(Minor GC)` , `Old(Major GC)` 영역

자바 `Garbage Collection` 은 2가지를 전제로 설계되었습니다.

- 대부분의 객체는 금방 접근 불가능한 상태(Unreachable)이 된다.
- 오래된 객체에서 새로운 객체로의 참조는 아주 적게 존재한다.

즉, 객체는 대부분 일회성이며 메모리에 오랫동안 남아있는 경우는 드물다! 를 전제로 만들어졌습니다.

그렇기 때문에, 객체의 생존 기간에 따라 물리적인 `Heap` 영역을 `Young`, `Old` 2가지 영역으로 설계했습니다.

> 초기에는 `Perm` 영역도 존재했으나, Java8부터 제거되었습니다.

![img](https://user-images.githubusercontent.com/59816811/135704340-48ecc01a-3cbc-4f9e-85da-a0db35f6d98f.png)

- `Young` 영역
  - 새롭게 생성된 객체가 할당되는 영역
  - 대부분의 객체가 금방 Unreachable 상태가 되기 때문에, 대부분의 객체는 `Young` 영역에 생성되었다가 사라집니다.
  - `Young` 영역에 대한 가비지 컬렉션을 `Minor GC`라고 부릅니다.
- `Old` 영역
  - `Young`영역에서 Reachable 상태를 유지하여 살아남은 객체가 복사되는 영역
  - 복사되는 과정에서 대부분 `Young` 영역보다 크게 할당되며, 크기가 큰 만큼 `GC`가 `Young`영역보다 적게 발생한다.
  - `Old` 영역에 대한 가비지 컬렉션을 `Major GC` 라고 부릅니다.

<br>

그러면, `Old` 영역에서 `Young` 영역의 객체를 참조하면 어떻게 될까?? `Old` 영역에서 참조하고 있는 `Young` 영역 객체가 GC 대상이 되면 안되므로 이를 위해 `Old` 영역에는 512바이트의 덩어리로 되어 있는 `Card Table` 이 존재합니다.

![helloworld-1329-2](https://user-images.githubusercontent.com/59816811/135704440-fa1df0a7-178a-4c94-a1d8-70c545f04cec.png)

카드 테이블을 이용해 `Old` 영역에서 참조하고 있는 `Young` 영역 객체를 관리하고 있습니다. 따라서, `Minor GC` 는 카드 테이블만 확인해 `Old` 영역에서 참조하고 있지 않다면 GC가 가능합니다. 즉, `Minor GC`는 `Stop-the-world` 과정 없이 카드 테이블을 이용해 가능합니다.

<br>

### 3) Young (Minor GC)

`Young` 영역은 또 내부적으로 3개의 영역으로 나뉜다.

- Eden 영역 
- Survivor 영역 ( 2개 )

영역의 처리 절차는 다음과 같다.

- 새로 생선한 대부분의 객체는 `Eden` 영역에 위치한다.
- `Eden` 영역에서 GC가 한 번 발생한 후 살아남은 객체는 `Survivor` 영역 중 하나로 이동한다.
- `Eden` 영역에서 GC가 발생하면 이미 살아남은 객체가 존재하는 `Survivor` 영역으로 객체가 계속 쌓인다.
- 하나의 `Survivor` 영역이 가득 차게 되면 그 중에서 살아남은 객체를 다른 `Survivor` 영역으로 이동한다. 그리고 가득 찬 `Survivor` 영역을 비웁니다.
- 이 과정을 반복하다가 계속해서 살아남아 있는 객체는 `Old` 영역으로 이동하게 된다.

![helloworld-1329-3](https://user-images.githubusercontent.com/59816811/135704781-1d9aa15c-1ebf-43aa-b06b-52a876cc7d68.png)

> HotSpot JVM에서는 보다 빠른 메모리 할당을 위해서 두 가지 기술을 사용한다. 하나는 bump-the-pointer라는 기술이며, 다른 하나는 TLABs(Thread-Local Allocation Buffers)라는 기술이다.
>
> bump-the-pointer는 Eden 영역에 할당된 마지막 객체를 추적한다. 마지막 객체는 Eden 영역의 맨 위(top)에 있다. 그리고 그 다음에 생성되는 객체가 있으면, 해당 객체의 크기가 Eden 영역에 넣기 적당한지만 확인한다. 만약 해당 객체의 크기가 적당하다고 판정되면 Eden 영역에 넣게 되고, 새로 생성된 객체가 맨 위에 있게 된다. 따라서, 새로운 객체를 생성할 때 마지막에 추가된 객체만 점검하면 되므로 매우 빠르게 메모리 할당이 이루어진다.
>
> 그러나 멀티 스레드 환경을 고려하면 이야기가 달라진다. Thread-Safe하기 위해서 만약 여러 스레드에서 사용하는 객체를 Eden 영역에 저장하려면 락(lock)이 발생할 수 밖에 없고, lock-contention 때문에 성능은 매우 떨어지게 될 것이다. HotSpot VM에서 이를 해결한 것이 TLABs이다.
>
> 각각의 스레드가 각각의 몫에 해당하는 Eden 영역의 작은 덩어리를 가질 수 있도록 하는 것이다. 각 쓰레드에는 자기가 갖고 있는 TLAB에만 접근할 수 있기 때문에, bump-the-pointer라는 기술을 사용하더라도 아무런 락이 없이 메모리 할당이 가능하다.

<br>

### 4)  Old (Major GC)

`Young` 영역에서 오래 살아남은 객체는 `Old` 영역으로 Promotion 된다. 따라서, `Major GC`는 객체들이 계속 Promotion되어 `Old` 영역의 메모리가 부족해지면 발생하게 된다. `Young` 영역은 일반적으로 `Old` 영역보다 크키가 작기 때문에 GC가 보통 0.5초에서 1초 사이에 끝난다. 그렇기 때문에 `Minor GC`는 애플리케이션에 크게 영향을 주지 않는다. 하지만 `Old` 영역은 `Young` 영역보다 크며 `Young` 영역을 참조할 수도 있다. 그렇기 때문에 `Major GC`는 일반적으로 `Minor GC`보다 시간이 오래걸리며, 10배 이상의 시간을 사용한다.

<br>

##### Stop The World

 `Stop The World`는 가비지 컬렉션을 실행하기 위해 JVM이 애플리케이션의 실행을 멈추는 작업이다. `Major GC`가 실행될 때는 GC를 실행하는 쓰레드를 제외한 모든 쓰레드들의 작업이 중단되고, GC가 완료되면 작업이 재개된다. 당연히 모든 쓰레드들의 작업이 중단되면 애플리케이션이 멈추기 때문에, GC의 성능 개선을 위해 튜닝을 한다고 하면 보통 stop-the-world의 시간을 줄이는 작업을 하는 것이다. 또한 JVM에서도 이러한 문제를 해결하기 위해 다양한 실행 옵션을 제공하고 있다.

<br>

##### **Mark and Sweep**

- Mark: 사용되는 메모리와 사용되지 않는 메모리를 식별하는 작업
- Sweep: Mark 단계에서 사용되지 않음으로 식별된 메모리를 해제하는 작업

 `Stop The World`를 통해 모든 작업을 중단시키면, GC는 스택의 모든 변수 또는 Reachable 객체를 스캔하면서 각각이 어떤 객체를 참고하고 있는지를 탐색하게 된다. 그리고 사용되고 있는 메모리를 식별하는데, 이러한 과정을 Mark라고 한다. 이후에 Mark가 되지 않은 객체들을 메모리에서 제거하는데, 이러한 과정을 Sweep라고 한다.

<br>

##### Q. Old 영역이 가득 차면 Major GC가 발생하고 이로 인해 성능에 문제가 생긴다면 Old 영역의 Heap 크기를 키우면 되지 않을까??

​	A. Heap 영역을 키우더라도 결국 Major GC는 언젠가 발생하게 된다. 이때, Heap 영역의 크기에 비례해서 GC 실행 시간이 정해지므로 오히려 애플리케이션의 실행이 멈추는 시간이 더 길어지는 악효과가 발생한다.

​	--> Major GC 를 어떤 알고리즘을 이용해 어떻게 최소화 할 것인가. 어떻게 피할 것인가를 생각하자.

<br>

### 5) Old ( Major GC) 방식

 Old 영역은 기본적으로 데이터가 가득 차면 GC를 실행합니다. GC 방식에 따라서 처리 절차가 달라지고, GC 방식은 JDK 7을 기준으로 5가지 방식이 있습니다.

- Serial GC
- Parallel GC
- Parallel Old GC(Parallel Compacting GC)
- Concurrent Mark & Sweep GC(이하 CMS)
- G1(Garbage First) GC

> JDK 11에서는 Default 로 G1 GC를 사용하고 있다.JDK 11에서는 Default 로 G1 GC를 사용하고 있다.

##### Serial GC

 Serial GC는 데스크톱의 CPU 코어가 하나만 있을 때 사용하기 위해서 만든 방식이라 Serial GC를 사용하면 애플리케이션의 성능이 많이 떨어집니다. Young 영역에서의 GC는 위에서 정리한 내용대로 실행하고, Old 영역의 GC는 mark-sweep-compact이라는 알고리즘을 사용합니다. 

 이 알고리즘의 첫 단계는 Old 영역에 살아 있는 객체를 식별(Mark)하는 것이다. 그 다음에는 힙(heap)의 앞 부분부터 확인하여 살아 있는 것만 남긴다(Sweep). 마지막 단계에서는 각 객체들이 연속되게 쌓이도록 힙의 가장 앞 부분부터 채워서 객체가 존재하는 부분과 객체가 없는 부분으로 나눈다(Compaction).

##### Parallel GC

 Parallel GC는 Serail GC가 하나의 스레드로 처리하는 것에 비해 멀티 스레드로 GC 작업을 처리합니다.

##### Parallel Old GC

 Parallel GC와 비교하여 Old 영역의 GC 알고리즘만 다르다. 이 방식은 Mark-Summary-Compaction 단계를 거친다. Summary 단계는 앞서 GC를 수행한 영역에 대해서 별도로 살아 있는 객체를 식별한다는 점에서 Mark-Sweep-Compaction 알고리즘의 Sweep 단계와 다르며, 약간 더 복잡한 단계를 거친다

#####  CMS GC

 앞의 GC 방식보다 더 개선되었으면서, 복잡한 방식이다. Stop-The-World 시간을 최소화 하는데 초첨을 맞췄다. 컨셉은 GC 대상 객체를 최대한 자세히 파악한 후, Stop-The-World 가 발생하는 Sweep 시간을 최소화 하는데 초점을 맞췄다. Low Latency GC라고도 부르고, `Compaction` 이 기본적으로 제공되지 않고 필요할 때만 일어난다.

1. Initial Mark
   GC 과정에서 살아남을 객체를 Root Set에서 가장 가까운 객체만 탐색하며, 참조가 끊겼는지를 확인한다. 이 과정에서 Stop-The-World 가 일어나지만, 탐색 깊이가 짧아 멈추는 시간 역시 짧다. ( 클래스 로더에서 가장 가까운 객체중 살아 있는 객체만 찾는다 )
2. ConcurrentMark
   Initial Mark 단계에서 GC 대상으로 판별한 객체들을 따라가며 GC 대상인지 추가로 확인한다. 이 과정중에는 Stop-The-World 가 일어나지 않는다. ( 위에서 살아있다고 확인한 객체에서 참조되고 있는 객체를 확인한다 )
3. Remark
   Concurrent Mark 단계의 결과를 검증한다. Concurrent Mark 단계에서 GC 대상으로 추가 확인되었는지, 참조가 제거되었는지 등 확인을 한다. 이때 Stop-The-World가 일어나며, 이 시간을 최대한 줄이기 위해 멀티스레드로 검증작업을 수행한다. ( 위 단계에서 새로 추가되거나 참조가 끊긴 객체를 확인한다 )
4. Concurrent Sweep
   GC 대상으로 판별된 객체들을 멀티스레드로 메모리에서 제거한다. 이때 Stop-The-World가 발생하지 않는다.

단점으로는 하는 일이 많다보니 CPU 부하가 커진다는 것이고, Compaction이 기본적으로 제공되지 않고 필요할 때만 일어나는데, 이때의 Stop-The-World 시간이 다른 GC보다 더 길게 걸릴 수도 있다.

##### G1 GC

 하드웨어가 발전되어 JVM을 가동하는 메모리의 크기도 점점 커져가는데, 이전까지의 GC들은 큰 용량의 메모리에 적합하지 않다(Root set부터 순차적으로 탐색하기에 용량이 클 수록 탐색 시간이 길어짐).
 G1 GC는 이런 점을 개선하여, 큰 Heap 메모리에서 짧은 GC 시간을 보장하는데 그 목적을 둔다. G1 GC는 Eden, Survivor, Old 영역이 존재하지만, 고정된 크기로 고정된 위치에 존재하지 않는다. 전체 Heap 영역을 Region이라는 특정한 크기로 나눠서, 각 Region의 상태에 따라 역할(Eden, Survivor, Old)이 동적으로 부여된다. 2048개의 Region으로 나뉠수 있으며, 옵션을 통해 1MB~32MB 사이로 지정할 수 있다. Region은 기본적으로 ( 전체 Heap 메모리 ) / 2048 로 default 값이 지정되어 있습니다.

![다운로드 (2)](https://user-images.githubusercontent.com/59816811/135750099-e391d405-f3a2-495e-84fd-d2977b27b528.png)

앞에서 봤던 Eden, Survivor, Old 외에 Humonogous와 Available/Unused Region이 추가로 보인다. Humongous는 설정된 Region 크기의 50%를 초과하는 큰 객체를 저장하기 위한 공간으로, 이 Region에서는 GC 동작이 최적으로 동작하지 않는다. Available/Unused 는 이름에서 짐작할 수 있듯, 아직 사용되지 않은 공간이다.

`G1GC` 에서도 마찬가지로 **Minor GC** 가 존재하며, 요 과정에는 살아남은 객체들을 Survivor Region으로 옮기고, Eden에 대한 영역을 사용가능한(Availabe)Region으로 돌리는 형태로 과정이 일어나게 됩니다. `G1GC` 에는 은 `IHOP(InitiatingHeapOccupancyPercent)` 에서 정한 수치를 초과하면 **Full GC** 와 유사한 **Concurrent Cycle** 이라는 과정이 총 6개의 단계를 거쳐 이루어지게 된다.

1. **Initial Mark** : Old Region에 존재하는 객체들이 참조하는 Survivor Region을 찾는다(STW)
2. **Root Region Scan** : 위에서 찾은 Survivor 객체들에 대한 스캔 작업을 실시한다
3. **Concurrent Mark** : 전체 Heap의 scan 작업을 실시하고, GC 대상 객체가 발견되지 않은 Region은 이후 단계를 제외한다
4. **Remark** : 애플리케이션을 멈추고(STW) 최종적으로 GC 대상에서 제외할 객체를 식별한다
5. **Cleanup** : 애플리케이션을 멈추고(STW) 살아있는 객체가 가장 적은 Region에 대한 미사용 객체를 제거한다
6. **Copy** : GC 대상의 Region이었지만, Cleanup 과정에서 완전히 비워지지 않은 Region의 살아남은 객체들을 새로운 Region(Available/Unused) Region에복사하여 Compaction을 수행한다

<br>

참고

https://d2.naver.com/helloworld/1329

https://velog.io/@hygoogi/%EC%9E%90%EB%B0%94-GC%EC%97%90-%EB%8C%80%ED%95%B4%EC%84%9C

https://huisam.tistory.com/entry/jvmgc

