# **π₯ μΌκΈ μ»¬λ μ**

## **π μΌκΈ μ»¬λ μ**

μΌκΈ μ»¬λ μ(first-class collection)μ΄λ μνΈμμ€ μ€μλ¬μ§μ "κ°μ²΄μ§ν₯ μνμ²΄μ‘° μμΉ"μμ μΈκΈλ κ°λ μ€ νλμ΄λ©°, μ»¬λ μμ λ€λ£¨λ λ°©μμ μ€λͺνλ κ°λμ΄λ€.

### **κ°μ²΄μ§ν₯ μνμ²΄μ‘° μμΉ**

> κ·μΉ 1. ν λ©μλμ μ€μ§ ν λ¨κ³μ λ€μ¬μ°κΈ°(indent)λ§ νλ€.
>
> κ·μΉ 2. else μμ½μ΄λ₯Ό μ°μ§ μλλ€.
>
> κ·μΉ 3. λͺ¨λ  μμκ°κ³Ό λ¬Έμμ΄μ ν¬μ₯νλ€.
>
> κ·μΉ 4. ν μ€μ μ μ νλλ§ μ°λλ€.
>
> κ·μΉ 5. μ€μ¬μ°μ§ μλλ€(μΆμ½ κΈμ§).
>
> κ·μΉ 6. λͺ¨λ  μν°ν°λ₯Ό μκ² μ μ§νλ€.
>
> κ·μΉ 7. 3κ° μ΄μμ μΈμ€ν΄μ€ λ³μλ₯Ό κ°μ§ ν΄λμ€λ₯Ό μ°μ§ μλλ€.
>
> κ·μΉ 8. μΌκΈ μ»¬λ μμ μ΄λ€.
>
> κ·μΉ 9. getter/setter/propertyλ₯Ό μ°μ§ μλλ€.

μ½λλ₯Ό κ°μ²΄μ§ν₯μ μ΄κ³  λ¦¬ν©ν λ§μ΄ μ½κ² μμ±νκΈ° μν κ·μΉμ΄λ©° μ΄ μ€ 8λ²μ§Έ ν­λͺ©μ΄ μΌκΈ μ»¬λ μμ΄λ€. λ³Έ κ·μΉμμ μ μνλ μΌκΈ μ»¬λ μμ λν μ€λͺμ λ€μκ³Ό κ°λ€.

```
κ·μΉ 8: μΌκΈ μ»¬λ μ μ¬μ©
μ΄ κ·μΉμ μ μ©μ κ°λ¨νλ€.
μ»¬λ μμ ν¬ν¨ν ν΄λμ€λ λ°λμ λ€λ₯Έ λ©€λ² λ³μκ° μμ΄μΌ νλ€.
νν°κ° μ΄ μΈ ν΄λμ€μ μΌλΆκ° λ¨μ μ μ μλ€.
νν°λ λν μ€μ€λ‘ ν¨μ κ°μ²΄κ° λ  μ μλ€.
λν μ ν΄λμ€λ λ κ·Έλ£Ήμ κ°μ΄ λ¬Άλλ€λ κ° κ·Έλ£Ήμ κ° μμμ κ·μΉμ μ μ©νλ λ±μ λμμ μ²λ¦¬ν  μ μλ€.
μ΄λ μΈμ€ν΄μ€ λ³μμ λν κ·μΉμ νμ€ν νμ₯μ΄μ§λ§ κ·Έ μμ²΄λ₯Ό μν΄μλ μ€μνλ€.
μ»¬λ μμ μ€λ‘ λ§€μ° μ μ©ν μμ νμμ΄λ€.
λ§μ λμμ΄ μμ§λ§ νμ νλ‘κ·Έλλ¨Έλ μ μ§λ³΄μ λ΄λΉμμ μλ―Έμ  μλλ λ¨μ΄λ κ±°μ μλ€.
```

### **μΌκΈ μ»¬λ μμ΄λ?**

#### **μ»¬λ μ(Collection)μ wrappingνλ©΄μ, κ·Έ μΈ λ€λ₯Έ λ©€λ² λ³μκ° μλ μνλ₯Ό μΌκΈ μ»¬λ μμ΄λΌ νλ€.**

κ°λ¨ν μ½λλ‘ μ΄ν΄λ³΄λ©΄

```java
public class Person {
    private List<Account> accounts;
}
```

μ μ½λμ κ³μ’ μ»¬λ μ(`List<Account>`)μ μλμ κ°μ΄ wrappingνμ¬ μ¬μ©νλ κ²μ λ§νλ€.

```java
public class Person {
    private Accounts accounts;
}

public class Accounts {
    private List<Account> accounts;
}
```

μΌκΈ μ»¬λ μμ λ€μκ³Ό κ°μ μ΄μ μ κ°μ§λ€.

1. λΉμ¦λμ€μ μ’μμ μΈ μλ£κ΅¬μ‘°λ₯Ό λ§λ€ μ μλ€.
2. μ»¬λ μμ λΆλ³μ±μ λ³΄μ₯ν  μ μλ€.
3. μνμ νμλ₯Ό ν κ³³μμ κ΄λ¦¬ν  μ μλ€.
4. μ΄λ¦μ΄ μλ μ»¬λ μμ λ§λ€ μ μλ€.

## **μΌκΈ κ°μ²΄(first-class object)μ κ°λ**

**μΌκΈ κ°μ²΄**λ **ν¨μν νλ‘κ·Έλλ°**μμ μ¬μ©νλ κ°λμΌλ‘, **μΌκΈ μλ―Ό**μ΄λΌκ³ λ νλ€. μΌκΈ κ°μ²΄λ λ€μ μ‘°κ±΄μ λ§μ‘±νλ κ°μ²΄λ₯Ό λ§νλ€.

1. λ³μλ λ°μ΄ν°μ ν λΉν  μ μμ΄μΌ νλ€.
2. κ°μ²΄μ μΈμ(νλΌλ―Έν°)λ‘ λκΈΈ μ μμ΄μΌ νλ€.
3. κ°μ²΄μ λ°νκ°μΌλ‘ μ¬μ©ν  μ μμ΄μΌ νλ€.

μλ°μμλ μΌκΈ κ°μ²΄λ₯Ό μ νμ μΌλ‘ μ§μνλ€(lambda ννμ λλ μ΅λͺ ν΄λμ€). νμ§λ§ javascriptμ κ°μ΄ μΌκΈ κ°μ²΄λ₯Ό μ§μνλ μΈμ΄μμλ ν¨μλ₯Ό μΌκΈ κ°μ²΄λ‘ μ·¨κΈνκΈ° λλ¬Έμ, ν¨μλ₯Ό λ³μμ ν λΉνκ±°λ, ν¨μμ μΈμλ‘ μ λ¬νκ±°λ, ν¨μμ λ°νκ°μΌλ‘ μ¬μ©ν  μ μλ€.

```java
// 1. λ³μμ ν λΉ
Consumer<String> c = (msg) -> System.out.println(msg);

// 2. μΈμλ‘ μ λ¬
public static void print(Consumer <String> action, String msg) {
    action.accept(msg)
}
print((msg) -> System.out.println(msg) , "Hello World");

//3. λ°νκ°μΌλ‘ μ¬μ©
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

μΌκΈ μ»¬λ μμ μΌκΈ κ°μ²΄μλ λ€λ₯Έ κ°λμ΄λ€.

## **π μΌκΈ μ»¬λ μμ μ₯μ **

### **1. λΉμ¦λμ€μ μ’μμ μΈ μλ£κ΅¬μ‘°**

λΉμ¦λμ€ λ‘μ§μ κ΅¬ννλ€ λ³΄λ©΄ νΉμ  μ»¬λ μμ κ²μ¦ λ‘μ§μ κ΅¬νν΄μΌ νλ κ²½μ°κ° μλ€. μλ₯Ό λ€μ΄, κ³μ’ μ λ³΄λ₯Ό λ΄λ ν΄λμ€κ° μλ€κ³  κ°μ νκ³  κ³μ’ μ λ³΄μ κ³μ’ λ²νΈλ₯Ό ν¬ν¨νλ€κ³  νμ. κ³μ’ λ²νΈμλ λ€μκ³Ό κ°μ μ‘°κ±΄μ΄ μλ€.

- κ³μ’λ²νΈλ 10μλ¦¬ μ«μμ΄λ€.
- κ³μ’λ²νΈλ μ°μλ μ«μλ₯Ό ν¬ν¨νμ§ μλλ€.

μ΄λ° κ²½μ° μΌκΈ μ»¬λ μμ μ¬μ©νλ©΄ λΉμ¦λμ€μ μ’μμ μΈ μλ£κ΅¬μ‘°λ₯Ό λ§λ€ μ μλ€.

μΌκΈ μ»¬λ μμ μ¬μ©νμ§ μλ κ²½μ° μΌλ°μ μΌλ‘ κ²μ¦ λ‘μ§μ μλΉμ€ λ©μλμμ μ§νλλ€.

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
            throw new IllegalArgumentException("κ³μ’λ²νΈλ 10κ°μ μ«μμ¬μΌ ν©λλ€.");
        }
    }

    private void validateDuplicateContinuous(List<Short> accountNumber) {
        for (int i = 0; i < ACCOUNT_NUMBER_SIZE - 1; i++) {
            if (accountNumber.get(i).equals(accountNumber.get(i + 1))) {
                throw new IllegalArgumentException("κ³μ’λ²νΈλ μ°μμΌλ‘ μ€λ³΅λ μ«μλ₯Ό κ°μ§ μ μμ΅λλ€.");
            }
        }
    }
}
```

μ μ½λμμλ μλΉμ€ λ©μλμμ λΉμ¦λμ€ λ‘μ§μ μ²λ¦¬νμλ€. νμ§λ§ μ΄λ¬ν κ²½μ° κ³μ’λ²νΈκ° νμν κ³³μμ κ²μ¦ λ‘μ§μ λ€μ κ΅¬νν΄μ£Όμ΄μΌ νλ λ¬Έμ κ° λ°μνλ€. μ¦, μ»¬λ μμ κ°μ²΄λ‘ λ³΄μ§ μκ³  μΌλ°μ μΈ μλ£κ΅¬μ‘°λ‘λ§ μ¬μ©νκ³  μκΈ° λλ¬Έμ λΉμ¦λμ€μ μ’μμ μΈ μλ£κ΅¬μ‘°λ₯Ό λ§λ€ μ μλ€. λ§μ½ μ½λμ λλ©μΈμ λν μ§μμ΄ λ―Έν‘ν κ°λ°μκ° μλ€λ©΄ μ μ½λλ₯Ό λ³΄κ³  μ μ§λ³΄μνκΈ° μ΄λ €μΈ μ μλ€.

μ΄λ¬ν λ¬Έμ λ₯Ό ν΄κ²°νκΈ° μν΄μ  κ³μ’λ²νΈμ μ‘°κ±΄μΈ 10κ°μ§ μ«μλ‘ μ΄λ£¨μ΄μ Έ μκ³  μ°μμ μΌλ‘ μ€λ³΅λ μ«μλ₯Ό νμ©νμ§ μλ μλ£κ΅¬μ‘°λ₯Ό μ§μ  λ§λ€λ©΄ λλ€. μ΄λ¬ν μλ£κ΅¬μ‘° λλ ν΄λμ€λ₯Ό μΌκΈ μ»¬λ μμ΄λΌκ³  νλ€.

μΌκΈ μ»¬λ μμΌλ‘ κ΅¬νν΄λ³΄λ©΄ λ€μκ³Ό κ°λ€.

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
            throw new IllegalArgumentException("κ³μ’λ²νΈλ 10κ°μ μ«μμ¬μΌ ν©λλ€.");
        }
    }

    private void validateDuplicateContinuous(List<Short> accountNumber) {
        for (int i = 0; i < accountNumber.size() - 1; i++) {
            if (accountNumber.get(i).equals(accountNumber.get(i + 1))) {
                throw new IllegalArgumentException("κ³μ’λ²νΈλ μ°μμΌλ‘ μ€λ³΅λ μ«μλ₯Ό κ°μ§ μ μμ΅λλ€.");
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

μ μ½λμ²λΌ μΌκΈ μ»¬λ μμ μ¬μ©νλ©΄ λΉμ¦λμ€μ μ’μμ μΈ μλ£κ΅¬μ‘°λ₯Ό λ§λ€μ΄ μ¬μ©ν  μ μλ€.

### **2. λΆλ³μ±**

μΌκΈ μ»¬λ μμ μ»¬λ μμ λΆλ³μ λ³΄μ₯νλ€. μλ°μμ λ§νλ λΆλ³ κ°μ²΄λ λ¬΄μμΌκΉ?

1. κ°μ²΄λ₯Ό λ³κ²½νλ λ©μλλ₯Ό μ κ³΅νμ§ μλλ€.
2. κ°μ²΄λ₯Ό μ¬μ μν  μ μλ λ©μλλ₯Ό μ κ³΅νμ§ μλλ€.
3. λͺ¨λ  νλλ₯Ό finalλ‘ λ§λ λ€.
4. λͺ¨λ  νλλ₯Ό privateμΌλ‘ λ§λ λ€.

μ¦, κ°μ²΄μ μνλ₯Ό λ°κΏ μ μμΌλ―λ‘ μλ‘μ΄ μνλ‘ λ³κ²½ν΄μΌ ν  κ²½μ° μλ‘μ΄ λΆλ³ κ°μ²΄λ₯Ό λ§λ€μ΄ κΈ°μ‘΄μ λΆλ³ κ°μ²΄λ₯Ό λμ²΄μμΌμΌ νλ€. μ΄λ¬ν λΆλ³κ°μ²΄λ side-effect λ° λμμ± λ¬Έμ μ λΈμΆλμ§ μλλ€.

μΌλ°μ μΈ μ»¬λ μμ λΆλ³ κ°μ²΄λ‘ λ³Ό μ μλ€. `final` ν€μλλ λΆλ³μ λ§λ€μ΄μ£Όλ κ²μ΄ μλ μ¬ν λΉμ κΈμ§νλ κ²μ΄λ€.

```java
@Test
public void final_λ³μ_κ°_λ³κ²½() {
    final List<Integer> numbers = new ArrayList<>();

    numbers.add(1);
    numbers.add(2);
    numbers.add(3);
    numbers.add(4);

    assertThat(numbers.size()).isEqualTo(4);
}
```

μ μ½λλ₯Ό λ³΄λ©΄ `final` ν€μλλ₯Ό μ¬μ©νμμλ λΆκ΅¬νκ³  μ»¬λ μμ κ°μ λ³κ²½ν  μ μλ€. μ΄λ `final` ν€μλκ° κ°μ²΄μ μ¬ν λΉλ§μ κΈμ§νλ κ²μ΄κΈ° λλ¬Έμ΄λ€.

μλ°μμ λΆλ³μ±μ κ°μ§λ κ°μ²΄λ₯Ό λ§λ€κΈ° μν΄μ  μΌκΈ μ»¬λ μ λλ λνΌ ν΄λμ€λ₯Ό μ¬μ©ν΄μΌ νλ€. μμμ μ€λͺν μΌκΈ μ»¬λ μμΈ κ³μ’ λ²νΈ ν΄λμ€λ₯Ό λ³΄λ©΄,

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

μ κ³μ’ λ²νΈ ν΄λμ€λ λΆλ³μ±μ λ§μ‘±νλ€. 1) κ°μ²΄μ λ³κ²½νλ λ©μλλ₯Ό μ κ³΅νμ§ μκ³ , 2) κ°μ²΄λ₯Ό μ¬μ μνλ λ©μλλ₯Ό μ κ³΅νμ§ μμΌλ©°, 3) λͺ¨λ  νλκ° `final`μ΄λ©°, λͺ¨λ  νλκ° `private`μ΄λ€.

μ΄λ κ² μΌκΈ μ»¬λ μμ μ¬μ©νλ©΄ λΆλ³μ±μ λ§μ‘±νλ μ»¬λ μμ λ§λ€ μ μλ€.

### **3. μνμ νμλ₯Ό ν κ³³μμ κ΄λ¦¬**

μΌκΈ μ»¬λ μμ μΈ λ²μ§Έ μ₯μ μ κ°κ³Ό λ‘μ§μ΄ ν¨κ» μ‘΄μ¬νλ€λ κ²μ΄λ€.

μλ₯Ό λ€μ΄ μ¬λ¬ μνμ κ³μ’λ€μ κ°μ§κ³  μκ³  νΉμ  μνμ μμ‘ ν©κ³κ° νμνλ€κ³  κ°μ ν΄λ³΄λ©΄,

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
public void μνμ_νμκ°_λΆλ¦¬() {
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

μΌκΈ μ»¬λ μμ μ¬μ©νμ§ μμΌλ©΄ μΌλ°μ μΌλ‘ `List`μ λ°μ΄ν°λ₯Ό λ΄κ³ , μλΉμ€ λ©μλμμ νμν λ‘μ§μ μννλ€. μ΄λ κ² λλ©΄ `accounts`λΌλ μ»¬λ μκ³Ό κ³μ°νλ λ‘μ§μ μλ‘ κ΄λ ¨μ΄ μλλ°, μ½λλ₯Ό ν΅ν΄ μ΄λ₯Ό ννν  μ μλ€.

λ§μ½ μν λ³λ‘ λ€λ₯Έ κ³μ° λ©μλλ₯Ό μ¬μ©ν΄μΌ νλ κ²½μ° μν λ³λ‘ ν΄λΉ λ©μλλ₯Ό κ°μ ν  μ μλ λ°©λ²μ΄ μλ€. μ΄λ λκ°μ κΈ°λ₯μ νλ λ©μλλ₯Ό μ€λ³΅μΌλ‘ κ΅¬ννκ±°λ κ³μ° λ©μλλ₯Ό λλ½νλ μ€μλ₯Ό λ°μμν¬ μ μλ€.

νμ§λ§ μΌκΈ μ»¬λ μμ μ¬μ©νλ©΄ μ΄λ¬ν λ¬Έμ λ₯Ό ν΄κ²°ν  μ μλ€.

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

λ§μ½ λ€λ₯Έ μνλ€μ κ³μ° λ©μλλ€μ΄ νμνλ€λ©΄ `Accounts` ν΄λμ€μ μΆκ°νλ©΄ λλ€. μ΄λ κ² λλ©΄ `Accounts` ν΄λμ€λ μν λ³ κ³μ° λ©μλλ₯Ό κ°μ ν  μ μλ€. μ΄λ λλ€μμΌλ‘ μΆκ°ν  μ μλ€.

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

μ΄λ κ² λλ©΄ `Accounts`λΌλ μΌκΈ μ»¬λ μμ ν΅ν΄ μνμ νμλ₯Ό ν κ³³μμ κ΄λ¦¬ν  μ μλ€.

```java
@Test
public void μνμ_νμλ₯Ό_νκ³³μμ() {
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

### **4. μ΄λ¦μ΄ μλ μ»¬λ μ**

μΌκΈ μ»¬λ μμ λ§μ§λ§ μ₯μ μ μ»¬λ μμ μ΄λ¦μ λΆμΌ μ μλ€λ κ²μ΄λ€.

μλ₯Ό λ€μ΄, μν λ³λ‘ κ³μ’λ₯Ό λͺ¨μ λ Listλ€μ κ΄λ¦¬νλ€κ³  κ°μ ν΄λ³΄μ. νλμν κ³μ’ λ¦¬μ€νΈμ κ΅­λ―Όμν κ³μ’ λ¦¬μ€νΈλ μλ‘ λ€λ₯Έλ°, μ΄λ₯Ό κ΅¬λ³νλ κ°μ₯ μ¬μ΄ λ°©λ²μ λ³μ μ΄λ¦μ λ€λ₯΄κ² νλ κ²μ΄λ€.

```java
@Test
public void λ³μλͺμΌλ‘_μ»¬λ μ_κ΅¬λ³() {
    List<Account> HanaBankAccounts = createHanaBankAccounts();
    List<Account> KbBankAccounts = createKbBankAccounts();
}
```

μ΄λ κ² λλ©΄ μμ κ·λͺ¨μ κ°μΈ νλ‘μ νΈμμλ ν° λ¬Έμ κ° μμ§λ§ λν νλ‘μ νΈμ κ²½μ° λ¬Έμ κ° λ°μνλ€.

1. κ²μμ΄ μ΄λ €μμ§λ€.
   - νλμν κ³μ’ λ¦¬μ€νΈκ° μ΄λ»κ² μ¬μ©λλμ§ λ³μλͺμΌλ‘λ§ κ²μν  μ μλ€.
2. λͺνν ννμ΄ μ΄λ ΅λ€.
   - λ³μλͺμ μλ―Έλ₯Ό λΆμ¬νκΈ° μ΄λ ΅λ€. κ°λ°νκ³Ό μ΄μν κ°μ λ³΄νΈμ μΈ μΈμ΄λ‘ μ¬μ©νκΈ° μ΄λ ΅λ€.

μ΄λ¬ν λ¬Έμ μ μ μΌκΈ μ»¬λ μμ μ¬μ©νμ¬ ν΄κ²°ν  μ μλ€. κ°κ° κ³μ’ λ¦¬μ€νΈμ μΌκΈ μ»¬λ μμ λ§λ€κ³  ν΄λΉ ν΄λμ€ μ΄λ¦ κΈ°λ°μΌλ‘ μ©μ΄ μ¬μ© λ° κ²μμ νλ©΄ λλ€.

```java
@Test
public void μΌκΈμ»¬λ μμΌλ‘_μ΄λ¦_λΆμ¬() {
    HanaBankAccounts hanaBankAccounts = new HanaBankAccounts(createHanaBankAccounts());
    KbBankAccounts kbBankAccounts = new KbBankAccounts(createKbBankAccounts());
}
```
