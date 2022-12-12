# **Thread**

## **📖 Thread란?**

- 프로세스 내에서 실행되는 흐름의 단위 또는 CPU Utilization의 최소 단위
- 쓰레드는 다음과 같은 특성을 가짐
  - 쓰레드는 각각의 쓰레드 ID, 프로그램 카운터(PC), 레지스터, 스택 영역을 가짐.
  - 하나의 프로세스 내에서 동작하는 여러 쓰레드들은 같은 코드, 데이터 세그먼트, 힙 영역을 공유함.
  - 쓰레드 생성 및 제거에 따른 비용이 적음(inexpensive).

![threads](https://user-images.githubusercontent.com/39883609/205608902-1b34fd5b-1e26-4148-8630-3c70216d7742.png)

- 하나의 프로세스가 하나의 쓰레드를 가지면 **단일 쓰레드(Single Thread) 프로세스**라고 함.
- 멀티프로세싱 어플리케이션을 **멀티 쓰레드 프로세스**로 구현하게 되면 프로세스 생성 비용을 줄일 수 있음.
- 쓰레드를 lightweight process라고 부르기도 함.

![thread_in_process](https://user-images.githubusercontent.com/39883609/205609246-ab408eeb-4982-4e4e-8820-9f0b14d8461c.png)

- 쓰레드는 각각의 프로그램 카운터를 가지며 프로세스 주소 공간 내 스택 영역에 각자의 스택 영역을 가지면서 존재함.

## **💻 Thread의 장점**

![single_vs_multi_thread](https://user-images.githubusercontent.com/39883609/205610603-77f30e67-897b-4676-895e-86178b9b74e2.png)

### **자원 공유 (Resource Sharing)**

- IPC(Inter-Process Communication) 과정에서 프로세스 간 통신을 위해선 shared memory 또는 message passing을 사용해야 함.
- 쓰레드는 기본적으로 쓰레드가 속한 프로세스 내에서 코드 및 데이터 세그먼트와 힙 영역을 공유하기 때문에 쓰레드 간 자원 공유가 용이함.

### **경제성 (Economy)**

- 프로세스를 생성하고 종료하는 과정을 위한 메모리 복사 및 리소스 할당 비용이 매우 큼.
- 쓰레드는 프로세스 내에서 자원을 공유하기 때문에 프로세스 생성보다 더 경제적임.
- 쓰레드 간 context-switching 시 공유 영역을 가지고 있기 때문에 캐시 메모리를 비우지 않고 context-switching이 가능함.

### **반응성(Responsiveness)**

- 프로그램(프로세스)이 실행 되는 중에 block 되는 경우 쓰레드를 사용하면 해당 업무를 수행하는 쓰레드만 block되며 다른 쓰레드들은 계속해서 실행될 수 있음.
- block되는 극단적인 상황이 아닌 I/O 작업과 같이 작업 시간이 길어지는 경우 작업이 끝나기를 기다리며 다른 쓰레드를 실행시킬 수 있으므로 프로그램 실행을 계속 유지할 수 있음.

### **TCB(Thread Control Block)**

- 프로세스 내에 존재하는 쓰레드들의 정보를 저장하기 위한 공간. PCB에 쓰레드에 대한 정보를 저장할 수는 없음.
- 각각의 쓰레드 별로 TCB를 가지고 있으며, PC, 레지스터에 대한 정보를 가짐.
- 프로세스 내에 쓰레드를 가지고 있으므로 PCB 또한 TCB들에 대한 정보를 가지고 있음.
- 최신 OS들은 ready queue에 TCB를 저장하며 context switching이 발생할 경우 TCB 단위로 수행함.

![tcb](https://lh4.googleusercontent.com/Ov67zFw-Y5UXrxaz8ZqdPTrzxe0bSpQGXS-Nbz90qXRikQJV3P6dwClAaL1CZO3sxk65X3h22t82phtCo2GdjIe98KG98AZ75e1Ana6hJmAqRYbNIZfjffOidi0jxBOkjWsRQa07)

## **⏱️ Multicore Programming**

### **Concurrency vs Parallelism**

- Concurrency : 여러 작업이 동시에 실행되는 것처럼 보이는 것. Sudo Parallelism이라고도 부름.
- 싱글 코어 또는 싱글 프로세서 환경에서는 실제로 동시에 실행될 수 없음. 단지 짧은 주기를 가지고 context-switching이 반복되므로 동시에 실행되는 것처럼 보이는 것. 이를 concurrency라고 부름.

![concurrency](https://user-images.githubusercontent.com/39883609/205623407-d9df49fd-500c-43ac-9375-f5670ebaa1e1.png)

- Parallelism : 여러 작업이 동시에 실행되는 것. Concurrency와는 다르게 실제로 동시에 여러 스레드 혹은 프로세스를 실행하는 것.
- 멀티 코어 또는 멀티 프로세스 환경에서는 실제로 동시에 실행될 수 있음. 이를 parallelism이라고 부름.

![parallelism](https://user-images.githubusercontent.com/39883609/205623834-85481642-edcc-489f-9bf0-797158536d60.png)

### **Multicore Programming에서 고려해야 할 부분**

- 멀티코어 환경이 보편화되면서 시스템 디자이너 또는 어플리케이션 프로그래머는 멀티코어 환경에서의 프로그래밍을 고려해야 함.
- 예를 들어 OS 개발자의 경우, 멀티코어 환경에서 프로그램을 어떻게 parallel한 실행시킬지 스케쥴링 알고리즘은 무엇을 사용할지 고민해야 함.

**작업 식별(Identifying tasks)**

- 멀티코어 환경에서 실행되는 어플리케이션은 동시에 수행되는 단일 작업으로 나뉘어져야 함.
- 이상적으로, 각각의 다른 코어에서 parallel하게 수행되기 위해서는 하나의 단일 작업은 다른 단일 작업과 독립적으로 수행되어야 함.

**균형(Balance)**

- 병렬적으로 수행되는 작업들로 나누는 것도 중요하지만, 프로그래머는 그 작업들이 전체 작업에 대한 균등한 기여도를 가지도록 나누어야 함.
- 예를 들어, 특정 작업이 전체 작업들에 비해 낮은 기여도를 가진다면 해당 작업을 수행하는 코어가 낭비되는 것임.

**데이터 분할(Data splitting)**

- 어플리케이션이 독립된 작업들로 나누어지는 것 처럼, 작업들이 접근하고 조작하는 데이터 또한 각각의 코어에서 사용할 수 있도록 나누어져야 한다.

**데이터 종속성(Data dependency)**

- 작업들이 접근하는 데이터가 둘 이상의 작업 사이에서 종속성을 가지지 않는지 확인해야 함.
- 종속성을 가지는 경우 올바른 순서로 접근할 수 있도록 작업들을 동기화해야 함.

**테스트 및 디버깅(Testing and debugging)**

- 멀티코어에서 병렬적으로 실행되는 어플리케이션은 다양한 실행 경로를 가지기 때문에 테스트와 디버깅이 어려워짐.

### **병렬성(Parallelism)의 종류**

- 일반적으로 두 종류의 병렬성이 존재함.

![data_task_parallelism](https://user-images.githubusercontent.com/39883609/206079812-a98602fe-dbc2-460d-8520-5ed639f4e6b0.png)

**Data parallelism**

- 멀티코어 컴퓨팅 환경에서 전체 데이터를 서브 데이터들로 분산하고 각각의 코어가 동일한 연산을 하는 것에 초점을 맞춤.
- 예를 들어, 크기가 N인 배열이 있고 단일 쓰레드가 해당 배열의 요소들을 모두 더하는 작업을 하는 경우
  - 멀티코어 환경에서 데이터 병렬성은 배열을 N개의 작은 배열로 분할하고 각각의 코어가 각각의 작은 배열의 요소들을 더하는 것을 병렬적으로 수행함.
- Java8의 병렬 스트림이 데이터 병렬성의 예임.

**Task parallelism**

- 데이터 병렬성과는 다르게 데이터를 나누는 것이 아닌 작업 단위를 나누는 데에 초점을 맞춤.
- 위에서 제시한 예시를 가져와 설명하면, 작업 병렬성은 배열을 나누어 처리하는 것이 아닌 배열의 인덱스를 N개로 나뉘어 해당 범위의 요소들을 더하는 각각 다른 작업을 수행함.
- 웹 서버에서 요청을 처리하는 경우가 작업 병렬성의 예임.

![data_task_parallelism](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Ft1.daumcdn.net%2Fcfile%2Ftistory%2F24740D4158C539841C)

- 일반적으로 데이터 병렬성과 작업 병렬성이 단독으로 사용되지는 않고 함께 사용되는 경우가 많음.

## **🔥 Multithreading Models**

![user_kernel_threads](https://user-images.githubusercontent.com/39883609/206084794-3154e592-1e3d-4a18-a1a4-e86ba9984a40.png)

- 쓰레드는 유저 레벨에서 제공하는 유저 쓰레드와 커널 레벨에서 제공하는 커널 쓰레드로 나뉨.
- 유저 쓰레드와 커널 쓰레드를 매핑하는 세 가지 멀티쓰레딩 모델이 있음.
  - Many-to-One
  - One-to-One
  - Many-to-Many 

### **User Thread**

- 커널 영역의 상위인 유저 영역에서 제공되는 쓰레드로 일반적으로 쓰레드 생성, 제거, 스케쥴링 등의 관리 기능을 제공하는 **라이브러리**를 통해 구현됨.
- 동일한 메모리 영역(하나의 프로세스)에서 스레드가 생성되고 관리되며 커널 호출이 없기 때문에 속도가 빠름.
- 하지만, 하나의 프로세스에서 동시에 실행되기 때문에 하나의 쓰레드가 I/O blocking 등으로 중단되면 나머지 모든 쓰레드도 중단되는 단점이 있음.
- 이는 커널 영역에서 프로세스 내의 유저 쓰레드들을 인식하지 못하기 때문임.

### **Kernel Thread**

- 커널 영역에서 제공되는 쓰레드로 커널 자체가 쓰레드를 생성하고 관리함.
- 커널 쓰레드의 경우 유저 쓰레드와는 달리 커널 영역에서 독립적으로 실행되기 때문에 하나의 쓰레드가 I/O blocking 등으로 중단되더라도 다른 쓰레드에 영향을 주지 않음.
- 하지만, 쓰레드를 생성하고 스케쥴링 및 동기화 과정에서 System call 호출이 빈번한데 이는 속도 저하의 원인이 됨.
- 또한 유저 모드와 커널 모드 전환이 빈번함.

### **Many-to-One Model**

![Many-to-One_Model](https://user-images.githubusercontent.com/39883609/207010271-bfa7da24-46c6-465b-b6f4-96f72bf7f12f.png)

- 다수의 유저 레벨 쓰레드를 하나의 커널 레벨 쓰레드에 매핑하는 멀티쓰레딩 모델.
- 유저 공간의 스레드 라이브러리에 의해 쓰레드가 관리되므로 속도가 빠르다.
- 하지만, 한 쓰레드가 I/O blocking 등으로 중단되면 나머지 모든 쓰레드도 중단되는 단점이 있음.
- 한 번에 하나의 유저 쓰레드만이 커널 쓰레드에 접근할 수 있으므로 멀티코어 시스템에서 병렬로 실행될 수 없음.

### **One-to-One Model**

![One-to-One_Model](https://user-images.githubusercontent.com/39883609/207011035-95e0632a-e863-4741-a066-8f5144022211.png)

- 하나의 유저 레벨 쓰레드를 하나의 커널 레벨 쓰레드에 매핑하는 멀티쓰레딩 모델.
- 하나의 유저 레벨 쓰레드가 I/O blocking 등으로 중단되어도 다른 쓰레드가 영향을 받지 않음.
- 다수의 커널 쓰레드를 가지므로 유저 레벨 쓰레드가 멀티코어 시스템에서 병렬로 실행될 수 있음.
- 하지만, 하나의 사용자 쓰레드를 생성할 때 그에 매핑되는 커널 쓰레드도 생성해줘야 하기 때문에 속도가 느림.

### **Many-to-Many Model**

![Many-to-Many_Model](https://user-images.githubusercontent.com/39883609/207011637-4e6dcaf4-dae9-4474-8ed2-b2ca16b33cfe.png)

![Two-level_Model](https://user-images.githubusercontent.com/39883609/207012197-dc4b6402-13b8-496c-86d7-10755e207f3d.png)

- 다수의 유저 레벨 쓰레드를 다수의 커널 레벨 쓰에드에 매핑하는 멀티쓰레딩 모델.
- Many-to-One 모델과 One-to-One 모델의 단점을 해결하기 위해 나온 모델.
- 커널 쓰레드를 생성하는 시스템 부담이 적으며 멀티코어 환경에서 병렬적으로 실행 가능함.
- 다만 구현이 복잡하고, 현재 대부분의 시스템에서 코어 수가 증가하는 추세라 커널 쓰레드의 수를 제한하는 것의 중요성이 약해짐.
- 현재 대부분 One-to-One 모델을 이용함.

## **📄 Java Thread**

- Java에서는 기본적으로 유저 레벨 쓰레드를 제공하며 One-to-One 모델을 사용해 쓰레드를 관리하는 API를 제공함.

| Java Version | Thread API |
| --- | --- |
| < Java5 | `Runnable`, `Thread` |
| Java5 | `Callable`, `Future`, `Executor`, `ExecutorService`, `ThreadPoolExecutor` |
| Java7 | `Fork/Join`, `RecursiveTask` |
| Java8 | `CompletableFuture` |
| Java9 | `Reactive` |
| Java19 | `Virtual Thread` |

### **Runnable, Thread**

- 초기 Java 버전부터 제공하던 멀티쓰레딩 API.
- `Runnable` 인터페이스를 구현하거나 `Thread` 클래스를 상속받아 쓰레드를 생성할 수 있음.

```java
@Test
void thread() {
  Thread thread = new MyThread();

  thread.start();
}

static class MyThread extends Thread {
  @Override
  public void run() {
    System.out.println("Thread Start");
  }
}
```

- 위 코드는 `Thread` 클래스를 상속받아 쓰레드를 생성하는 방법이며, 아래 코드는 `Runnable` 인터페이스를 사용하여 쓰레드를 생성하는 방법임.

```java
@Test
void runnable() {
  Runnable runnable = new Runnable() {
    @Override
    public void run() {
      System.out.println("Thread Start");
    }
  };

  Thread thread = new Thread(runnable);
  thread.start();
}
```

### **Callable**

- `Runnable` 인터페이스는 결과를 반환할 수 없으며 반환값을 얻으려면 shared memory나 파이프를 사용해야 함.
- `Callable` 인터페이스는 `Runnable` 인터페이스와 동일하게 쓰레드를 생성하고 실행할 수 있으며, `call` 메서드를 통해 결과를 반환할 수 있음.

```java
@Test
void callable() throws ExecutionException, InterruptedException {
  Callable<String> callable = new Callable<String>() {
    @Override
    public String call() throws Exception {
      return "Thread Start";
    }
  };

  FutureTask<String> futureTask = new FutureTask<>(callable);
  Thread thread = new Thread(futureTask);
  thread.start();

  System.out.println(futureTask.get());
}
``` 

### **Future**

- `Callable` 인터페이스의 작업은 가용 쓰레드가 없거나 작업 시간이 오래 걸릴 수 있기 때문에 실행 결과를 바로 받지 못함.
- 이를 위해 `Future` 인터페이스를 사용하여 작업이 완료될 때까지 기다리거나, 작업이 완료되면 결과를 받을 수 있음.

### **ExecutorService**

- 명시적으로 쓰레드를 생성하는 `Thread`와 `Runnable`과는 달리 쓰레드 풀링을 통해 암시적으로 쓰레드를 관리하는 API.
- 작업들은 `ExecutorService`에 `submit` 메서드를 통해 전달되며 큐에 들어가고, `ExecutorService`는 작업을 처리할 쓰레드를 생성하고 큐에서 작업을 가져와 처리함.

![Executor_Service](https://i.imgur.com/zv9sPMk.jpg)

```java
@Test
void executorService() {
  ExecutorService executorService = Executors.newFixedThreadPool(2);

  executorService.submit(() -> {
    System.out.println("Thread Start");
  });

  executorService.shutdown();
}
```

### **Fork/Join**

![Fork_Join](https://user-images.githubusercontent.com/39883609/207020070-3f893a95-faf9-4c38-b157-c9a88489a01b.png)

- `ExecutorService` 기반으로 만들어진 API로 작은 크기로 쪼갤 수 있는 작업들을 효율적으로 처리하기 위해 만들어짐.
- 작업들을 `RecursiveTask`를 상속받아 구현하고, `ForkJoinPool`에 `invoke` 메서드를 통해 전달함.

```java
@Test
void forkJoin() {
  ForkJoinPool forkJoinPool = new ForkJoinPool(2);

  RecursiveTask<Integer> recursiveTask = new RecursiveTask<Integer>() {
    @Override
    protected Integer compute() {
      // Workload
    }
  };

  Integer result = forkJoinPool.invoke(recursiveTask);
  System.out.println(result);
}
```

![Java_fork_join](https://user-images.githubusercontent.com/39883609/207021246-47780155-c46a-420e-b523-30a4e41428fc.png)

### **CompletableFuture**

- `Fork/Join` 기반으로 만들어진 API로 `Future`를 확장한 API.
- `Future`는 작업이 완료될 때까지 기다렸다가 결과를 받아야 하지만, `CompletableFuture`는 작업이 완료되면 결과를 받을 수 있고, 작업이 완료되지 않았다면 다른 작업을 수행할 수 있음.
- `Future`와는 다르게 비동기 작업의 결과값을 조합하거나 에러 처리가 가능함.

```java
@Test
void completableFuture() throws ExecutionException, InterruptedException {
  CompletableFuture<String> completableFuture = new CompletableFuture<>();

  completableFuture.complete("Thread Start");

  System.out.println(completableFuture.get());
}
```

### **Reactive**

- 코드의 흐름에 중점을 두는 것이 아닌 데이터의 흐름에 중점을 두는 프로그래밍 방식.
- 비동기 데이터 스트림을 처리하고 에러 처리와 `Backpressure`를 지원하는 API.

> backpressure -> pub-sub 패턴에서 이벤트 스트림의 sub이 pub의 이벤트를 처리하는 속도보다 느릴 때, pub이 sub에게 이벤트를 보내는 속도를 조절하는 것.

- 프로그래머가 직접 쓰레드를 관리하지 않고 쓰레드 관리는 라이브러리와 프레임워크에 위임함.

![reactive_stream](https://user-images.githubusercontent.com/14136066/99145111-295c9e00-26af-11eb-912b-65ad45a58225.png)

- `Java Reactor`에서 제공하는 `Flux`와 `Mono`를 사용하여 비동기 데이터 스트림을 처리함.

```java
@Test
void reactive() {
  Flux<String> flux = Flux.just("A", "B", "C");

  flux.subscribe(
    success -> System.out.println(success),
    error -> System.out.println(error.getMessage()),
    () -> System.out.println("Complete")
  );
}
```

- Reactive Programming을 도입하게 되면 프로그램의 흐름이 모두 리액티브하게 바뀌어야 한다. 만약 프로그램 일부에 blocking API를 사용하게 되면 리액티브 프로그래밍이 무용지물이 될 수 있다.

### **Virtual Thread**

- `Java19`에서 프리뷰 형태로 제공하는 API.
- 가상 쓰레드는 JVM : OS가 1 : 1로 매핑되는 전통적인 쓰레드와는 다르게 기존의 JVM 쓰레드에 비해 훨신 가볍고 저렴함.
- `Golang`의 `goroutine`과 유사한 개념임.

![Virtual_Thread](https://files.itworld.co.kr/ITW_202211_01/java_virtual_threads_02.jpg)

```java
@Test
void virtualThread() {
  try (var executor = Executors.newVirtualThreadPerTaskExecutor()) {
    executor.submit(() -> say("Start"));
  }
  say("Thread");
}

static void say(String message) throws RunTimeException {
  try {
      Thread.sleep(Duration.ofMillis(100));
      System.out.println(message);
    } catch (InterruptedException e) {
    throw new RuntimeException(e);
  }
}
```