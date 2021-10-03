# Java 접근 수준 지시자

자바에서 지원하는 네 가지 종류의 '접근 수준 지시자' 인 public, protected, private, default 지시자에 대해서 다뤄보겠습니다.

### 1) Private 지시자

 `private` 지시자는 **정보 은닉을 위해 사용**합니다. 자바에서 말하는 '정보'란 클래스의 '인스턴스 변수' 를 의미합니다. **`private` 지시자를 이용하면 인스턴스 변수 혹은 인스턴스 메소드를 클래스 내부에서만 접근이 가능하도록 할 수 있습니다.** 그럼 왜 `private` 지시자가 등장했는지 다음 예제를 통해서 이해해보겠습니다.

```java
public class BankAccount {
    public String accountInfo; // 계좌번호
    public int balance; // 현재 잔액
    // 생성자
    public BankAccount(String info, int money){
        accountInfo = info;
        balance = money;
    }
    // 계좌 번호를 알려주는 메소드(기능)
    public void printAccountInfo() {
        System.out.println("게좌 번호는 : " + accountInfo + "입니다");
    }
    // 잔고를 알려주는 메소드(기능)
    public void printBalance() {
        System.out.println("현재 계좌 잔고는 : " + balance + "입니다");
    }
    // 입금하는 메소드(기능)
    public void deposit(int amount) {
        System.out.println(amount + "원을 입금하셨습니다.");
        balance += amount;
    }
    // 출금하는 메소드(기능)
    public void withdraw(int amount) {
        if(amount > balance)
            System.out.println("잔고보다 많은 금액을 인출하려고 합니다.");
        else {
            System.out.println(amount + "원을 출금하셨습니다.");
            balance -= amount;
        }
    }
}
```

 은행 계좌 클래스입니다. 인스턴스 변수로는 계좌번호를 나타내는 String 변수와 현재 잔액을 나타내는 int 변수가 있습니다. 또, 생성자를 이용해 인스턴스를 생성하며 변수를 초기화하고 있는 것을 볼 수 있습니다.

 우리는 balance 변수가 잔액을 나타내고 있으므로, 음수가 아니길 원합니다. ( 물론, 잔고가 마이너스 일 수도 있지만, 마이너스 통장이 아니라고 가정하겠습니다. ) 하지만, 우리는 다음과 같이 balance 변수를 음수로 초기화 할 수 있습니다.

```java
BankAccount account = new BankAccount("1002-000-0000", -1000);
account.printAccountInfo();
account.printBalance();
```

  이건 우리가 원하는 상황이 아니므로, 다음과 같이 메소드를 추가하고 생성자를 수정해 balance 변수에 음수를 넣을 수 없게 만들어보겠습니다.

```java
public BankAccount(String info, int money){
    accountInfo = info;
    balance = setBalance(money);
}
public int setBalance(int money) {
    if(money < 0) return 0;
    else return money;
}
```

 생성자 함수에서 입력 받는 초기 값을 setBalance 메소드를 통해서 음수라면 0으로 return 하도록 만들었습니다. 이러면 우리가 인스턴스 생성 시 음수 값을 입력하더라도 우리가 원하는대로 작동한다고 생각할 수 있습니다. 하지만, 다음과 같이 인스턴스를 생성하고 변수에 접근해 값을 변경한다면 막을 수 없습니다.

```java
BankAccount account = new BankAccount("1002-000-0000", 10000);
account.balance = -1000;
```

 이때, `private` 지시자를 활용해야합니다.  **정보 은닉을 원하는 인스턴스 변수에 `private` 지시자를 이용하면 클래스 내부에서만 접근이 가능합니다. 즉, setBalance 메소드와 생성자를 제외하고 balance 변수에 접근할 수 없게 되므로, 우리는 항상 balance 변수에 0 또는 양수가 저장되는 것을 보장받을 수 있습니다.**

 이처럼 **private 지시자를 이용하면 정보 은닉을 할 수 있고, 정보 은닉은 객체지향언어에서 굉장히 중요한 부분 중 하나입니다.**

<br>

### 2) Default 지시자

 `Default`지시자는 클래스 정의, 인스턴스 멤버인 인스턴스 변수, 인스턴스 메소드를 대상으로 사용할 수 있습니다. 이름 그대로 아무런 지시자가 없을 때, 기본적으로 자바 컴파일러가 `Default` 지시자를 적용합니다.

 `Default` 지시자는 클래스 내부와 동일 패키지에서 자유롭게 접근이 가능합니다. `Default` 지시자를 이용해 클래스를 정의하면 같은 패키지 내에서만 인스턴스 생성이 가능합니다.

<br>

### 3) Protected 지시자

 `protected` 지시자는 인스턴스 멤버를 대상으로만 선언이 가능합니다. protected 선언은 default 선언이 허용하는 접근을 모두 허용해 클래스 내부, 동일 패키지에서 접근이 가능하고 추가로 상속 받은 클래스에서 접근을 가능하게 해줍니다.

```java
package package1;

public class A{
	int num; // default 지시자
}

package package2;

public class B extends package1.A{
	public void Hello(int n) {
		num = n; // default 지시자로 선언된 상속된 변수를 접근 --> 에러
	}
}
```

 다음과 같이 서로 다른 2개의 package가 있다고 합시다. class B는 package1의 class A를 상속하지만, 인스턴스 변수인 num 이 default 지시자로 다른 패키지 내에서는 접근이 불가능합니다. 따라서 위의 코드는 에러가 발생합니다. 이때, protected 지시자를 이용하면 문제를 해결할 수 있습니다.

 **protceted 지시자를 이용하면 다른 패키지 내에서도 상속 관계라면 인스턴스 멤버에 접근이 가능합니다!**

<br>

### 4) Public 지시자

 `Public` 지시자는 클래스 정의, 인스턴스 멤버인 인스턴스 변수, 인스턴스 메소드를 대상으로 사용할 수 있습니다. 

  클래스에 public이 정의가 된다면 이는 위치에 상관없이 어디서든 해당 클래스의 인스턴스를 생성할 수 있다는 뜻입니다. **즉, public 지시자를 이용해 클래스를 정의하면 같은 패키지가 아니더라도 어디서든 인스턴스 생성이 가능합니다! ** 인스턴스 변수, 메소드도 마찬가지입니다. 어디에서든 사용이 가능합니다.

<br>