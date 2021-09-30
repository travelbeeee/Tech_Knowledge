### 04) equals, ==

공통적으로 둘다 모두 양 쪽에 있는 내용을 비교한 후에 boolean type으로 반환한다.

#### [ == ]

`==` 비교는 단순히 주소값을 비교하는 연산입니다. 즉, 비교 대상이 서로 같은 곳을 가리키고 있는지 물어보는 연산입니다.

자바에서 `Primitive Type` 은 그 안에 담겨있는 `Value`가 같다면 모두 같은 주소를 가리키고 있으므로 `Primitive Type` 을 비교할 때는 `==` 연산을 사용해도 값 비교를 할 수 있습니다. 하지만, `Primitive Type`이 아닌 객체들은 그 안에 담겨있는 `Value`가 같더라도 주소가 다를 수 있습니다. 따라서, `==` 을 이용해 비교를 하게 되면 두 객체의 내용이 아니라 단순히 가리키고 있는 주소만 비교하게 됩니다. 이를 해결하기 위해 모든 객체는 `equals` 메소드를 가지고 있습니다.

<br>

#### [ equals ]

`equals` 메소드는 자바 최상위 클래스인 `Object` 클래스에 정의되어있는 메소드입니다.

```java
boolean equals(Obejct obj)
```

`boolean equals(Object obj)`로 정의된 `equals `메소드는 2개의 객체가 동일한지 검사하기 위해 사용된다. 

기본적으로 eqauls가 구현된 방법은 2개의 객체가 참조하는 것이 동일한지를 확인하는 것이다.

즉, 2개의 객체가 가리키는 곳이 동일한 메모리 주소일 경우에만 동일한 객체가 된다.

하지만, `String` 클래스를 생각해보면 `equals` 메소드를 이용해 동일한 객체를 참조하는 것인지가 아니라, 안에 있는 문자열이 같은지 확인을 할 수 있다. 이처럼 `equals` 메소드를 오버라이딩해서 상황에 맞게 사용할 수 있습니다.

<br>

#### [ hashCode ]

`hashCode` 메소드는 `equals` 메소드와 마찬가지로 `Object` 클래스에 정의되어있는 메소드입니다.

```java
public native int hashCode();
```

기본적으로 heap에 저장된 객체의 메모리 주소를 반환하도록 되어있습니다.

<br>

#### [ equals와 hashCode의 관계 ]

동일한 객체는 동일한 메모리 주소를 갖는다는 것을 의미하므로, 동일한 객체는 동일한 해시코드를 가져야 한다. 또, `HashSet`, `HashMap`, `Hashtable`과 같은 프레임워크에서 객체의 비교시 `hashCode()` 결과값을 통해 비교하기 때문에 `equals()`를 오버라이드 할 때, `hashCode()`도 오버라이드 해주는 것이 좋다.

- hashCode() 값이 같다면, equals() 결과는 true여야한다.

<br>

#### [ 예시 ]

```java
int num1 = 1;
int num2 = 1;
String str1 = "aaa";
String str2 = new String("aaa");
System.out.println("str1 == str2 : " + (str1 == str2));
System.out.println("str1 equals str2 : " + (str1.equals(str2)));
System.out.println("num1 == num2 : " + (num1 == num2));
// 출력
str1 == str2 : false
str1 equals str2 : true
num1 == num2 : true
```

==연산자와 String클래스의 equals()메소드의 가장 큰 차이점은 == 연산자는 비교하고자 하는 두개의 대상의 주소값을 비교하는데 반해 String클래스의 equals 메소드는 비교하고자 하는 두개의 대상의 값 자체를 비교합니다. 

일반적인 타입들 int형, char형등은 Call by Value 형태로 기본적으로 대상에 주소값을 가지지 않는 형태로 사용됩니다. 하지만 String은 일반적인 타입이 아니라 클래스입니다. 클래스는 기본적으로 Call by Reference형태로 생성 시 주소값이 부여됩니다. 그렇기에 String타입을 선언했을때는 같은 값을 부여하더라도 서로간의 주소값이 다를 수가 있습니다.