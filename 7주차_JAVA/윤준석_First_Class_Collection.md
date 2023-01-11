# **🔥 일급 컬렉션**

## **👉 일급 컬렉션**

일급 컬렉션(first-class collection)이란 소트웍스 앤솔러지의 "객체지향 생활체조 원칙"에서 언급된 개념 중 하나이며, 컬렉션을 다루는 방식을 설명하는 개념이다.

### **객체지향 생활체조 원칙**

> 규칙 1. 한 메서드에 오직 한 단계의 들여쓰기(indent)만 한다.
>
> 규칙 2. else 예약어를 쓰지 않는다.
>
> 규칙 3. 모든 원시값과 문자열을 포장한다.
>
> 규칙 4. 한 줄에 점을 하나만 찍는다.
>
> 규칙 5. 줄여쓰지 않는다(축약 금지).
>
> 규칙 6. 모든 엔티티를 작게 유지한다.
>
> 규칙 7. 3개 이상의 인스턴스 변수를 가진 클래스를 쓰지 않는다.
>
> 규칙 8. 일급 컬렉션을 쓴다.
>
> 규칙 9. getter/setter/property를 쓰지 않는다.

코드를 객체지향적이고 리팩토링이 쉽게 작성하기 위한 규칙이며 이 중 8번째 항목이 일급 컬렉션이다. 본 규칙에서 제시하는 일급 컬렉션에 대한 설명은 다음과 같다.

```
규칙 8: 일급 컬렉션 사용
이 규칙의 적용은 간단하다.
컬렉션을 포함한 클래스는 반드시 다른 멤버 변수가 없어야 한다.
필터가 이 세 클래스의 일부가 됨을 알 수 있다.
필터는 또한 스스로 함수 객체가 될 수 있다.
또한 새 클래스는 두 그룹을 같이 묶는다든가 그룹의 각 원소에 규칙을 적용하는 등의 동작을 처리할 수 있다.
이는 인스턴스 변수에 대한 규칙의 확실한 확장이지만 그 자체를 위해서도 중요하다.
컬렉션은 실로 매우 유용한 원시 타입이다.
많은 동작이 있지만 후임 프로그래머나 유지보수 담당자에 의미적 의도나 단초는 거의 없다.
```

### **일급 컬렉션이란?**

#### **컬렉션(Collection)을 wrapping하면서, 그 외 다른 멤버 변수가 없는 상태를 일급 컬렉션이라 한다.**

간단한 코드로 살펴보면

```java
public class Person {
    private List<Account> accounts;
}
```

위 코드의 계좌 컬렉션(`List<Account>`)을 아래와 같이 wrapping하여 사용하는 것을 말한다.

```java
public class Person {
    private Accounts accounts;
}

public class Accounts {
    private List<Account> accounts;
}
```

일급 컬렉션은 다음과 같은 이점을 가진다.

1. 비즈니스에 종속적인 자료구조를 만들 수 있다.
2. 컬렉션의 불변성을 보장할 수 있다.
3. 상태와 행위를 한 곳에서 관리할 수 있다.
4. 이름이 있는 컬렉션을 만들 수 있다.

## **일급 객체(first-class object)의 개념**

**일급 객체**란 **함수형 프로그래밍**에서 사용하는 개념으로, **일급 시민**이라고도 한다. 일급 객체는 다음 조건을 만족하는 객체를 말한다.

1. 변수나 데이터에 할당할 수 있어야 한다.
2. 객체의 인자(파라미터)로 넘길 수 있어야 한다.
3. 객체의 반환값으로 사용할 수 있어야 한다.

자바에서는 일급 객체를 제한적으로 지원한다(lambda 표현식 또는 익명 클래스). 하지만 javascript와 같이 일급 객체를 지원하는 언어에서는 함수를 일급 객체로 취급하기 때문에, 함수를 변수에 할당하거나, 함수의 인자로 전달하거나, 함수의 반환값으로 사용할 수 있다.

```java
// 1. 변수에 할당
Consumer<String> c = (msg) -> System.out.println(msg);

// 2. 인자로 전달
public static void print(Consumer <String> action, String msg) {
    action.accept(msg)
}
print((msg) -> System.out.println(msg) , "Hello World");

//3. 반환값으로 사용
public static Object hello() {
    return new Object() {
        public String toString() {
            return "Hello World";
        }
    }
}
Object hello = hello();
System.out.println(hello.toString());
```

일급 컬렉션은 일급 객체와는 다른 개념이다.

## **👍 일급 컬렉션의 장점**

### **1. 비즈니스에 종속적인 자료구조**

비즈니스 로직을 구현하다 보면 특정 컬렉션의 검증 로직을 구현해야 하는 경우가 있다. 예를 들어, 계좌 정보를 담는 클래스가 있다고 가정하고 계좌 정보에 계좌 번호를 포함한다고 하자. 계좌 번호에는 다음과 같은 조건이 있다.

- 계좌번호는 10자리 숫자이다.
- 계좌번호는 연속된 숫자를 포함하지 않는다.

이런 경우 일급 컬렉션을 사용하면 비즈니스에 종속적인 자료구조를 만들 수 있다.

일급 컬렉션을 사용하지 않는 경우 일반적으로 검증 로직은 서비스 메서드에서 진행된다.

```java
public class Account {
    private List<Short> accountNumber;
    private String accountName;
    private String bank;
}

public class AccountService {
    private static final int ACCOUNT_NUMBER_SIZE = 10;

    public void createAccountNumber() {
        List<Short> accountNumber = createNonDuplicateContinuousNumbers();
        validateSize(accountNumber);
        validateDuplicateContinuous(accountNumber);

        /* ... */
    }

    private void validateSize(List<Short> accountNumber) {
        if (accountNumber.size() != ACCOUNT_NUMBER_SIZE) {
            throw new IllegalArgumentException("계좌번호는 10개의 숫자여야 합니다.");
        }
    }

    private void validateDuplicateContinuous(List<Short> accountNumber) {
        for (int i = 0; i < ACCOUNT_NUMBER_SIZE - 1; i++) {
            if (accountNumber.get(i).equals(accountNumber.get(i + 1))) {
                throw new IllegalArgumentException("계좌번호는 연속으로 중복된 숫자를 가질 수 없습니다.");
            }
        }
    }
}
```

위 코드에서는 서비스 메서드에서 비즈니스 로직을 처리하였다. 하지만 이러한 경우 계좌번호가 필요한 곳에서 검증 로직을 다시 구현해주어야 하는 문제가 발생한다. 즉, 컬렉션을 객체로 보지 않고 일반적인 자료구조로만 사용하고 있기 때문에 비즈니스에 종속적인 자료구조를 만들 수 없다. 만약 코드와 도메인에 대한 지식이 미흡한 개발자가 있다면 위 코드를 보고 유지보수하기 어려울 수 있다.

이러한 문제를 해결하기 위해선 계좌번호의 조건인 10가지 숫자로 이루어져 있고 연속적으로 중복된 숫자를 허용하지 않는 자료구조를 직접 만들면 된다. 이러한 자료구조 또는 클래스를 일급 컬렉션이라고 한다.

일급 컬렉션으로 구현해보면 다음과 같다.

```java
public class Account {
    private AccountNumber accountNumber;
    private String accountName;
    private String bank;
}

public class AccountNumber {
    private static final int ACCOUNT_NUMBER_SIZE = 10;

    private final List<Short> accountNumber;

    public AccountNumber(List<Short> accountNumber) {
        validateSize(accountNumber);
        validateDuplicateContinuous(accountNumber);
        this.accountNumber = accountNumber;
    }

    private void validateSize(List<Short> accountNumber) {
        if (accountNumber.size() != ACCOUNT_NUMBER_SIZE) {
            throw new IllegalArgumentException("계좌번호는 10개의 숫자여야 합니다.");
        }
    }

    private void validateDuplicateContinuous(List<Short> accountNumber) {
        for (int i = 0; i < accountNumber.size() - 1; i++) {
            if (accountNumber.get(i).equals(accountNumber.get(i + 1))) {
                throw new IllegalArgumentException("계좌번호는 연속으로 중복된 숫자를 가질 수 없습니다.");
            }
        }
    }
}

public class AccountServive {
    public void createAccountNumber() {
        AccountNumber accountNumber = new AccountNumber(createNonDuplicateContinuousNumbers())

        /* ... */
    }
}
```

위 코드처럼 일급 컬렉션을 사용하면 비즈니스에 종속적인 자료구조를 만들어 사용할 수 있다.

### **2. 불변성**

일급 컬렉션은 컬렉션의 불변을 보장한다. 자바에서 말하는 불변 객체란 무엇일까?

1. 객체를 변경하는 메서드를 제공하지 않는다.
2. 객체를 재정의할 수 있는 메서드를 제공하지 않는다.
3. 모든 필드를 final로 만든다.
4. 모든 필드를 private으로 만든다.

즉, 객체의 상태를 바꿀 수 없으므로 새로운 상태로 변경해야 할 경우 새로운 불변 객체를 만들어 기존의 불변 객체를 대체시켜야 한다. 이러한 불변객체는 side-effect 및 동시성 문제에 노출되지 않는다.

일반적인 컬렉션은 불변 객체로 볼 수 없다. `final` 키워드는 불변을 만들어주는 것이 아닌 재할당을 금지하는 것이다.

```java
@Test
public void final_변수_값_변경() {
    final List<Integer> numbers = new ArrayList<>();

    numbers.add(1);
    numbers.add(2);
    numbers.add(3);
    numbers.add(4);

    assertThat(numbers.size()).isEqualTo(4);
}
```

위 코드를 보면 `final` 키워드를 사용했음에도 불구하고 컬렉션의 값을 변경할 수 있다. 이는 `final` 키워드가 객체의 재할당만을 금지하는 것이기 때문이다.

자바에서 불변성을 가지는 객체를 만들기 위해선 일급 컬렉션 또는 래퍼 클래스를 사용해야 한다. 앞에서 설명한 일급 컬렉션인 계좌 번호 클래스를 보면,

```java
public class AccountNumber {
    private static final int ACCOUNT_NUMBER_SIZE = 10;

    private final List<Short> accountNumber;

    public AccountNumber(List<Short> accountNumber) {
        validateSize(accountNumber);
        validateDuplicateContinuous(accountNumber);
        this.accountNumber = accountNumber;
    }

    public String getAccountNumber() {
        return this.accountNumber.toString();
    }
}
```

위 계좌 번호 클래스는 불변성을 만족한다. 1) 객체을 변경하는 메서드를 제공하지 않고, 2) 객체를 재정의하는 메서드를 제공하지 않으며, 3) 모든 필드가 `final`이며, 모든 필드가 `private`이다.

이렇게 일급 컬렉션을 사용하면 불변성을 만족하는 컬렉션을 만들 수 있다.

### **3. 상태와 행위를 한 곳에서 관리**

일급 컬렉션의 세 번째 장점은 값과 로직이 함께 존재한다는 것이다.

예를 들어 여러 은행의 계좌들을 가지고 있고 특정 은행의 잔액 합계가 필요하다고 가정해보면,

```java
public class Account {
    private final Bank bank;
    private Long balance;

    public Account(Bank bank, Long balance) {
        this.bank = bank;
        this.balance = balance;
    }

    public Bank getBankType() {
        return this.bank;
    }

    public Long getBalance() {
        return this.balance;
    }
}

@Test
public void 상태와_행위가_분리() {
    List<Account> accounts = Arrays.asList(
            new Account(Bank.HANA, 2000000L),
            new Account(Bank.KB, 1600000L),
            new Account(Bank.HANA, 500000L),
            new Account(Bank.NH, 700000L)
    );

    Long HanaBankSum = accounts.stream()
            .filter(account -> account.getBankType().equals(Bank.HANA))
            .mapToLong(Account::getBalance)
            .sum();

    assertThat(HanaBankSum).isEqualTo(2500000L);
}
```

일급 컬렉션을 사용하지 않으면 일반적으로 `List`에 데이터를 담고, 서비스 메서드에서 필요한 로직을 수행한다. 이렇게 되면 `accounts`라는 컬렉션과 계산하는 로직을 서로 관련이 있는데, 코드를 통해 이를 표현할 수 없다.

만약 은행 별로 다른 계산 메서드를 사용해야 하는 경우 은행 별로 해당 메서드를 강제할 수 있는 방법이 없다. 이는 똑같은 기능을 하는 메서드를 중복으로 구현하거나 계산 메서드를 누락하는 실수를 발생시킬 수 있다.

하지만 일급 컬렉션을 사용하면 이러한 문제를 해결할 수 있다.

```java
public class Accounts {
    private List<Account> accounts;

    public Accounts(List<Account> accounts) {
        this.accounts = accounts;
    }

    public Long getHanaBankSum() {
        return accounts.stream()
                .filter(account -> account.getBankType().equals(Bank.HANA))
                .mapToLong(Account::getBalance)
                .sum();
    }
}
```

만약 다른 은행들의 계산 메서드들이 필요하다면 `Accounts` 클래스에 추가하면 된다. 이렇게 되면 `Accounts` 클래스는 은행 별 계산 메서드를 강제할 수 있다. 이는 람다식으로 추가할 수 있다.

```java
public class Accounts {
    private List<Account> accounts;

    public Accounts(List<Account> accounts) {
        this.accounts = accounts;
    }

    public Long getHanaBankSum() {
        return getFilteredSum(account -> Bank.isHanaBank(account.getBankType()));
    }

    public Long getKbBankSum() {
        return getFilteredSum(account -> Bank.isKbBank(account.getBankType()));
    }

    public Long getNhBankSum() {
        return getFilteredSum(account -> Bank.isNhBank(account.getBankType()));
    }

    private Long getFilteredSum(Predicate<Account> predicate) {
        return accounts.stream()
                .filter(predicate)
                .mapToLong(Account::getBalance)
                .sum();
    }
}
```

이렇게 되면 `Accounts`라는 일급 컬렉션을 통해 상태와 행위를 한 곳에서 관리할 수 있다.

```java
@Test
public void 상태와_행위를_한곳에서() {
    Accounts accounts = new Accounts(Arrays.asList(
            new Account(Bank.HANA, 2000000L),
            new Account(Bank.KB, 1600000L),
            new Account(Bank.HANA, 500000L),
            new Account(Bank.NH, 700000L)
    ));

    Long HanaBankSum = accounts.getHanaBankSum();

    assertThat(HanaBankSum).isEqualTo(2500000L);
}
```

### **4. 이름이 있는 컬렉션**

일급 컬렉션의 마지막 장점은 컬렉션에 이름을 붙일 수 있다는 것이다.

예를 들어, 은행 별로 계좌를 모아 둔 List들을 관리한다고 가정해보자. 하나은행 계좌 리스트와 국민은행 계좌 리스트는 서로 다른데, 이를 구별하는 가장 쉬운 방법은 변수 이름을 다르게 하는 것이다.

```java
@Test
public void 변수명으로_컬렉션_구별() {
    List<Account> HanaBankAccounts = createHanaBankAccounts();
    List<Account> KbBankAccounts = createKbBankAccounts();
}
```

이렇게 되면 작은 규모의 개인 프로젝트에서는 큰 문제가 없지만 대형 프로젝트의 경우 문제가 발생한다.

1. 검색이 어려워진다.
   - 하나은행 계좌 리스트가 어떻게 사용되는지 변수명으로만 검색할 수 있다.
2. 명확한 표현이 어렵다.
   - 변수명에 의미를 부여하기 어렵다. 개발팀과 운영팀 간에 보편적인 언어로 사용하기 어렵다.

이러한 문제점을 일급 컬렉션을 사용하여 해결할 수 있다. 각각 계좌 리스트의 일급 컬렉션을 만들고 해당 클래스 이름 기반으로 용어 사용 및 검색을 하면 된다.

```java
@Test
public void 일급컬렉션으로_이름_부여() {
    HanaBankAccounts hanaBankAccounts = new HanaBankAccounts(createHanaBankAccounts());
    KbBankAccounts kbBankAccounts = new KbBankAccounts(createKbBankAccounts());
}
```
