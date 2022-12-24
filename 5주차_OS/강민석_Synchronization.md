# 🔅 동시성 개요

앞에서 프로세스끼리 데이터를 공유하려면 **share-memory**기법이나 **message passing**기법을 사용하였다.

공유된 데이터를 사용하려면 항상 동시성을 생각해야하는데, 데이터 불일치라는 문제가 생길 수 있기 때문이다. 

그래서 프로세스나 쓰레드의 실행 순서를 보장해주는 등의 작업을 해줘야한다.

-----

## 🔑 입출금 예제

간단한 예제로는 입출금 예제가 있다.

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
        //virtual Thread 아직 intellij에서 지원안함. (preview)
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

결과

![image](https://user-images.githubusercontent.com/30401054/209386112-3fca98e1-83da-4fcc-8c9b-15b21f24763d.png)

![image](https://user-images.githubusercontent.com/30401054/209385471-4e2c91b9-cfa1-4c66-bd48-81f87236d666.png)

개발자의 의도와도 다르다. 예측도 거의 불가능. 디버그도 어렵다. 저 코드에서 `thread.sleep(1000)`를 지우면 개발자가 원하는대로 돌아가기는 한다. 연산이 간단하기에 추적이나 디버그하는데 시간이 크게 소요되지 않지만 프로그램이 커지면 커질수록 디버그와 추적하기는 어려워진다.

때문에 동시성을 계속 고려하면서 프로그래밍을 짜거나 논리적 흐름을 구성하는게 정말 중요하다.

```text
MOV EAX, balance       ; EAX 레지스터에 balance 값 로드
SUB EAX, money      ; EAX에서 money를 뺀 결과를 EAX에 저장
MOV balance, EAX      ; balance에 EAX의 값 대입
```
> x86아키텍처의 어셈블리어 코드

![image](https://user-images.githubusercontent.com/30401054/209426229-5d9cc6ae-418e-46d1-a5de-3b3aa3c89ed1.png)

즉, 자원을 공유하는 부분에서 스케줄러나 인터럽트를 통해 레지스터가 **saved**되거나 **restored**되면 문제가 발생할 확률이 생기는 것이다.

위는 쓰레드 2개를 나타낸것이지만, 실제 코드에서는 쓰레드 100개가 동시에 같은 자원을 할당받는다. 그렇기에 결과적으로 -98000같은 숫자가 나오는 것이다.

-----

## 🔑 Race Condition

여러 개의 쓰레드나 프로세스가 공유 자원을 할당받으려하고 할당받아서, 그 수행의 결과값이 매 번 달라 예상치 못한 결과가 나오는 경우.

-----

## 🔑 해결법 (Critical Section Problem)

어떤 코드영역이 각각의 프로세스나 쓰레드마다 같은 데이터를 갱신하거나 접근하는 영역을 Critical Section(임계영역)이라고 부른다.

이럴경우 race Condition이 발생할 수 있고, 이를 해결하기 위한 방법들이 **Critical Section Problem**이라고 한다.

이 문제를 풀기 위해서는 개념적으로 3가지 요구되는 것들이 있다.

- Mutual Exclusion(상호배제): 어떤 프로세스가 임계영역에서 작업을 수행중일 때는 다른 프로세스는 접근할 수 없다는 것을 보장.

하지만 상호배제를 적용하면 **deadlock**과 **starvation**이라는 문제가 발생한다.

- Progress(avoid **deadlock**): 쓰레드가 자원에 접근할 수 있는 상태를 일컫는 말. 임계 영역에 쓰레드가 있는 경우 Progress가 있다고 할 수 있고, 임계 영역에 쓰레드가 없는 경우 Progress가 없다고 한다. 그래서 임계영역에 있는 쓰레드는 자원에 접근이 가능하지만, 임계영역에 없는 쓰레드는 자원에 접근이 불가능하다는 것. 그래서 임계영역에 들어갈 쓰레드를 결정해줘야한다.
- Bounded waiting(avoid **starvation**): 기아를 방지하기 위해서 한 번 임계구역에 들어간 프로세스나 쓰레드는 다음번 임계구역에 들어갈 때 제한을 두어야 한다.


이 3가지를 모두 만족하면 임계구역 문제를 해결하였다고 할 수 있지만, 현실적으로 3가지를 모두 만족하기란 쉽지가 않다.

이러한 임계영역에 동시접근을 해결하기 위해서는 공유자원에 접근 하였을 때 **인터럽트**자체를 방지하여 컨택스트 스위치가 발생하지 않도록 만들 수 있지만, 멀티프로세서 환경에서는 모든 코어에 대해 인터럽트를 막으면 효율성이 떨어지므로 사용하지 않는다.

- Mutex Locks
- Semaphore
- Monitor
- Liveness

`Liveness`는 다른 방법의 단점을 극복하고 Mutual Exclusion + Progress까지 수행. 나머지는 Mutual Exclusion까지만

### 🔑 Mutex Locks

- mutex: **mut**ual **ex**clusion

임계역역에 들어갈 때 Lock이라는 것을 걸고, 나올 때 Lock을 풀어서 상호배제를 보장하는 방법. 즉, 프로세스나 쓰레드는 **Lock**라는 것을 획득하고 임계영역에 들어가야한다. 단순한 방법

개념적으로 이런식으로 구현이 가능하다.

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

결과

![image](https://user-images.githubusercontent.com/30401054/209431945-a918996c-ba0a-4cbe-82c6-ff79b068caa6.png)

- 단점

- Busy waiting: 내부적으로 다른프로세스들이 lock이 풀려있는지 while(true)를 통해 계속 질의하여 의미없이 CPU가 계속 돌아가고 있는 문제.

```java
public class MyClass {
  private boolean locked = false;

  public void doSomething() {
    while (locked) {
      // busy waiting loop 다른 프로세스, 쓰레드는 계속 여기 걸려있음.
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

- Spinlock: 위의 Busy waiting이 생기면 while문 끝으로 갔다가 다시 올라가고 하는 형태가 마치 spin의 형태라서 지어진 이름. Busy waiting이 멀티 프로세스환경에서 마냥 나쁜게 아니다. 왜냐하면 context switch가 일어나지 않기에 그 시간을 절약할 수 있다는 것 즉, ready-Queue에서 대기했다가 가는것이 아니고 프로세스가 계속 Running상태에서 공유자원이 해제되면 들어갈 수 있기 때문이다.

### 🔑 Semaphores

![image](https://user-images.githubusercontent.com/30401054/209433925-7763adf7-f126-4837-b846-590126cd37eb.png)
> 알고리즘 개념도

![image](https://user-images.githubusercontent.com/30401054/209433897-d76b57a8-c9ea-4573-8a40-ae5a537a2065.png)
> 전체 구성도

mutex-lock 경우는 임계구역에 하나의 프로세스나 쓰레드만 접근이 가능했지만, 세마포어는 접근가능한 쓰레드나 프로세스의 개수를 정해놓으면 접근할 때마다 `count`변수를 빼고 임계구역을 벗어날때마다 `count`변수를 증가시켜 자원에 접근가능한 쓰레드 수를 제한할 수 있다. 그러다가 `count`변수가 0이 되면 모든 리소스가 할당된 것이므로 다른 쓰레드는 접근이 불가능하게 막는다.

count를 빼는 행위를 **wait()**한다고 하고, count를 더하는 행위를 **signal()**이라고 한다.(세마포어에서 사용하는 용어)

- Binary Semaphore

자원에 접근 가능한 쓰레드 수를 0 ~ 1로 제한하는 것. mutex-lock처럼 동작한다.

- Counting Semaphore

자원에 접근 가능한 쓰레드 수를 2이상으로 제한하는 것이다.


![image](https://user-images.githubusercontent.com/30401054/209434363-cd5ce6e3-735a-407d-b7f1-1a9526a60d24.png)

S1을 수행하는 프로세스를 P1, S2를 수행하는 프로세스를 P2라 하자. 그리고 count변수를 0으로 초기화되어있다고 한다.

만약 쓰레드간 순서를 제어하고 싶다면 위처럼 구현하면 된다. 

P1이 자원을 반납하면 count가 1증가하여 사용할 수 있게 되는데, 이로인해서 P2는 S1이 끝날때까지 접근이 불가능한 것이다.

세마포어도 <u>busy waiting</u>의 문제가 있는데, 멀티프로세서 환경에서는 <u>spinlock</u>으로 사용해도 되지만, 그렇지 않은 경우 이를 해결해야한다.

그래서 세마포어를 통해 자원에 접근을 할 수 없다면 자기 자체를 중지하고 <u>waiting queue</u>로 보내고 다른 프로세스가 **signal()**을 호출하면 <u>waiting quque</u>에 있던 프로세스를 <u>ready queue</u>로 보내는 전략이 있다.

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

출금하는 부분은 쓰레드 1개만 들어와야하므로 위의 세마포어를 이용해 동기화문제를 해결할 수 있다.

![image](https://user-images.githubusercontent.com/30401054/209435461-999859a8-7fc3-4994-a7e2-045ca2a7457b.png)

위의 결과에서 WithDraw ~ 출력부분을 보면 순차적으로 잘 빼고 있음을 볼 수 있다. 

그리고 접근 가능한 쓰레드 수를 1이 아닌 2로 지정하면 동기화문제가 해결되지 않는것을 볼 수 있다.

![image](https://user-images.githubusercontent.com/30401054/209435770-ed20518a-ee80-454f-9d1d-ea987f2c0f4e.png)

결국 적절한 세마포어 수와, 내부적 알고리즘을 얼마나 Atomic하게 짰냐에 따라 다르다.

### 🔑 Monitors

세마포어는 잘 쓰면 효율적이고 편리하게 동기화 문제를 해결할 수 있지만 가장 적절한 접근 가능한 쓰레드 수를 지정하는 일은 매우 어렵다.

그리고 세마포어는 **timing error**라는 문제가 발생할 수 있는데, 적절한 곳에 `wait()`나 `signal()`을 안 써주는 경우 코드를 짜다보니 `wait()`을 생략하거나 두 번 호출된 경우 등 예측할 수 없는 문제가 발생할 수 있는 문제다. 즉, 해당 함수를 호출하는 순서가 중요해진다는 것이다.

여기서 상대적으로 간단한 동기화 도구인 <u>monitor</u>가 등장했다.

#### monitor type

monitor type은 상호배제를 제공하는 일종의 데이터 타입이라고 생각하면 된다. 

이 데이터 타입 안에 있는 구문들은 모두 동기화를 보장할 수 있도록 하는 것이다. Java에서는 `synchronized`키워드가 있다.

```java
 public synchronized void withDraw(int money) throws InterruptedException {
        if(balance >= money){
            Thread thread = Thread.currentThread();
            this.balance -= money;
            System.out.printf(
                    "WithDraw Function Thread %s - Task %d finished.%n", thread.getName(), getBalance());

        }
```

`synchronized` 키워드만 추가해주면 해당 블럭들은 전부 임계구역이 된다.

![image](https://user-images.githubusercontent.com/30401054/209437330-8858180f-b0bd-4321-9c02-4f9c416e699b.png)

하나가 끝날 때까지 다른 스레드들은 접근을 못한다.


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

이런식으로 block을 지정해서 동기화문제를 해결할 수도 있다.

mutexlock과의 차이점은 권한 제어 여부에 있다.

monitors는 스레드가 순차적으로 실행되기 위함의 목적이 있고, mutexlock은 스레드가 자원에 접근할 수 있는 권한을 제어하기 위함에 목적이 있다.


## 🔑정리

동기화를 해결하기 위해 3가지 기법이 있다.

- mutex lock: 임계구역에 락이라는걸 걸어서 상호배제를 보장하는방법
- Semaphores: 임계구역에 접근이 가능한 쓰레드 수를 제한해서 상호배제를 보장하는 방법
- Monitors: 임계구역에 접근할 수 있는 쓰레드의 순서를 제어하는 방법. 






