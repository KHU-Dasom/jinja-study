# π Java8

2014λ 3μ 18μΌμ μΆμλ Javaλ²μ μ΄λ€. ν¬κ² λ€μκ³Ό κ°μ λ³νκ° μμλ€.

1. Functional Interfaces μ Lambdaννμ μΆκ°
2. `forEach()`λ©μλ μΆκ°.
3. Interfaceμ Default Static Method μΆκ°
4. Stream API μΆκ°
5. μλ‘μ΄ λ μ§/μκ° API μΆκ°
6. Collection API ν₯μ
7. λμμ± κ΄λ ¨ API ν₯μ
8. Java IO ν₯μ
9. μ¬λ¬ API κ°μ 
10. Optional Class μΆκ°


-----

## βοΈ 1. Functional interface, Lambda μΆκ°

### Functional interface

- Functional Interface: 1κ°μ μΆμ λ©μλλ₯Ό κ°κ³  μλ μΈν°νμ΄μ€. *Single Abstract Method*λΌκ³ λ νλ€.

μ ν¨μν μΈν°νμ΄μ€λ μΆμ λ©μλλ₯Ό 1κ°λ§ κ°μ ΈμΌν κΉ? μ΄λ μΈν°νμ΄μ€μ λ©μλκ° λ¨ νλμ κΈ°λ₯μ μ κ³΅ν΄μΌνκΈ° λλ¬Έμ΄λ€.

κ·Έλμ νΉμ¬λ κ°λ°μκ° 2κ°μ μΆμ λ©μλλ₯Ό μ μ©μν¬ μ μμΌλ―λ‘ `@FunctionalInterface`λΌλ μ΄λΈνμ΄μμ μΈν°νμ΄μ€ μμ λΆμ¬μ μ»΄νμΌλ¬μκ² κ²μ¦μ μμ²­νκ³ , μ»΄νμΌ μλ¬λ₯Ό λ°μμν¨λ€. μ¦, `@Override`μ λΉμ·νκ² κ°λ°μμ μ€μμ νμμμ μν΅μ λͺ©μ μ΄ λΉμ·ν κ²μ΄λ€.

κ·Έλ¦¬κ³  μ΄λ₯Ό μ¬μ©νλ μ΄μ λ μλ°8μ μΆκ°λ Lambdaμμ΄ ν¨μν μΈν°νμ΄μ€λ‘λ§ μ κ·Όμ΄ κ°λ₯νκΈ° λλ¬Έμ΄λ€.

μ΄λ μλ°μμ ν¨μν κ°λ° ν¨λ¬λ€μμ μ§μνκΈ° μμνλ©΄μ μΈν°νμ΄μ€μ μ΄λ€ λ‘μ§μ **κ°**μΌλ‘ μ°κΈ° μν¨μ΄λ€.

```java
public interface FunctionalInterfaceClass {
    public abstract void outputText(String text);
}


@Test
void java8FunctionalTest(){
    FunctionalInterfaceClass func = text -> {
        System.out.println(text);
    };
    //output: java8 λ²μ μ λλ€μ ν¨μν μΈν°νμ΄μ€ νμ€νΈ 
    func.outputText("java8 λ²μ μ λλ€μ ν¨μν μΈν°νμ΄μ€ νμ€νΈ ");
}

```

μλ λλ€μμ κ΅¬νμμ μ΄κ³ , μ΅λͺ ν΄λμ€λ‘ κ΅¬νλ κ°λ₯νλ€.

```java
@Test
void java8FunctionalTestAnonymousfunction(){
    FunctionalInterfaceClass func = new FunctionalInterfaceClass() {
        @Override
        public void outputText(String text) {
            System.out.println(text);
        }
    };
    func.outputText("java8 λ²μ μ λλ€μ ν¨μν μΈν°νμ΄μ€ νμ€νΈ ");

    
    FunctionalInterfaceClass func2 = new FunctionalInterfaceClass() {
        @Override
        public void outputText(String text) {
            System.out.println(text.toUpperCase());
        }
    };
    func2.outputText("java8 λ²μ μ λλ€μ ν¨μν μΈν°νμ΄μ€ νμ€νΈ ");
}


//output
//java8 λ²μ μ λλ€μ ν¨μν μΈν°νμ΄μ€ νμ€νΈ 
//JAVA8 λ²μ μ λλ€μ ν¨μν μΈν°νμ΄μ€ νμ€νΈ 
```

νμ€ν λλ€μμ μΌμλμ λΉκ΅ν΄μλ μ΅λͺ ν΄λμ€λ λ³΄κΈ° λ³΅μ‘ν¨κ³Ό κ°κ²°ν¨μ΄ λΆμ‘±νλ€.

### Lambda

λλ€λ₯Ό μ λ¦¬λ μ΄ κΈμ λͺ©μ μ΄ μλλ―λ‘ κ°λ¨ν μ€λͺνκ³  λμ΄κ°μΌνλ€.

- λλ€ ννμ: μ΅λͺ ν¨μλ‘, μ΄λ¦μ΄ μκ³  μλ³μλ§ μλ ν¨μμ΄λ€. μΌλ°μ μΌλ‘ λ€λ₯Έ ν¨μμ λ§€κ°λ³μλ‘ μ μλλ κ³³μμ μ ννκ² μ μλλ€. 

κΈ°λ³Έμ μΈ κ΅¬μ‘°λ λ€μκ³Ό κ°λ€.

```text
(parameters) -> expression
```

κ°λ¨νκ² μ΄λ¬ν μμ λ‘ λ§μ μμ κ° μλλ° μμ κ΅¬μ‘°λ₯Ό μ¬μ©νλ©΄

`(int x, int y) -> x+y`λ‘ ννν  μ μλ€. μ΄ ν¨μμ μ΄λ¦μ μκ³  μλ³μλ§ μλ€λκ²μ΄λ€.

λλ€λ₯Ό μ¬μ©νκΈ° μν΄μ ν¨μν μΈν°νμ΄μ€λ₯Ό νλ λ§λ λ€.

```java
@FunctionalInterface
public interface LambdaSumInterface {
    public abstract int sum(int x, int y);
}
```

κ·Έλ¦¬κ³  μμμ μ μν κ΅¬μ‘° κ·Έλλ‘ μ¬μ©νλ©΄ λλ€.

```java
@Test
void lambdaSumTest(){
    LambdaSumInterface result = (int x,int y) -> x+y;
    //10
    result.sum(5,5);
}
```

κ°λ¨νκ³  μμ£Ό μ°μ΄λ ν¨μν μΈν°νμ΄μ€λ€μ λ§€ λ² μ μΈνκΈ°κ° κ·μ°?μ κ²μ΄λ€ κ·Έλμ μλ°μμ κΈ°λ³Έμ μΌλ‘ μ κ³΅νλ ν¨μν μΈν°νμ΄μ€λ€μ΄ μλ€.

- Runnable: μΈμλ₯Ό λ°μ§ μκ³  λ¦¬ν΄κ°λ μλ μΈν°νμ΄μ€

```java
@Test
void runAbleTest(){
    Runnable consoleOutPut = () -> System.out.println("μΈμλ μκ³  λ¦¬ν΄λ μλ€.");
    //run()μ ν΅ν΄ νΈμΆ κ°λ₯.
    //output: μΈμλ μκ³  λ¦¬ν΄λ μλ€.
    consoleOutPut.run();
}
```

- Supplier: μΈμλ₯Ό λ°μ§ μκ³  T νμμ κ°μ²΄λ₯Ό λ¦¬ν΄νλ€.

```java
@Test
void supplierTest(){
    Supplier<String> consoleOutput = () -> "abcdefg";
    //get()μΌλ‘ λ¦¬ν΄κ°μ λ°μμ¬ μ μλ€.
    String output = consoleOutput.get();
    //2μ°¨ μμ λλ¬Έμλ‘ λ³ν
    System.out.println(output.toUpperCase());
}
```

- Consumer: Tνμμ κ°μ²΄λ₯Ό μΈμλ‘ λ°κ³  λ¦¬ν΄μ μλ€.

```java
@Test
void consumerTest(){
    Consumer<String> myBlogURL = username -> System.out.println("https://" + username + "/github.io");
    //output: https://kkminseok/github.io
    myBlogURL.accept("kkminseok");
}
```

- Function: `Function<T, R>`μ Tνμμ μΈμλ₯Ό λ°κ³ , Rνμμ κ°μ²΄λ₯Ό λ¦¬ν΄

```java
@Test
void functionTest(){
    Function<String,String> myBlogURL = username ->  "https://" + username + "/github.io";
    String url = myBlogURL.apply("kkminseok");
    //output: https://kkminseok/github.io
    System.out.println(url);
}
```

- Predicate: `Predicate<T>`μ Tνμμ μΈμλ₯Ό λ°κ³ , `boolean`μ λ¦¬ν΄

```java
@Test
void predicateTest(){
    Predicate<String> isMyBlogURL = url -> url.equals("https://kkminseok/github.io");
    boolean result = isMyBlogURL.test("https://kkminseok/github.io");
    //true
    System.out.println(result);
}
```

μ’ λ λ³΅μ‘ν κΈ°λ₯λ€λ μ‘΄μ¬νκ³  μ λ¦¬νμλ μ΄κΈμ΄ λλ¬΄ λ¬΄κ±°μμ§κΉλ΄ κΆκΈνλ©΄ μ°Ύμλ³΄μ.

μΌλ¨ μ΄μ λΆν° μ΄ **ν¨μν μΈν°νμ΄μ€**μ κΈ°λ°νμ¬ μ¬λ¬ κΈ°λ₯λ€μ΄ μκ²¨λ¬μΌλ ν λ² λ³΄μ.


## βοΈ 2. foreach() λ©μλ μΆκ°

`foreach()` λ©μλλ List, Mapκ³Ό κ°μ μ¬λ¬ μλ£κ΅¬μ‘°λ₯Ό μννλ©΄μ κ°λ°μκ° μ§μ ν μμμ μννκ² λμμ€λ€.

`foreach()`λ μμμ μ€λͺν `Consumer`μ μΈμλ‘ λκΈ°κ³  μ΄ ν¨μν μΈν°νμ΄μ€μ λ©μλλ₯Ό μ μ©νλ€. 

Java8 λ²μ  μ΄μ μλ λ°°μ΄μ `Iterator`λ₯Ό ν΅ν΄ μννμ¬ μμμ νλ€λ©΄ λ€μκ³Ό κ°μ΄ μμνλ€.

```java
//java8μ΄μ 
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

Iteratorλ₯Ό λ³μλ‘ μ μΈνλ©°, λμ λ λκΉμ§ μμμ ν΄μ€¬λ€. 

μ΄μ  `foreach()`λ₯Ό ν΅ν΄ λλ€μμΌλ‘ κ°λ¨νκ² ννμ΄ κ°λ₯ν΄μ‘λ€.

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

μ°Έκ³ λ‘ λλ€ννμμ μ¬μ©νμ§ μκ³  κ΅¬νν λ €λ©΄ λ€μκ³Ό κ°μ΄ ν  μλ μλ€.

```java
//subList.forEach(sub -> System.out.println(sub)); λμ μ
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

`foreach()`λ λ΄λΆμ μΌλ‘ μμκ°μ΄ κ΅¬νλμ΄ μλ€.

- μΈμμ λκ°μ μ²΄ν¬νκ³  forλ¬Έμ ν΅ν΄ μννλ€.
- TνμμΌλ‘ λ€μ΄μ¨ μΈμλ₯Ό `accept()`λ©μλλ₯Ό ν΅ν΄ μλΉνκ³  λ¦¬ν΄κ°μ λ°ννμ§ μλλ€.

### foreach()μ μ₯μ 

- μΌλ¨ forλ¬Έμ΄λ Iteratorλ³΄λ€ μ§§κ³  κ°κ²°νλ€.
- κ·Έλμ κ°λμ±μ΄ μ’μμ‘λ€.
- κ·Έλμ μ½λ μ€λ₯μ μνμ±μ΄ μ μ΄μ‘λ€.

### foreach()μ λ¨μ 

- μμ μ μ΄κ° νλ€λ€. μΈλ±μ€ μ κ·Όμ΄ μ΄λ ΅κΈ° λλ¬Έμ΄λ€.
- λΉμ°νκ² μ§λ§ μ»¬λ μ μμλ₯Ό μΆκ°νκ±°λ μ­μ μ κΈ°λ₯μ΄ μλ€.


## βοΈ 3. Interfaceμ Default Static Method μΆκ°

Interfaceλ μλ ν¨μλ₯Ό κ΅¬ννμ§ λͺ»νκ² λμ΄μλ€.

νμ§λ§, Java8μμλ `default` λλ `static`μ΄λΌλ ν€μλλ₯Ό λ©μλ μμ λΆμ¬μ μ¬μ©νλ©΄ μΈν°νμ΄μ€μμλ ν¨μλ₯Ό κ΅¬νν  μ μκ² νλ€.

```java
//μΈν°νμ΄μ€
public interface DefaultMethodTestInterface {

    default void printThisInterface(){
        System.out.println("DefaultMethodTestInterfaceμλλ€.");
    }

    static void printTest(){
        System.out.println("testestsetst");
    }
}

//κ΅¬νν΄λμ€
public class DefaultMethodTestImpl implements DefaultMethodTestInterface{
}

//Test
@Test
void defaultMethodInInterface(){
    DefaultMethodTestImpl interface1 = new DefaultMethodTestImpl();
    interface1.printThisInterface();
    //interfaceμμ μ§μ νΈμΆ
    DefaultMethodTestInterface.printTest();
}

```

![image](https://user-images.githubusercontent.com/30401054/210025428-e60a54ed-60db-4ea5-9a8c-77d71c2868c1.png)

### λ€μ€μμμ²λΌ κ΅¬νν΄λ³Έλ€λ©΄?

Javaλ *Diamond-Problem* λλ¬Έμ λ€μ€μμμ μ§μνμ§ μλλ€. μΈν°νμ΄μ€λ λ€μ€μμμ΄ κ°λ₯νλ€. μλνλ©΄ λͺ¨λ **Override**λΌλ κ°μ μ±μ λκΈ° λλ¬Έμ λ¬Έμ κ° λ°μνμ§ μκΈ° λλ¬Έμ΄λ€.

νμ§λ§ μμ²λΌ Default Method Interfaceλ₯Ό μ΄μ©νλ©΄ λ¬Έμ κ° λ°μν  μλ μκ² λ€.

```java
//μΈν°νμ΄μ€ 1
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

//μΈν°νμ΄μ€ 2
@FunctionalInterface
public interface Interface2 {

	void method2();
	
	default void log(String str){
		System.out.println("I2 logging::"+str);
	}

}
```

λ€μ€μμμ νλ©΄ `log()`κ° κ²ΉμΉκ² λ  κ²μ΄λ€. 

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

μ§μ  ν΄λ³΄λ©΄ μκ² μ§λ§ μ»΄νμΌλ¬κ° `log()`μ κ΅¬νμ **κ°μ **νλ€.

κ·Έλμ `Interface1`μ `log()`μ `Interface2`μ `log()`μ κ΅¬νμ΄ μλ―Έκ° μκ² λλ κ²μ΄λ€.

## βοΈ 4. Stream API μΆκ°

- Java Stream API for Bulk Data Operations on Collections

`java.util.stream`ν΄λμ€κ° μΆκ°λμλ€. μ»¬λ μμ λμμΌλ‘ νν°λ§, λ°μ΄ν°λ³ν λ±μ μμμ μ½κ² ν΄μ£Όλ μ­ν μ νλ€.

μ΄ μμμ λ³λ ¬, μμ°¨μ μΌλ‘ μνμ΄ κ°λ₯νκ³  νΉν λμ©λμ λ°μ΄ν°λ₯Ό μ μ νλλ°μ νμνλ€κ³  νλ€.

μ΄κ±΄ λλ¬΄ μμ΄ λ°©λνλ―λ‘ λ°λ‘ μ£Όμ λ₯Ό μ‘κ³  ν΄μΌν  κ² κ°λ€. κ°λ¨ν μμ λ§ μ κ² λ€.

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

λ³λ ¬μ²λ¦¬μ μμ°¨μ²λ¦¬μ μκ°μ μ¬κ³  κ²°κ³Όλ₯Ό λ΄λ νμ€νΈμΈλ°

λλ λΉμ°ν λ³λ ¬μ²λ¦¬κ° λΉ λ₯Όκ±°λΌ μκ°νλ€.

νμ§λ§

![image](https://user-images.githubusercontent.com/30401054/210026819-a0f32844-46fe-40b0-85be-b3ce466ea695.png)

κ½€λ μ μλ―Ένκ² μ°¨μ΄κ° λ¬κ³ , μ΄λ₯Ό μ°Ύμλ³΄λ μ°λ λ μμ±λΉμ©μ λ°λ₯Έ μκ°μ§μ°μ΄ μ΄λ§νΌ μ»Έλκ²μ΄λ€.
κ·Έλ¦¬κ³  λμμ± λ¬Έμ λ μκ°νμ΄μΌ νλ€.

λ°μ΄ν°κ° ν° κ²½μ°λ λ¬Όλ‘  λ³λ ¬μ²λ¦¬κ° λ λΉ¨λλ€.(μμ λ¦¬μ€νΈμ 100λ§κΉμ§ λ£μ κ²½μ°)

![image](https://user-images.githubusercontent.com/30401054/210026965-9b8d004d-0bfa-4815-a0e7-0f757d8dad47.png)

μλ¬΄νΌ Stream APIλ κ°λ¨νκ² μ§κ³  λμ΄κ°κ² λ€.

## βοΈ 5. μλ‘μ΄ λ μ§/μκ° API μΆκ°

- Java Date Time API

Java8 μ΄μ μλ `java.util.Date` ν΄λμ€λ₯Ό μ΄μ©ν΄μ μκ°κ³Ό λ μ§λ₯Ό νννλ€.

κ·Όλ° μ΄ ν΄λμ€λ μλ° μ»¬λ μ νλ μμν¬λ μκ°κ³Ό κ΄λ ¨λ μΌλΆ κΈ°λ₯μ μ κ³΅νμ§ μκ³ , μκ° μμλ₯Ό μΆλ ₯νλ €λ©΄ μλΈ ν΄λμ€λ₯Ό μμ±ν΄μ μ¬μ©ν΄μΌ νλ€. κ·Έλμ λΆνΈνλ€. μ΄ ν΄λμ€λ μλ° κ°λ° μ΄κΈ°λ μΆκ°λμ΄μ λ§μ κΈ°λ₯μ΄ λνλμ΄μμλ€.

Java8μμλ `java.time`ν¨ν€μ§λ₯Ό ν΅ν΄ λ μ§λ₯Ό μ’ λ νννκΈ° μ½κ² νμλ€.

μ΄λ λ λ°λ‘ λ€λ£¨λκ² λμμ λλ‘ λ°©λνλ―λ‘ μ΄μ λλ‘ μμκ°λκ² μ’μ κ² κ°λ€.

## βοΈ 6. Collection API ν₯μ

- Collection API improvements

λ¨Όμ , μ»¬λ μμ Java 1.2λ²μ μ μ²μ λ±μ₯νλ€.


μμμ μ»¬λ μμ λν `foreach()`λ©μλλ `Stream API`λ₯Ό μ΄ν΄λ΄€λ€.

- `Map`μλ `replaceAll()`, `compute()`, `merge()` λ±μ΄ μΆκ°λμλ€.
- `HashMap`ν΄λμ€μ μΆ©λλ¬Έμ  μκ³ λ¦¬μ¦μ κ°μ νμλ€.
- `Collection`μ κΈ°λ³Έ λ©μλ `removeif(Predicate filter)`λ₯Ό ν΅ν΄ μ‘°κ±΄μ λ§κ² μμλ₯Ό μ­μ ν  μ μκ² νμλ€.
- `Collection`μ `spliterator()`λ©μλλ₯Ό ν΅ν΄ μμ°¨μ  λλ λ³λ ¬λ‘ μμλ₯Ό μνν  μ μλ `Spliterator`μΈμ€ν΄μ€λ₯Ό λ°ννλ€.

λ±λ±μ΄ μλ€.

## βοΈ 7. λμμ± κ΄λ ¨ API ν₯μ

- Concurrency API Changes/Enhancements

λμμ± κ΄λ ¨ APIμ μ±λ₯μ΄ ν₯μ λμλ€λλ°, μ€ νλ‘κ·Έλ¨μ μ¬μ©ν κ²½νμ΄ μμ΄ μ κΈ°λ§ νκ² λ€.

- `ConcurrentHashMap`μ λ©μλ λ€μ μ±λ₯μ΄ ν₯μλμλ€κ³  νλ€.
- `newWorkStealingPool()`μ΄λΌλ λ©μλκ° ν₯μ λμλ€. 
> μμ€νμ μ μ²΄ νλ‘μΈμ€ μλ§νΌ μ€λ λλ₯Ό μμ±νκ³  κ΄λ¦¬νλ μ€λ λνμ λ§λλ μμμ μ²λ¦¬νλ λ©μλλΌκ³  νλ€. κ·Έλ¦¬κ³ , μμνμ μμμ΄ μμ κ²½μ° λ€λ₯Έ μ°λ λμ μμμ λμμ€λ€κ³  ν¨.


## βοΈ 8. Java IO ν₯μ

- Java IO Improvements

νμΌμμΆλ ₯ κΈ°λ₯μ ν₯μμν€λ λ©μλλ€μ΄ λ±μ₯νλ€.

- `Files.list(Path dir)`: lazily populated Streamμ λ°ννκ³ , λλ ν°λ¦¬ μμ κ°κ°μ ν­λͺ©λ€μ κ°μ Έμ¨λ€. lazily populated Streamμ΄λ μ€νΈλ¦Όμ΄ λ°λ‘ μ±μμ§μ§ μκ³  μμκ° νμν  λ κ·Έ κ°μ μμ±νμ¬ μ±λ₯μ κ°μ μν€λ λ°©λ²μ΄λ€.

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

- `Files.lines(Path path)`: λͺ¨λ  λΌμΈμ μ½κ³  μ€νΈλ¦ΌμΌλ‘ λ°ννλ€.

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

## βοΈ 9. μ¬λ¬ API κ°μ 

- Miscellaneous Java 8 Core API improvements

- `ThreadLocal`ν΄λμ€μ μ μ  λ©μλμΈ `withInitial`μ `supplier`λΌλ μΈμλ₯Ό ν΅ν΄ μΈμ€ν΄μ€λ₯Ό μ½κ² μμ±ν  μ μκ² λμλ€.
- `Comparator` μΈν°νμ΄μ€μ μ λ ¬, μ­μ λ ¬ κ°μ κΈ°λ³Έ λ©μλμ μ μ  λ©μλκ° μΆκ°λμλ€.
- `min()`,`max()`,`sum()` λ©μλλ€μ λνΌ ν΄λμ€λ‘ κ°μΈκ² λμλ€.
- `logicalAnd()`,`logicalOr()`,`logicalXor()` λ©μλλ `Boolean` λνΌν΄λμ€λ‘ κ°μΈκ² λμλ€.

λ±λ±, μ¬λ¬κ°μ§ μκ²Όλλ° κ³Όμ λΆκΈ


## βοΈ 10. Optional Class μΆκ°

`NullPointerException`μ λ€λ£¨κΈ° μ½κ² λ§λλ `Optional` ν΄λμ€κ° μΆκ°λμλ€.

μ΄ ν΄λμ€λ₯Ό ν΅ν΄ κ°μ²΄κ° λΉμ΄μλμ§, μ‘΄μ¬νλμ§ νμΈν  μ μμ΄μ μ¬λ¬ μλ¬λ₯Ό νΌν  μ μκ² λμλ€.

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


## κ²°λ‘ 

Java8μ μ°λ¦¬κ° μ£Όλ‘ μ¬μ©νλ `Optional`, `Stream API`, λλ€μ, ν¨μν μΈν°νμ΄μ€κ° λμλ€.

ν¨μν μΈν°νμ΄μ€λ₯Ό ν΅ν΄ νμλλ κΈ°λ₯λ€μ΄ λ§μ΄ μκ²Όλ€κ³  λ³΄λ©΄ λλ€.

