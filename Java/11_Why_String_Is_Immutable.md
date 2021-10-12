# Why String is Immutable??

 자바에서 `String` 객체는 `Immutable` 합니다. 즉, 수정이 불가능하도록 설계했습니다. 왜 이렇게 설계를 했을까요??

### 1) 메모리 절약

 문자열은 같은 문자열이 반복되서 사용될 가능성이 큽니다. 예를 들어, Google에 A라는 검색어는 다른 요청에서도 굉장히 많이 등장할 것이고 매번 새로운 객체를 할당하는 것이 아니라, 한 번 할당해놓은 객체를 사용해서 메모리 낭비를 막을 수 있습니다.

### 2) 캐싱

 `String`의 `hashcode`는 불변이기 때문에 언제나 같으므로 캐싱을 통해 더욱 빠르게 조회할 수 있습니다.

### 3) Multi Thread 동기화 문제 해결

 `String`은 불변이므로 `Multi Thread` 상황에서도 안전합니다.

### 4) 보안기능

 `String`은 자바의 파라미터로 폭넓게 네트워크 커넥션, 파일 가져오기, JDBC 드라이버명 등에서 사용되고 `Immutable` 이므로 보안상에도 이점이 있습니다.

### 5) String Pool

 JVM의 `Heap` 영역 안에 `String Pool` 이라는 영역이 있습니다. `String`은 `Immutable`이기 때문에 `String Pool` 안에서 관리를 하므로 메모리 절약 및 성능 향상의 이점이 있습니다.

참고

https://www.baeldung.com/java-string-immutable