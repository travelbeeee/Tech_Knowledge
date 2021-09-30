### 3) String vs StringBuildervs StringBuffer

자바에서 문자열을 다루는 객체는 크게 `String`, `StringBuffer`, `StringBuilder` 3개가 있습니다.

- String 객체
  - immutable 합니다. 즉, 한 번 생성이 되면 변경이 불가능 합니다.
  - 우리가 알고 있는 String 더하기 연산을 하여 2개의 String 객체를 붙일 시 새로운 객체가 생성되어 재할당되는 것입니다. 다라서, `Heap` 영역에 참조를 잃은 문자열 객체가 계속해서 쌓이게 되고, `GC`에 의해 수거가 되지만 메모리 관리 측면에서 좋지 않습니다. 
- StringBuilder
  - mutable 합니다. append() 메소드를 지원해서 문자열 값을 변경할 수 있습니다.
  - 멀티쓰레드 환경에서 동기화를 보장한다.
- StringBuffer
  - 멀티쓰레드 환경에서 동기화를 보장하는 StringBuilder

```java
String str1 = "hello";
String str2 = "world";
StringBuilder str3 = new StringBuilder("hello");
StringBuilder str4 = new StringBuilder("world");

System.out.println("str1.hashCode() = " + str1.hashCode());
System.out.println("str2.hashCode() = " + str2.hashCode());
System.out.println("str1 + str2 = " + (str1 + str2).hashCode()); // 새로운 객체 생성
System.out.println("str3.hashCode() = " + str3.hashCode());
System.out.println("str3 + str4 =  " + str3.append(str4).hashCode()); // str3 객체에 값이 변경

/*
str1.hashCode() = 99162322
str2.hashCode() = 113318802
str1 + str2 = -1524582912
str3.hashCode() = 391447681
str4.hashCode() = 1935637221
str3 + str4 =  391447681 
*/
```

