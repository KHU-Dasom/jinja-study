# 가비지 컬렉션
만약 운영체제로부터 메모리를 할당받고 해제하지 않는다면 프로세스가 계속 커지다가 죽게 되는데 이러한 것을 메모리 릭(Memory Leak) 이라고 한다.  
C나 C++에서는 메모리 관리를 사용자가 직접 하게 되는데, 자바에서는 JVM이 메모리 관리를 대신 해준다.  
JVM에서 더 이상 사용하지 않는 메모리를 해제하는 동작을 가비지 컬렉션(Garbage Collection)이라고 한다.  

## Java 메모리 관리
![JVM Arch](https://www.javainterviewpoint.com/java-virtual-machine-architecture-in-java/jvm-architecture/) 
JVM 메모리 영역에는 Heap, Stack, Method, PC Register 등의 영역이 존재한다.  

Method 영역은 컴파일된 자바 바이트 코드인 .class 파일의 정보가 저장된다.  
Heap 영역은 동적으로 생성된 객체의 정보가 저장된다. 즉 new 로 생성된 Object 타입의 데이터가 할당된다.   
Stack 영역은 정적으로 할당된 메모리 영역으로 Primitive 타입(boolean, short, int, long, float double)의 값과 object 타입의 데이터 참조 값이 할당된다.  
PC Register는 스레드 별로 실행되는 명령어 주소가 저장된다.  
Native Method Stack는 자바 이외의 언어로 작성된 코드가 저장되는 Stack 영역이다.  

위 영역 중 Method, Heap 영역은 모든 스레드가 데이터를 공유하고 하나의 공간이 존재하고 나머지 영역은 스레드 별로 공간이 존재하게 된다.  

![JVM Stack Heap](https://i.imgur.com/Kv9ichJ.gif) 
참조: https://deepu.tech/memory-management-in-jvm/

## 가비지 컬렉션 과정
![Unreachable Objects](https://t1.daumcdn.net/cfile/tistory/99B649355C6708ED08) 
가비지 컬렉터는 더 이상 참조되지 않는 객체(Unreachable Object)를. 가비지로 판단하고 이 객체의 메모리 공간을 회수한다.  
### Stop The World
JVM이 가비지 컬렉션을 위해 어플리케이션 실행을 멈추는 작업이다.  
GC을 수행할 때에는 GC를 제외한 모든 스레드들의 작업이 중단되고 GC가 완료되면 작업이 재개된다.  
따라서 GC를 수행하는 시간을 줄여야 한다.  
### Mark and Sweep
![Mark and Sweep](https://t1.daumcdn.net/cfile/tistory/997FE9375DDCC3920C) 
Stop The World를 통해 작업이 중단되면 가비지 컬렉터는 다음과 같은 동작을 한다.
1. Stack의 모든 변수를 스캔하며 어떤 객체가 참조하고 있는지 찾아서 마킹한다.
2. Reachable Object가 참조하고 있는 객체도 찾아서 마킹한다.
3. 마킹되지 않은 객체를 Heap에서 제거한다.
이 때 1,2 번의 과정을 Mark, 3번의 과정을 Sweep이라고 한다.  

![Mark Sweep Compact](https://velog.velcdn.com/images%2Ftkadks123%2Fpost%2Fd60f4c41-860f-4dce-9457-a72e5af4209f%2Fmsc.png) 
sweep 이후에 데이터를 모아줄 수 있는데 이 과정을 Compact라고 한다.

위 알고리즘은 언제 참조가 멈추는지 알 수 없으므로 의도적으로 GC를 실행시켜야 동작할 수 있다.  

### 언제 GC가 발생하는가?
![Heap 구조](https://t1.daumcdn.net/cfile/tistory/99C2E8455C34355326)  

Heap 영역은. 한 번에 모든 메모리가 정리되어 앱이 버벅이는 것을 방지학 위해 Young Generation과 Old Generation 영역으로 구분된다.  

Young Generation은 짧게 살아남는 메모리가 존재하는 공간이다.  
모든 객체는 최초에 Young Generation에서 생성된다.  
Old Generation에 비해 공간이 작아 메모리 상의 객체를 찾아 제거하는데 적은 시간이 소모된다.  
Young Generation에서 발생하는 GC를 Minor GC라고 부른다.

Old Generation은 길게 살아남는 메모리들이 존재하는 공간이다.  
Young Generation에서 생성되었으나 GC 중에 제거되지 않은 경우 Old Generation으로 이동한다.  
Young Generation에 비해 상대적으로 큰 공간을 가지고 잇어 메모리 객체 제거에 많은 시간이 걸린다.  
Old Generation에서 발생하는 GC를 Major GC라고 부른다.  

GC 과정은 다음과 같다.
1. 최초 생성된 객체는 Eden 공간에 할당된다.
2. 만약 Eden 공간이 가득 차면 Minor GC가 발생한다.
3. Mark and Sweep을 통하여 Reachable 객체는 모두 S0(Survival 0) 공간으로 가게 된다. Unreachable 객체는 모두 삭제된다.
4. 다음 Minor GC에서 동일한 일이 발생한다. Unreachable 객체는 모두 삭제되고 Reachable 객체는 survival 공간으로 이동한다. 하지만, 이번에는 S0가 아닌 S1으로 이동한다. 그리고 기존 S0에 존재하는 객체는 S1으로 이동한다. 그리고 기존 S0에 존재하는 객체는 AGE가 1 증가하게 된다.
5. 다음 Minor GC에서도 해당 과정을 반복한다. 하지만 기존 S1의 공간이 차 있다면 S0로 이동하게 되고 만약 S0 공간이 차 있다면 S1으로 이동하게 된다.
6. Minor GC 이후에 객체의 나이가 임계값을 넘는다면 Old Generation 공간으로 객체가 이동하게 된다. 이것을 promotion이라고 한다.
7. Minor GC를 계속 반복하여 모든 Old Generation이 차게 되면 Major GC가 발생한다.
[참조](https://www.oracle.com/webfolder/technetwork/tutorials/obe/java/gc01/index.html)  

## 가비지 컬렉션 알고리즘
### Reference Counting
![Reference Counting](https://blog.kakaocdn.net/dn/dnVVsF/btrriscwVk3/7VMjYa6heR6Gy0Fd4SSAt0/img.png) 
각각 Heap 영역의 객체가 자신을 참조하는 count 값을 가지고 참조하는 값이 0인 경우 해제한다.  
해당 알고리즘은 순환 참조 시에 Reference Count 값이 0이 되지 않는 순환 참조 문제가 있어 사용하지 않는다.  
### Serial GC
하나의 스레드로 GC를 실행한다.  
따라서 Stop the World 시간이 길다.  
싱글 스레드 환경 및 Heap이 매우 작을 때 사용한다.  
### Parallel GC
여러개의 스레드로 GC를 실행한다.  
멀티코어 환경에서 사용한다.  
Java8의 default GC 방식이다.  
### Concurrent Mark Sweep GC
![CMS GC](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fn7YwT%2FbtrpLtdvcFM%2FHBn7XPTZkdAkcIRjHxRkNk%2Fimg.png) 
Stop The World 최소화를 위해 고안하였다.  
멀티 스레드 환경에서 동작할 수 있다.  
GC 작업을 4단계로 나누어 어플리케이션과 동시에 실행한다.  
1. Initial Mark: GC Root가 클래스 로더에서 가장 가까운 객체 중 살아있는 객체만 마킹 (SWT 발생)
2. Concurrenct Mark: 참조하는 객체를 따라가면서 지속적으로 마킹
3. Remark: Concurrent Mark 단계에서 새로 추가되거나 참조가 사라진 객체를 다시 확인 (SWT 발생)
4. Concurrent Sweep:  참조되지 않는 객체를 삭제
위 알고리즘은 별도의 Compaction이 없다는 단점이 있다.  

G1 GC의 등장으로 더 이상 사용하지 않는다.  
### G1 GC (Garbage First)
![G1 GC](https://i0.wp.com/thinkground.studio/wp-content/uploads/2020/11/201106_g1gc-heap-area.png?w=1373&ssl=1) 
Java9부터 default GC 방식이다.   
```Console
foo@bar:~$ java -XX:+PrintCommandLineFlags -version
-XX:ConcGCThreads=2 -XX:G1ConcRefinementThreads=8 -XX:GCDrainStackTargetSize=64 -XX:InitialHeapSize=268435456 -XX:MarkStackSize=4194304 -XX:MaxHeapSize=4294967296 -XX:MinHeapSize=6815736 -XX:+PrintCommandLineFlags -XX:ReservedCodeCacheSize=251658240 -XX:+SegmentedCodeCache -XX:+UseCompressedClassPointers -XX:+UseCompressedOops -XX:+UseG1GC
openjdk version "19.0.1" 2022-10-18
OpenJDK Runtime Environment (build 19.0.1+10-21)
OpenJDK 64-Bit Server VM (build 19.0.1+10-21, mixed mode, sharing)
```
대용량 메모리와 멀티 프로세싱 환경에서 최적화된 서버 스타일의 가비지 컬렉터이다.  
Heap 영역을 물리적으로 구분하지 않고 논리적으로 구분한다.  
각 영역이 고정되어있지 않고 Region의 상태에 따라 변동된다.  
CMS GC와 유사한 방식으로 동작한다.  
CMS GC에 비하여 Heap 영역의 리전 별 크기를 조절할 수 있고, 중단 없이 Compaction이 가능하다는 장점이 있다.  
[참조](https://www.oracle.com/technetwork/tutorials/tutorials-1876574.html)  

