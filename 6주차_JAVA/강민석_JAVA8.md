# 🔅 Java8

2014년 3월 18일에 출시된 Java버전이다. 크게 다음과 같은 변화가 있었다.

1. Functional Interfaces 와 Lambda표현식 추가
2. `forEach()`메소드 추가.
3. Interface에 Default Static Method 추가
4. Stream API 추가
5. 새로운 날짜/시간 API 추가
6. Collection API 향상
7. 동시성 관련 API 향상
8. Java IO 향상
9. 여러 API 개선
10. Optional Class 추가


-----

## ✏️ 1. Functional interface, Lambda 추가

### Functional interface

- Functional Interface: 1개의 추상 메소드를 갖고 있는 인터페이스. *Single Abstract Method*라고도 한다.

왜 함수형 인터페이스는 추상 메서드를 1개만 가져야할까? 이는 인터페이스의 메소드가 단 하나의 기능을 제공해야하기 때문이다.

그래서 혹여나 개발자가 2개의 추상 메서드를 적용시킬 수 있으므로 `@FunctionalInterface`라는 어노테이션을 인터페이스 위에 붙여서 컴파일러에게 검증을 요청하고, 컴파일 에러를 발생시킨다. 즉, `@Override`와 비슷하게 개발자의 실수와 협업시의 소통의 목적이 비슷한 것이다.

그리고 이를 사용하는 이유는 자바8에 추가된 Lambda식이 함수형 인터페이스로만 접근이 가능하기 때문이다.

이는 자바에서 함수형 개발 패러다임을 지원하기 시작하면서 인터페이스의 어떤 로직을 **값**으로 쓰기 위함이다.

```java
public interface FunctionalInterfaceClass {
    public abstract void outputText(String text);
}


@Test
void java8FunctionalTest(){
    FunctionalInterfaceClass func = text -> {
        System.out.println(text);
    };
    //output: java8 버전의 람다와 함수형 인터페이스 테스트 
    func.outputText("java8 버전의 람다와 함수형 인터페이스 테스트 ");
}

```

위는 람다식의 구현예제이고, 익명 클래스로 구현도 가능하다.

```java
@Test
void java8FunctionalTestAnonymousfunction(){
    FunctionalInterfaceClass func = new FunctionalInterfaceClass() {
        @Override
        public void outputText(String text) {
            System.out.println(text);
        }
    };
    func.outputText("java8 버전의 람다와 함수형 인터페이스 테스트 ");

    
    FunctionalInterfaceClass func2 = new FunctionalInterfaceClass() {
        @Override
        public void outputText(String text) {
            System.out.println(text.toUpperCase());
        }
    };
    func2.outputText("java8 버전의 람다와 함수형 인터페이스 테스트 ");
}


//output
//java8 버전의 람다와 함수형 인터페이스 테스트 
//JAVA8 버전의 람다와 함수형 인터페이스 테스트 
```

확실히 람다식을 썼을때와 비교해서는 익명 클래스는 보기 복잡함과 간결함이 부족하다.

### Lambda

람다를 정리는 이 글의 목적이 아니므로 간단히 설명하고 넘어가야한다.

- 람다 표현식: 익명 함수로, 이름이 없고 식별자만 있는 함수이다. 일반적으로 다른 함수의 매개변수로 정의되는 곳에서 정확하게 정의된다. 

기본적인 구조는 다음과 같다.

```text
(parameters) -> expression
```

간단하게 이러한 예제로 덧셈 예제가 있는데 위의 구조를 사용하면

`(int x, int y) -> x+y`로 표현할 수 있다. 이 함수의 이름은 없고 식별자만 있다는것이다.

람다를 사용하기 위해서 함수형 인터페이스를 하나 만든다.

```java
@FunctionalInterface
public interface LambdaSumInterface {
    public abstract int sum(int x, int y);
}
```

그리고 위에서 정의한 구조 그대로 사용하면 된다.

```java
@Test
void lambdaSumTest(){
    LambdaSumInterface result = (int x,int y) -> x+y;
    //10
    result.sum(5,5);
}
```

간단하고 자주 쓰이는 함수형 인터페이스들은 매 번 선언하기가 귀찮을 것이다 그래서 자바에서 기본적으로 제공하는 함수형 인터페이스들이 있다.

- Runnable: 인자를 받지 않고 리턴값도 없는 인터페이스

```java
@Test
void runAbleTest(){
    Runnable consoleOutPut = () -> System.out.println("인자도 없고 리턴도 없다.");
    //run()을 통해 호출 가능.
    //output: 인자도 없고 리턴도 없다.
    consoleOutPut.run();
}
```

- Supplier: 인자를 받지 않고 T 타입의 객체를 리턴한다.

```java
@Test
void supplierTest(){
    Supplier<String> consoleOutput = () -> "abcdefg";
    //get()으로 리턴값을 받아올 수 있다.
    String output = consoleOutput.get();
    //2차 작업 대문자로 변환
    System.out.println(output.toUpperCase());
}
```

- Consumer: T타입의 객체를 인자로 받고 리턴은 없다.

```java
@Test
void consumerTest(){
    Consumer<String> myBlogURL = username -> System.out.println("https://" + username + "/github.io");
    //output: https://kkminseok/github.io
    myBlogURL.accept("kkminseok");
}
```

- Function: `Function<T, R>`은 T타입의 인자를 받고, R타입의 객체를 리턴

```java
@Test
void functionTest(){
    Function<String,String> myBlogURL = username ->  "https://" + username + "/github.io";
    String url = myBlogURL.apply("kkminseok");
    //output: https://kkminseok/github.io
    System.out.println(url);
}
```

- Predicate: `Predicate<T>`은 T타입의 인자를 받고, `boolean`을 리턴

```java
@Test
void predicateTest(){
    Predicate<String> isMyBlogURL = url -> url.equals("https://kkminseok/github.io");
    boolean result = isMyBlogURL.test("https://kkminseok/github.io");
    //true
    System.out.println(result);
}
```

좀 더 복잡한 기능들도 존재하고 정리하자니 이글이 너무 무거워질까봐 궁금하면 찾아보자.

일단 이제부터 이 **함수형 인터페이스**에 기반하여 여러 기능들이 생겨났으니 한 번 보자.


## ✏️ 2. foreach() 메소드 추가

`foreach()` 메소드는 List, Map과 같은 여러 자료구조를 순회하면서 개발자가 지정한 작업을 수행하게 도와준다.

`foreach()`는 위에서 설명한 `Consumer`을 인자로 넘기고 이 함수형 인터페이스의 메소드를 적용한다. 

Java8 버전 이전에는 배열을 `Iterator`를 통해 순회하여 작업을 한다면 다음과 같이 작업했다.

```java
//java8이전
@Test
void foreachTestBeforeJava8(){
    List<String> subList = new ArrayList<String>();
    subList.add("Carrot");
    subList.add("Potato");
    subList.add("Cauliflower");
    subList.add("LadyFinger");
    subList.add("Tomato");
    Iterator<String> it = subList.iterator();
    while(it.hasNext()){
        System.out.println(it.next());
    }
}
```

Iterator를 변수로 선언하며, 끝을 돌 때까지 작업을 해줬다. 

이제 `foreach()`를 통해 람다식으로 간단하게 표현이 가능해졌다.

```java
@Test
void foreachTest(){
    List<String> subList = new ArrayList<String>();
    subList.add("Carrot");
    subList.add("Potato");
    subList.add("Cauliflower");
    subList.add("LadyFinger");
    subList.add("Tomato");
    subList.forEach(sub -> System.out.println(sub));
}
```

참고로 람다표현식을 사용하지 않고 구현할려면 다음과 같이 할 수도 있다.

```java
//subList.forEach(sub -> System.out.println(sub)); 대신에
subList.forEach(new Consumer<String>() {
    @Override
    public void accept(String s) {
        System.out.println(s);
    }
});
```

```java
default void forEach(Consumer<? super T> action) {
    Objects.requireNonNull(action);
    for (T t : this) {
        action.accept(t);
    }
}
```

`foreach()`는 내부적으로 위와같이 구현되어 있다.

- 인자의 널값을 체크하고 for문을 통해 순회한다.
- T타입으로 들어온 인자를 `accept()`메서드를 통해 소비하고 리턴값은 반환하지 않는다.

### foreach()의 장점

- 일단 for문이나 Iterator보다 짧고 간결하다.
- 그래서 가독성이 좋아졌다.
- 그래서 코드 오류의 위험성이 적어졌다.

### foreach()의 단점

- 순서 제어가 힘들다. 인덱스 접근이 어렵기 떄문이다.
- 당연하겠지만 컬렉션 요소를 추가하거나 삭제의 기능이 없다.


## ✏️ 3. Interface에 Default Static Method 추가

Interface는 원래 함수를 구현하지 못하게 되어있다.

하지만, Java8에서는 `default` 또는 `static`이라는 키워드를 메소드 앞에 붙여서 사용하면 인터페이스에서도 함수를 구현할 수 있게 한다.

```java
//인터페이스
public interface DefaultMethodTestInterface {

    default void printThisInterface(){
        System.out.println("DefaultMethodTestInterface입니다.");
    }

    static void printTest(){
        System.out.println("testestsetst");
    }
}

//구현클래스
public class DefaultMethodTestImpl implements DefaultMethodTestInterface{
}

//Test
@Test
void defaultMethodInInterface(){
    DefaultMethodTestImpl interface1 = new DefaultMethodTestImpl();
    interface1.printThisInterface();
    //interface에서 직접호출
    DefaultMethodTestInterface.printTest();
}

```

![image](https://user-images.githubusercontent.com/30401054/210025428-e60a54ed-60db-4ea5-9a8c-77d71c2868c1.png)

### 다중상속처럼 구현해본다면?

Java는 *Diamond-Problem* 때문에 다중상속을 지원하지 않는다. 인터페이스는 다중상속이 가능하다. 왜냐하면 모두 **Override**라는 강제성을 띄기 때문에 문제가 발생하지 않기 때문이다.

하지만 위처럼 Default Method Interface를 이용하면 문제가 발생할 수도 있겠다.

```java
//인터페이스 1
@FunctionalInterface
public interface Interface1 {
    void method1(String str);

    default void log(String str){
        System.out.println("I1 logging::"+str);
    }

    static void print(String str){
        System.out.println("Printing "+str);
    }
}

//인터페이스 2
@FunctionalInterface
public interface Interface2 {

	void method2();
	
	default void log(String str){
		System.out.println("I2 logging::"+str);
	}

}
```

다중상속을 하면 `log()`가 겹치게 될 것이다. 

```java
public class MultiImplementClass implements Interface1,Interface2{
    @Override
    public void method1(String str) {

    }

    @Override
    public void method2() {

    }

    @Override
    public void log(String str) {
        System.out.println("MultiImplementClass logging::"+str);
        Interface1.print("abc");
    }
}

```

직접 해보면 알겠지만 컴파일러가 `log()`의 구현을 **강제**한다.

그래서 `Interface1`의 `log()`와 `Interface2`의 `log()`의 구현이 의미가 없게 되는 것이다.

## ✏️ 4. Stream API 추가

- Java Stream API for Bulk Data Operations on Collections

`java.util.stream`클래스가 추가되었다. 컬렉션을 대상으로 필터링, 데이터변환 등의 작업을 쉽게 해주는 역할을 한다.

이 작업은 병렬, 순차적으로 수행이 가능하고 특히 대용량의 데이터를 정제하는데에 탁월하다고 한다.

이건 너무 양이 방대하므로 따로 주제를 잡고 해야할 것 같다. 간단한 예제만 적겠다.

```java
@Test
void streamTest(){
    List<Integer> myList = new ArrayList<>();
    for(int i=0; i<10000; i++) myList.add(i);

    Stream<Integer> sequentialStream = myList.stream().sequential();
    Stream<Integer> parallelStream = myList.stream().parallel();

    long startTime = System.currentTimeMillis();
    Stream<Integer> integerStream = parallelStream.filter(p -> p > 5000);
    integerStream.forEach(i -> System.out.println(i));
    long endTime = System.currentTimeMillis();
    long duration1 = endTime - startTime;


    startTime = System.currentTimeMillis();
    Stream<Integer> integerStream1 = sequentialStream.filter(p -> p > 5000);
    integerStream1.forEach(i -> System.out.println(i));
    endTime = System.currentTimeMillis();
    long duration2 = endTime - startTime;
    System.out.println("parallel test took " + duration1 + " milliseconds");
    System.out.println("sequential test took " + duration2 + " milliseconds");
}
```

병렬처리와 순차처리의 시간을 재고 결과를 내는 테스트인데

나는 당연히 병렬처리가 빠를거라 생각했다.

하지만

![image](https://user-images.githubusercontent.com/30401054/210026819-a0f32844-46fe-40b0-85be-b3ce466ea695.png)

꽤나 유의미하게 차이가 났고, 이를 찾아보니 쓰레드 생성비용에 따른 시간지연이 이만큼 컸던것이다.
그리고 동시성 문제도 생각했어야 했다.

데이터가 큰 경우는 물론 병렬처리가 더 빨랐다.(위의 리스트에 100만까지 넣은 경우)

![image](https://user-images.githubusercontent.com/30401054/210026965-9b8d004d-0bfa-4815-a0e7-0f757d8dad47.png)

아무튼 Stream API는 간단하게 짚고 넘어가겠다.

## ✏️ 5. 새로운 날짜/시간 API 추가

- Java Date Time API

Java8 이전에는 `java.util.Date` 클래스를 이용해서 시간과 날짜를 표현했다.

근데 이 클래스는 자바 컬렉션 프레임워크나 시간과 관련된 일부 기능을 제공하지 않고, 시간 요소를 출력하려면 서브 클래스를 작성해서 사용해야 했다. 그래서 불편했다. 이 클래스는 자바 개발 초기때 추가되어서 많은 기능이 낙후되어있었다.

Java8에서는 `java.time`패키지를 통해 날짜를 좀 더 표현하기 쉽게 하였다.

이는 또 따로 다루는게 나을정도로 방대하므로 이정도로 알아가는게 좋을 것 같다.

## ✏️ 6. Collection API 향상

- Collection API improvements

먼저, 컬렉션은 Java 1.2버전에 처음 등장했다.


위에서 컬렉션에 대한 `foreach()`메서드랑 `Stream API`를 살펴봤다.

- `Map`에는 `replaceAll()`, `compute()`, `merge()` 등이 추가되었다.
- `HashMap`클래스의 충돌문제 알고리즘을 개선하였다.
- `Collection`의 기본 메서드 `removeif(Predicate filter)`를 통해 조건에 맞게 요소를 삭제할 수 있게 하였다.
- `Collection`의 `spliterator()`메서드를 통해 순차적 또는 병렬로 요소를 순회할 수 있는 `Spliterator`인스턴스를 반환한다.

등등이 있다.

## ✏️ 7. 동시성 관련 API 향상

- Concurrency API Changes/Enhancements

동시성 관련 API의 성능이 향상 되었다는데, 실 프로그램에 사용한 경험이 없어 적기만 하겠다.

- `ConcurrentHashMap`의 메소드 들의 성능이 향상되었다고 한다.
- `newWorkStealingPool()`이라는 메서드가 향상 되었다. 
> 시스템의 전체 프로세스 수만큼 스레드를 생성하고 관리하는 스레드풀을 만드는 작업을 처리하는 메소드라고 한다. 그리고, 작업큐에 작업이 없을 경우 다른 쓰레드의 작업을 도와준다고 함.


## ✏️ 8. Java IO 향상

- Java IO Improvements

파일입출력 기능을 향상시키는 메서드들이 등장했다.

- `Files.list(Path dir)`: lazily populated Stream을 반환하고, 디렉터리 안의 각각의 항목들을 가져온다. lazily populated Stream이란 스트림이 바로 채워지지 않고 요소가 필요할 때 그 값을 생성하여 성능을 개선시키는 방법이다.

```java
@Test
void fileListTest(){
    Path dir = Paths.get("/");
    try (Stream<Path> stream = Files.list(dir)) {
        stream.forEach(System.out::println);
    } catch (IOException e) {
        e.printStackTrace();
    }
}
```

![image](https://user-images.githubusercontent.com/30401054/210041708-3ef0e0e2-2227-46a2-86ca-1708a20196ca.png)

- `Files.lines(Path path)`: 모든 라인을 읽고 스트림으로 반환한다.

```java
//test
//1
//2
//3
//4
//5
//6
@Test
void fileLinesTest() throws IOException {
    Path file = Paths.get("test.txt");
    Stream<String> lines = Files.lines(file);
    lines.forEach(System.out::println);
}
```

![image](https://user-images.githubusercontent.com/30401054/210043088-2fcea563-0afe-4e9a-86ea-0ccebc0a6348.png)

## ✏️ 9. 여러 API 개선

- Miscellaneous Java 8 Core API improvements

- `ThreadLocal`클래스의 정적 메소드인 `withInitial`은 `supplier`라는 인자를 통해 인스턴스를 쉽게 생성할 수 있게 되었다.
- `Comparator` 인터페이스의 정렬, 역정렬 같은 기본 메서드와 정적 메소드가 추가되었다.
- `min()`,`max()`,`sum()` 메서드들은 래퍼 클래스로 감싸게 되었다.
- `logicalAnd()`,`logicalOr()`,`logicalXor()` 메서드는 `Boolean` 래퍼클래스로 감싸게 되었다.

등등, 여러가지 생겼는데 과유불급


## ✏️ 10. Optional Class 추가

`NullPointerException`을 다루기 쉽게 만드는 `Optional` 클래스가 추가되었다.

이 클래스를 통해 객체가 비어있는지, 존재하는지 확인할 수 있어서 여러 에러를 피할 수 있게 되었다.

```java
@Test
void OptionalTest(){
    // Creating an Optional object from a non-null value
    Optional<String> optional1 = Optional.of("value");

    // Creating an Optional object from a null value
    Optional<String> optional2 = Optional.ofNullable(null);

    // Creating an empty Optional object
    Optional<String> optional3 = Optional.empty();

    // Check if an Optional object has a value
    if (optional1.isPresent()) {
        System.out.println("optional1 has a value: " + optional1.get());
    }

    // Get the value of an Optional object, or a default value if it is empty
    String value1 = optional1.orElse("default value");
    String value2 = optional2.orElse("default value");
    System.out.println("value1:" + value1);
    System.out.println("value2:" + value2);

    // Use the value of an Optional object if it is present, or throw an exception if it is empty
    try {
        String value3 = optional1.orElseThrow(IllegalStateException::new);
    } catch (IllegalStateException e) {
        System.out.println("optional1 is empty");
    }

    try {
        String value4 = optional2.orElseThrow(IllegalStateException::new);
    } catch (IllegalStateException e) {
        System.out.println("optional2 is empty");
    }

    // Use the value of an Optional object if it is present, or execute a function if it is empty
    String value5 = optional1.orElseGet(() -> "default value from function");
    String value6 = optional2.orElseGet(() -> "default value from function");

    System.out.println("value5: " + value5);
    System.out.println("value6: " + value6);
}
```

![image](https://user-images.githubusercontent.com/30401054/210044802-72937df2-32d0-4c97-822e-765dcd198620.png)


## 결론

Java8은 우리가 주로 사용하는 `Optional`, `Stream API`, 람다식, 함수형 인터페이스가 나왔다.

함수형 인터페이스를 통해 파생되는 기능들이 많이 생겼다고 보면 된다.

