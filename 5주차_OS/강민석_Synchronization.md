# 동시성 개요

앞에서 프로세스끼리 데이터를 공유하려면 **share-memory**기법이나 **message passing**기법을 사용하였다.

공유된 데이터를 사용하려면 항상 동시성을 생각해야하는데, 데이터 불일치라는 문제가 생길 수 있기 때문이다. 

그래서 프로세스나 쓰레드의 실행 순서를 보장해주는 등의 작업을 해줘야한다.

-----

## 입출금 예제

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

