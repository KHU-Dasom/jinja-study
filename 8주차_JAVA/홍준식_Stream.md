# 스트림
데이터의 흐름  
JAVA 8부터 추가됨  
객체의 컬랙션을 처리하기 위하여 사용됨  
- 스트림은 자료 구조가 아니고 컬렉션, 배열, I/O 채널을 입력으로 받아 처리하기만 한다.
- 스트림은 기존 자료 구조를 변경하지 않고, 파이프라인된 함수의 결과를 제공하기만 한다.
- 모든 중개 연산(intermediate operation)은 다양한 중개 연산이 파이프라인 될 수 있으므로 lazily executed 되고 스트림을 결과로 반환한다. 최종 연산(terminal operation)이 스트림의 종료를 마킹하고 결과를 반환한다.  

## 컬렉션
자바에서 컬렉션은 데이터의 집합을 의미하며 모든 컬렉션은 최상위 조상인 Collection 인터페이스를 상속받는다.  
위 Collection 인터페이스에는 stream 메소드가 정의되어있고  stream 메소드를 통해 Stream 객체를 생성할 수 있다. [참조](https://github.com/openjdk/jdk/blob/4999f2cb164743ebf4badd3848a862609528dde3/src/java.base/share/classes/java/util/Collection.java#L742-L744)  
```Java
List<Dog> dogList = new ArrayList<>();  
dogList.add(new Dog());  
dogList.add(new Dog());  
  
Stream<Dog> dogStream = dogList.stream();  
dogStream.forEach(System.out::println);
```
## 배열
Arrays 클래스에 stream 메소드가 클래스 메소드로 정의되어있다.  
```Java
Dog[] dogArray = new Dog[]{new Dog(), new Dog(), new Dog()};  
  
Stream<Dog> dogStream = Arrays.stream(dogArray);  
dogStream.forEach(System.out::println);
```
## 가변 매개변수
Stream 클래스의 of 메소드를 통해 가변 매개변수를 전달받아 스트림을 생성할 수도 있다.
```Java
Stream<Dog> dogStream = Stream.of(new Dog(), new Dog(), new Dog());  
dogStream.forEach(System.out::println);
```
## 람다 표현식
람다 표현식을 통하여 무한 스트림을 생성할 수 있다.
```Java
Stream<Integer> stream = Stream.iterate(2, n -> n + 2);  
stream.limit(6).forEach(System.out::println);  // 2 4 6 8 10 12
```
# Intermediate Operatons (중개 연산)
생성된 스트림은 중개 연산을 통해 다른 스트림으로 변환된다.  
중개 연산은 스트림을 전달받아 스트림을 반환하므로, 연속해서 사용할 수 있다.  
또한, 스트림은 lazy executed 되어 성능을 최적화할 수 있다.  
위에서 사용한 limit 연산도 중개 연산이다.

## Filter
해당 조건에 맞는 요소들을 반환하는 연산이다.  
```Java
Stream<Integer> stream = Stream.iterate(2, n -> n + 2);  
stream.filter(n -> n % 4 != 0)
		.limit(6).forEach(System.out::println);  // 2 6 10 14 18 22
```
## Map
스트림 내 요소들을 변환하는 연산이다.  
```Java
Stream<Integer> stream = Stream.iterate(2, n -> n + 2);  
stream.map(n -> n + 1).limit(6)  
		.forEach(System.out::println);  // 3, 5, 7, 9, 11, 13
```
## Sort
스트림 내 요소를 정렬한다.
단, 무한 스트림에서 정렬은 불가능하다.  
```Java
Stream<Integer> stream = Stream.iterate(2, n -> n + 2);  
stream.limit(6).sorted(Comparator.reverseOrder())  
        .forEach(System.out::println);  // 12, 10, 9, 6, 4, 2
```
# Terminal Operation (최종 연산)
중개 연산을 통해 변환된 스트림은 최종 연산을 통해 결과를 표시한다.  
최종 연산 이전까지는 지연시켰던 모든 중개 연산이 최종 연산시에 수행된다.  
## Collect
스트림 내 요소들을 수집한다.  
```Java
Stream<Integer> stream = Stream.iterate(2, n -> n + 2);  
List<Integer> intList =  stream.limit(6).collect(Collectors.toList());  
System.out.println(intList);
```
## forEach
각 요소들을 순환하며 명시된 동작을 수행한다.  
반환값이 void라 보통 요소의 출력을 할 때에 많이 사용한다.  
```Java
Stream<Integer> stream = Stream.iterate(2, n -> n + 2);  
stream.limit(6).forEach(System.out::println);  // 2 4 6 8 10 12
```
## Reduce
스트림 내 각 요소들을 순환하며 연산을 수행한다.  
```Java
Stream<Integer> stream = Stream.iterate(2, n -> n + 2);  
Optional<Integer> result =  stream.limit(3).reduce((e1, e2) -> e2 - e1);  
System.out.println(result);  // 4 = 6 - (4 - 2)
```

# 병렬 처리
스트림은 parallelStream을 통하여 손쉬운 병렬 처리를 지원한다.
병렬 처리란 하나의 작업을 분할하여 각각의 코어가 병렬적으로 작업을 처리하는 것이다.  
```Java
List<Integer> listOfNumbers = Arrays.asList(1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12);  
listOfNumbers.parallelStream().forEach(number ->  
        System.out.println(number + " " + Thread.currentThread().getName())  
);
/*
4 ForkJoinPool.commonPool-worker-1
9 ForkJoinPool.commonPool-worker-1
10 ForkJoinPool.commonPool-worker-1
12 ForkJoinPool.commonPool-worker-1
3 ForkJoinPool.commonPool-worker-1
6 ForkJoinPool.commonPool-worker-5
7 ForkJoinPool.commonPool-worker-4
11 ForkJoinPool.commonPool-worker-2
8 main
1 ForkJoinPool.commonPool-worker-7
5 ForkJoinPool.commonPool-worker-6
2 ForkJoinPool.commonPool-worker-3
*/
```
병렬 스트림이 항상 순차 스트림보다 장점을 가지지는 않는다.  
스트림을 분할하고 스레드를 할당하며 결과를 병합하는 과정이 필요하여 오버헤드가 발생할 수 있다.  
- 요소의 수가 많고 요소당 처리시간이 긴 경우
- 코어의 수가 많은 경우: 싱글 코어에서의 병렬 처리는 의미가 없다.
- 병렬로 수행하기 어려운 스트림 모델: `iterate`의 경우 이전의 연산 결과가 이후 연산에 영향을 미쳐 분할이 어렵고 성능이 하락한다.
- stateless한 처리의 경우: 상태를 관리해야하는 stateful한 연산의 경우 병렬 처리 성능이 장점을 가지지 않는다.
# For Loop vs Stream
스트림의 간결한 구문은 가독성과 유지보수성에서 잠점을 가진다.  
병렬처리를 하지 않는 순차 스트림과 for loop에서는 for loop가 훨씬 뛰어난 성능을 보인다.  
단, 순환 비용보다 계산 비용이 더 큰 함수를 수행하는 경우에는 성능의 차이가 크지 않다.  
충분히 큰 데이터를 처리할 때에는 병렬 스트림을 사용하는 것이 의미를 가진다. [참조](https://sigridjin.medium.com/java-stream-api는-왜-for-loop보다-느릴까-50dec4b9974b)  


