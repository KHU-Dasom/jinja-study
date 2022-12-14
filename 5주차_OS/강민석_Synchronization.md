# π λμμ± κ°μ

μμμ νλ‘μΈμ€λΌλ¦¬ λ°μ΄ν°λ₯Ό κ³΅μ νλ €λ©΄ **share-memory**κΈ°λ²μ΄λ **message passing**κΈ°λ²μ μ¬μ©νμλ€.

κ³΅μ λ λ°μ΄ν°λ₯Ό μ¬μ©νλ €λ©΄ ν­μ λμμ±μ μκ°ν΄μΌνλλ°, λ°μ΄ν° λΆμΌμΉλΌλ λ¬Έμ κ° μκΈΈ μ μκΈ° λλ¬Έμ΄λ€. 

κ·Έλμ νλ‘μΈμ€λ μ°λ λμ μ€ν μμλ₯Ό λ³΄μ₯ν΄μ£Όλ λ±μ μμμ ν΄μ€μΌνλ€.

-----

## π μμΆκΈ μμ 

κ°λ¨ν μμ λ‘λ μμΆκΈ μμ κ° μλ€.

```java
import org.junit.jupiter.api.Test;

import java.util.ArrayList;
import java.util.List;
import java.util.concurrent.*;

public class TestClass {
    private Account account = new Account(100000);
    @Test
    void accountExample() throws InterruptedException, ExecutionException {
        ExecutorService executor = Executors.newFixedThreadPool(100);
        //virtual Thread μμ§ intellijμμ μ§μμν¨. (preview)
        //ExecutorService executor = Executors.newVirtualThreadPerTaskExecutor();

        List<Task> tasks = new ArrayList<>();
        for (int i = 0; i < 100; i++) {
            tasks.add(new Task((int)((Math.random()*3) + 1) * 1000, account));
        }

        long time = System.currentTimeMillis();

        List<Future<Integer>> futures = executor.invokeAll(tasks);

        long sum = 0;
        for (Future<Integer> future : futures) {
            sum += future.get();
        }

        time = System.currentTimeMillis() - time;

        System.out.println("sum = " + sum + "; time = " + time + " ms");

        executor.shutdown();
    }
}

class Account{
    private int balance;

    public Account(int balance){
        this.balance = balance;
    }

    public void withDraw(int money) throws InterruptedException {
        if(balance >= money){
            Thread thread = Thread.currentThread();
            thread.sleep(1000);
            this.balance -= money;
            System.out.printf(
                    "WithDraw Function Thread %s - Task %d finished.%n", thread.getName(), getBalance());
        }
    }
    public int getBalance(){
        return this.balance;
    }
}

class Task implements Callable<Integer> {

    private final int money;
    private final Account account;

    public Task(int money, Account account){
        this.money = money;
        this.account = account;
    }

    @Override
    public Integer call() throws Exception {
        System.out.printf(
                "Thread %s - Task %d waiting...%n", Thread.currentThread().getName(), money);

        try {
            account.withDraw(money);
        } catch (InterruptedException e) {
            System.out.printf(
                    "Thread %s - Task %d canceled.%n", Thread.currentThread().getName(), money);
            return -1;
        }

        System.out.printf(
                "Thread %s - Task %d finished.%n", Thread.currentThread().getName(), account.getBalance());
        return ThreadLocalRandom.current().nextInt(100);
    }
}
```

κ²°κ³Ό

![image](https://user-images.githubusercontent.com/30401054/209386112-3fca98e1-83da-4fcc-8c9b-15b21f24763d.png)

![image](https://user-images.githubusercontent.com/30401054/209385471-4e2c91b9-cfa1-4c66-bd48-81f87236d666.png)

κ°λ°μμ μλμλ λ€λ₯΄λ€. μμΈ‘λ κ±°μ λΆκ°λ₯. λλ²κ·Έλ μ΄λ ΅λ€. μ  μ½λμμ `thread.sleep(1000)`λ₯Ό μ§μ°λ©΄ κ°λ°μκ° μνλλλ‘ λμκ°κΈ°λ νλ€. μ°μ°μ΄ κ°λ¨νκΈ°μ μΆμ μ΄λ λλ²κ·Ένλλ° μκ°μ΄ ν¬κ² μμλμ§ μμ§λ§ νλ‘κ·Έλ¨μ΄ μ»€μ§λ©΄ μ»€μ§μλ‘ λλ²κ·Έμ μΆμ νκΈ°λ μ΄λ €μμ§λ€.

λλ¬Έμ λμμ±μ κ³μ κ³ λ €νλ©΄μ νλ‘κ·Έλλ°μ μ§κ±°λ λΌλ¦¬μ  νλ¦μ κ΅¬μ±νλκ² μ λ§ μ€μνλ€.

```text
MOV EAX, balance       ; EAX λ μ§μ€ν°μ balance κ° λ‘λ
SUB EAX, money      ; EAXμμ moneyλ₯Ό λΊ κ²°κ³Όλ₯Ό EAXμ μ μ₯
MOV balance, EAX      ; balanceμ EAXμ κ° λμ
```
> x86μν€νμ²μ μ΄μλΈλ¦¬μ΄ μ½λ

![image](https://user-images.githubusercontent.com/30401054/209426229-5d9cc6ae-418e-46d1-a5de-3b3aa3c89ed1.png)

μ¦, μμμ κ³΅μ νλ λΆλΆμμ μ€μΌμ€λ¬λ μΈν°λ½νΈλ₯Ό ν΅ν΄ λ μ§μ€ν°κ° **saved**λκ±°λ **restored**λλ©΄ λ¬Έμ κ° λ°μν  νλ₯ μ΄ μκΈ°λ κ²μ΄λ€.

μλ μ°λ λ 2κ°λ₯Ό λνλΈκ²μ΄μ§λ§, μ€μ  μ½λμμλ μ°λ λ 100κ°κ° λμμ κ°μ μμμ ν λΉλ°λλ€. κ·Έλ κΈ°μ κ²°κ³Όμ μΌλ‘ -98000κ°μ μ«μκ° λμ€λ κ²μ΄λ€.

-----

## π Race Condition

μ¬λ¬ κ°μ μ°λ λλ νλ‘μΈμ€κ° κ³΅μ  μμμ ν λΉλ°μΌλ €νκ³  ν λΉλ°μμ, κ·Έ μνμ κ²°κ³Όκ°μ΄ λ§€ λ² λ¬λΌ μμμΉ λͺ»ν κ²°κ³Όκ° λμ€λ κ²½μ°.

-----

## π ν΄κ²°λ² (Critical Section Problem)

μ΄λ€ μ½λμμ­μ΄ κ°κ°μ νλ‘μΈμ€λ μ°λ λλ§λ€ κ°μ λ°μ΄ν°λ₯Ό κ°±μ νκ±°λ μ κ·Όνλ μμ­μ Critical Section(μκ³μμ­)μ΄λΌκ³  λΆλ₯Έλ€.

μ΄λ΄κ²½μ° race Conditionμ΄ λ°μν  μ μκ³ , μ΄λ₯Ό ν΄κ²°νκΈ° μν λ°©λ²λ€μ΄ **Critical Section Problem**μ΄λΌκ³  νλ€.

μ΄ λ¬Έμ λ₯Ό νκΈ° μν΄μλ κ°λμ μΌλ‘ 3κ°μ§ μκ΅¬λλ κ²λ€μ΄ μλ€.

- Mutual Exclusion(μνΈλ°°μ ): μ΄λ€ νλ‘μΈμ€κ° μκ³μμ­μμ μμμ μνμ€μΌ λλ λ€λ₯Έ νλ‘μΈμ€λ μ κ·Όν  μ μλ€λ κ²μ λ³΄μ₯.

νμ§λ§ μνΈλ°°μ λ₯Ό μ μ©νλ©΄ **deadlock**κ³Ό **starvation**μ΄λΌλ λ¬Έμ κ° λ°μνλ€.

- Progress(avoid **deadlock**): μ°λ λκ° μμμ μ κ·Όν  μ μλ μνλ₯Ό μΌμ»«λ λ§. μκ³ μμ­μ μ°λ λκ° μλ κ²½μ° Progressκ° μλ€κ³  ν  μ μκ³ , μκ³ μμ­μ μ°λ λκ° μλ κ²½μ° Progressκ° μλ€κ³  νλ€. κ·Έλμ μκ³μμ­μ μλ μ°λ λλ μμμ μ κ·Όμ΄ κ°λ₯νμ§λ§, μκ³μμ­μ μλ μ°λ λλ μμμ μ κ·Όμ΄ λΆκ°λ₯νλ€λ κ². κ·Έλμ μκ³μμ­μ λ€μ΄κ° μ°λ λλ₯Ό κ²°μ ν΄μ€μΌνλ€.
- Bounded waiting(avoid **starvation**): κΈ°μλ₯Ό λ°©μ§νκΈ° μν΄μ ν λ² μκ³κ΅¬μ­μ λ€μ΄κ° νλ‘μΈμ€λ μ°λ λλ λ€μλ² μκ³κ΅¬μ­μ λ€μ΄κ° λ μ νμ λμ΄μΌ νλ€.


μ΄ 3κ°μ§λ₯Ό λͺ¨λ λ§μ‘±νλ©΄ μκ³κ΅¬μ­ λ¬Έμ λ₯Ό ν΄κ²°νμλ€κ³  ν  μ μμ§λ§, νμ€μ μΌλ‘ 3κ°μ§λ₯Ό λͺ¨λ λ§μ‘±νκΈ°λ μ½μ§κ° μλ€.

μ΄λ¬ν μκ³μμ­μ λμμ κ·Όμ ν΄κ²°νκΈ° μν΄μλ κ³΅μ μμμ μ κ·Ό νμμ λ **μΈν°λ½νΈ**μμ²΄λ₯Ό λ°©μ§νμ¬ μ»¨νμ€νΈ μ€μμΉκ° λ°μνμ§ μλλ‘ λ§λ€ μ μμ§λ§, λ©ν°νλ‘μΈμ νκ²½μμλ λͺ¨λ  μ½μ΄μ λν΄ μΈν°λ½νΈλ₯Ό λ§μΌλ©΄ ν¨μ¨μ±μ΄ λ¨μ΄μ§λ―λ‘ μ¬μ©νμ§ μλλ€.

- Mutex Locks
- Semaphore
- Monitor
- Liveness

`Liveness`λ λ€λ₯Έ λ°©λ²μ λ¨μ μ κ·Ήλ³΅νκ³  Mutual Exclusion + ProgressκΉμ§ μν. λλ¨Έμ§λ Mutual ExclusionκΉμ§λ§

### π Mutex Locks

- mutex: **mut**ual **ex**clusion

μκ³μ­μ­μ λ€μ΄κ° λ Lockμ΄λΌλ κ²μ κ±Έκ³ , λμ¬ λ Lockμ νμ΄μ μνΈλ°°μ λ₯Ό λ³΄μ₯νλ λ°©λ². μ¦, νλ‘μΈμ€λ μ°λ λλ **Lock**λΌλ κ²μ νλνκ³  μκ³μμ­μ λ€μ΄κ°μΌνλ€. λ¨μν λ°©λ²

κ°λμ μΌλ‘ μ΄λ°μμΌλ‘ κ΅¬νμ΄ κ°λ₯νλ€.

```java
//locking = acquire
lock.lock();
try {
    // critical section of code goes here
} finally {
    //unlocking = release
    lock.unlock();
}
```

```java
try {
    lock.lock();
    account.withDraw(money);
} catch (InterruptedException e) {
    System.out.printf(
            "Thread %s - Task %d canceled.%n", Thread.currentThread().getName(), money);
    return -1;
}finally {
    lock.unlock();
}
```

κ²°κ³Ό

![image](https://user-images.githubusercontent.com/30401054/209431945-a918996c-ba0a-4cbe-82c6-ff79b068caa6.png)

- λ¨μ 

- Busy waiting: λ΄λΆμ μΌλ‘ λ€λ₯Ένλ‘μΈμ€λ€μ΄ lockμ΄ νλ €μλμ§ while(true)λ₯Ό ν΅ν΄ κ³μ μ§μνμ¬ μλ―Έμμ΄ CPUκ° κ³μ λμκ°κ³  μλ λ¬Έμ .

```java
public class MyClass {
  private boolean locked = false;

  public void doSomething() {
    while (locked) {
      // busy waiting loop λ€λ₯Έ νλ‘μΈμ€, μ°λ λλ κ³μ μ¬κΈ° κ±Έλ €μμ.
    }
    locked = true;
    try {
      // critical section of code goes here
    } finally {
      locked = false;
    }
  }
}

```

- Spinlock: μμ Busy waitingμ΄ μκΈ°λ©΄ whileλ¬Έ λμΌλ‘ κ°λ€κ° λ€μ μ¬λΌκ°κ³  νλ ννκ° λ§μΉ spinμ ννλΌμ μ§μ΄μ§ μ΄λ¦. Busy waitingμ΄ λ©ν° νλ‘μΈμ€νκ²½μμ λ§λ₯ λμκ² μλλ€. μλνλ©΄ context switchκ° μΌμ΄λμ§ μκΈ°μ κ·Έ μκ°μ μ μ½ν  μ μλ€λ κ² μ¦, ready-Queueμμ λκΈ°νλ€κ° κ°λκ²μ΄ μλκ³  νλ‘μΈμ€κ° κ³μ Runningμνμμ κ³΅μ μμμ΄ ν΄μ λλ©΄ λ€μ΄κ° μ μκΈ° λλ¬Έμ΄λ€.

### π Semaphores

![image](https://user-images.githubusercontent.com/30401054/209433925-7763adf7-f126-4837-b846-590126cd37eb.png)
> μκ³ λ¦¬μ¦ κ°λλ

![image](https://user-images.githubusercontent.com/30401054/209433897-d76b57a8-c9ea-4573-8a40-ae5a537a2065.png)
> μ μ²΄ κ΅¬μ±λ

mutex-lock κ²½μ°λ μκ³κ΅¬μ­μ νλμ νλ‘μΈμ€λ μ°λ λλ§ μ κ·Όμ΄ κ°λ₯νμ§λ§, μΈλ§ν¬μ΄λ μ κ·Όκ°λ₯ν μ°λ λλ νλ‘μΈμ€μ κ°μλ₯Ό μ ν΄λμΌλ©΄ μ κ·Όν  λλ§λ€ `count`λ³μλ₯Ό λΉΌκ³  μκ³κ΅¬μ­μ λ²μ΄λ λλ§λ€ `count`λ³μλ₯Ό μ¦κ°μμΌ μμμ μ κ·Όκ°λ₯ν μ°λ λ μλ₯Ό μ νν  μ μλ€. κ·Έλ¬λ€κ° `count`λ³μκ° 0μ΄ λλ©΄ λͺ¨λ  λ¦¬μμ€κ° ν λΉλ κ²μ΄λ―λ‘ λ€λ₯Έ μ°λ λλ μ κ·Όμ΄ λΆκ°λ₯νκ² λ§λλ€.

countλ₯Ό λΉΌλ νμλ₯Ό **wait()**νλ€κ³  νκ³ , countλ₯Ό λνλ νμλ₯Ό **signal()**μ΄λΌκ³  νλ€.(μΈλ§ν¬μ΄μμ μ¬μ©νλ μ©μ΄)

- Binary Semaphore

μμμ μ κ·Ό κ°λ₯ν μ°λ λ μλ₯Ό 0 ~ 1λ‘ μ ννλ κ². mutex-lockμ²λΌ λμνλ€.

- Counting Semaphore

μμμ μ κ·Ό κ°λ₯ν μ°λ λ μλ₯Ό 2μ΄μμΌλ‘ μ ννλ κ²μ΄λ€.


![image](https://user-images.githubusercontent.com/30401054/209434363-cd5ce6e3-735a-407d-b7f1-1a9526a60d24.png)

S1μ μννλ νλ‘μΈμ€λ₯Ό P1, S2λ₯Ό μννλ νλ‘μΈμ€λ₯Ό P2λΌ νμ. κ·Έλ¦¬κ³  countλ³μλ₯Ό 0μΌλ‘ μ΄κΈ°νλμ΄μλ€κ³  νλ€.

λ§μ½ μ°λ λκ° μμλ₯Ό μ μ΄νκ³  μΆλ€λ©΄ μμ²λΌ κ΅¬ννλ©΄ λλ€. 

P1μ΄ μμμ λ°λ©νλ©΄ countκ° 1μ¦κ°νμ¬ μ¬μ©ν  μ μκ² λλλ°, μ΄λ‘μΈν΄μ P2λ S1μ΄ λλ λκΉμ§ μ κ·Όμ΄ λΆκ°λ₯ν κ²μ΄λ€.

μΈλ§ν¬μ΄λ <u>busy waiting</u>μ λ¬Έμ κ° μλλ°, λ©ν°νλ‘μΈμ νκ²½μμλ <u>spinlock</u>μΌλ‘ μ¬μ©ν΄λ λμ§λ§, κ·Έλ μ§ μμ κ²½μ° μ΄λ₯Ό ν΄κ²°ν΄μΌνλ€.

κ·Έλμ μΈλ§ν¬μ΄λ₯Ό ν΅ν΄ μμμ μ κ·Όμ ν  μ μλ€λ©΄ μκΈ° μμ²΄λ₯Ό μ€μ§νκ³  <u>waiting queue</u>λ‘ λ³΄λ΄κ³  λ€λ₯Έ νλ‘μΈμ€κ° **signal()**μ νΈμΆνλ©΄ <u>waiting quque</u>μ μλ νλ‘μΈμ€λ₯Ό <u>ready queue</u>λ‘ λ³΄λ΄λ μ λ΅μ΄ μλ€.

```java
class Account{

    private Semaphore semaphoreForWithdraw;
    private int balance;

    public Account(int balance){
        this.balance = balance;
        semaphoreForWithdraw = new Semaphore(1);
    }

    public void withDraw(int money) throws InterruptedException {
        if(balance >= money){
            Thread thread = Thread.currentThread();
            semaphoreForWithdraw.acquire();
            this.balance -= money;
            System.out.printf(
                    "WithDraw Function Thread %s - Task %d finished.%n", thread.getName(), getBalance());
            semaphoreForWithdraw.release();
        }
    }
    public int getBalance(){
        return this.balance;
    }
}
```

μΆκΈνλ λΆλΆμ μ°λ λ 1κ°λ§ λ€μ΄μμΌνλ―λ‘ μμ μΈλ§ν¬μ΄λ₯Ό μ΄μ©ν΄ λκΈ°νλ¬Έμ λ₯Ό ν΄κ²°ν  μ μλ€.

![image](https://user-images.githubusercontent.com/30401054/209435461-999859a8-7fc3-4994-a7e2-045ca2a7457b.png)

μμ κ²°κ³Όμμ WithDraw ~ μΆλ ₯λΆλΆμ λ³΄λ©΄ μμ°¨μ μΌλ‘ μ λΉΌκ³  μμμ λ³Ό μ μλ€. 

κ·Έλ¦¬κ³  μ κ·Ό κ°λ₯ν μ°λ λ μλ₯Ό 1μ΄ μλ 2λ‘ μ§μ νλ©΄ λκΈ°νλ¬Έμ κ° ν΄κ²°λμ§ μλκ²μ λ³Ό μ μλ€.

![image](https://user-images.githubusercontent.com/30401054/209435770-ed20518a-ee80-454f-9d1d-ea987f2c0f4e.png)

κ²°κ΅­ μ μ ν μΈλ§ν¬μ΄ μμ, λ΄λΆμ  μκ³ λ¦¬μ¦μ μΌλ§λ Atomicνκ² μ§°λμ λ°λΌ λ€λ₯΄λ€.

### π Monitors

μΈλ§ν¬μ΄λ μ μ°λ©΄ ν¨μ¨μ μ΄κ³  νΈλ¦¬νκ² λκΈ°ν λ¬Έμ λ₯Ό ν΄κ²°ν  μ μμ§λ§ κ°μ₯ μ μ ν μ κ·Ό κ°λ₯ν μ°λ λ μλ₯Ό μ§μ νλ μΌμ λ§€μ° μ΄λ ΅λ€.

κ·Έλ¦¬κ³  μΈλ§ν¬μ΄λ **timing error**λΌλ λ¬Έμ κ° λ°μν  μ μλλ°, μ μ ν κ³³μ `wait()`λ `signal()`μ μ μ¨μ£Όλ κ²½μ° μ½λλ₯Ό μ§λ€λ³΄λ `wait()`μ μλ΅νκ±°λ λ λ² νΈμΆλ κ²½μ° λ± μμΈ‘ν  μ μλ λ¬Έμ κ° λ°μν  μ μλ λ¬Έμ λ€. μ¦, ν΄λΉ ν¨μλ₯Ό νΈμΆνλ μμκ° μ€μν΄μ§λ€λ κ²μ΄λ€.

μ¬κΈ°μ μλμ μΌλ‘ κ°λ¨ν λκΈ°ν λκ΅¬μΈ <u>monitor</u>κ° λ±μ₯νλ€.

#### monitor type

monitor typeμ μνΈλ°°μ λ₯Ό μ κ³΅νλ μΌμ’μ λ°μ΄ν° νμμ΄λΌκ³  μκ°νλ©΄ λλ€. 

μ΄ λ°μ΄ν° νμ μμ μλ κ΅¬λ¬Έλ€μ λͺ¨λ λκΈ°νλ₯Ό λ³΄μ₯ν  μ μλλ‘ νλ κ²μ΄λ€. Javaμμλ `synchronized`ν€μλκ° μλ€.

```java
 public synchronized void withDraw(int money) throws InterruptedException {
        if(balance >= money){
            Thread thread = Thread.currentThread();
            this.balance -= money;
            System.out.printf(
                    "WithDraw Function Thread %s - Task %d finished.%n", thread.getName(), getBalance());

        }
```

`synchronized` ν€μλλ§ μΆκ°ν΄μ£Όλ©΄ ν΄λΉ λΈλ­λ€μ μ λΆ μκ³κ΅¬μ­μ΄ λλ€.

![image](https://user-images.githubusercontent.com/30401054/209437330-8858180f-b0bd-4321-9c02-4f9c416e699b.png)

νλκ° λλ  λκΉμ§ λ€λ₯Έ μ€λ λλ€μ μ κ·Όμ λͺ»νλ€.


```java
if(balance >= money){
    Thread thread = Thread.currentThread();
    synchronized (Account.class){
        this.balance -= money;
    }
    System.out.printf(
            "WithDraw Function Thread %s - Task %d finished.%n", thread.getName(), getBalance());

}
```

μ΄λ°μμΌλ‘ blockμ μ§μ ν΄μ λκΈ°νλ¬Έμ λ₯Ό ν΄κ²°ν  μλ μλ€.

mutexlockκ³Όμ μ°¨μ΄μ μ κΆν μ μ΄ μ¬λΆμ μλ€.

monitorsλ μ€λ λκ° μμ°¨μ μΌλ‘ μ€νλκΈ° μν¨μ λͺ©μ μ΄ μκ³ , mutexlockμ μ€λ λκ° μμμ μ κ·Όν  μ μλ κΆνμ μ μ΄νκΈ° μν¨μ λͺ©μ μ΄ μλ€.


## πμ λ¦¬

λκΈ°νλ₯Ό ν΄κ²°νκΈ° μν΄ 3κ°μ§ κΈ°λ²μ΄ μλ€.

- mutex lock: μκ³κ΅¬μ­μ λ½μ΄λΌλκ±Έ κ±Έμ΄μ μνΈλ°°μ λ₯Ό λ³΄μ₯νλλ°©λ²
- Semaphores: μκ³κ΅¬μ­μ μ κ·Όμ΄ κ°λ₯ν μ°λ λ μλ₯Ό μ νν΄μ μνΈλ°°μ λ₯Ό λ³΄μ₯νλ λ°©λ²
- Monitors: μκ³κ΅¬μ­μ μ κ·Όν  μ μλ μ°λ λμ μμλ₯Ό μ μ΄νλ λ°©λ². 






