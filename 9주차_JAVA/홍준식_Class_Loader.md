# JVM
OS에 종속받지 않고 CPU가 Java를 실행시킬 수 있도록 하는 가상 컴퓨터  
![JVM](https://user-images.githubusercontent.com/51476083/113502632-7959d580-9568-11eb-8457-edc8b60b0bac.png) 
Java 소스코드(`*.java`)는 CPU가 읽어들일 수 없어 기계어로 컴파일을 해줘야 한다.  
하지만 Java는 JVM이라는 가상 머신을 거쳐서 OS에 도달하기 때문에 JVM이 인식할 수 있는 Java Bytecode(`*.class`)로 변환한 뒤에 JVM이 CPU가 읽을 수 있는 기계어로 변환을 한다.  
따라서 OS에 상관 없이 생성된 Java Bytecode를 통해 코드를 실행할 수 있다.  

Java Bytecode는 컴파일러에 의하여 변환된 코드의 명령어가 1바이트라서 자바 바이트 코드로 불린다.  

```console
foo@bar:~$ ls
main.java
foo@bar:~$ javac main.java
foo@bar:~$ ls
main.class main.java
foo@bar:~$ java main
Hello World!

foo@bar:~$ od -tx1 main.class
0000000    ca  fe  ba  be  00  00  00  3f  00  1c  0a  00  02  00  03  07
0000020    00  04  0c  00  05  00  06  01  00  10  6a  61  76  61  2f  6c
0000040    61  6e  67  2f  4f  62  6a  65  63  74  01  00  06  3c  69  6e
0000060    69  74  3e  01  00  03  28  29  56  09  00  08  00  09  07  00
0000100    0a  0c  00  0b  00  0c  01  00  10  6a  61  76  61  2f  6c  61
0000120    6e  67  2f  53  79  73  74  65  6d  01  00  03  6f  75  74  01
0000140    00  15  4c  6a  61  76  61  2f  69  6f  2f  50  72  69  6e  74
0000160    53  74  72  65  61  6d  3b  08  00  0e  01  00  0c  48  65  6c
0000200    6c  6f  20  57  6f  72  6c  64  21  0a  00  10  00  11  07  00
0000220    12  0c  00  13  00  14  01  00  13  6a  61  76  61  2f  69  6f
0000240    2f  50  72  69  6e  74  53  74  72  65  61  6d  01  00  07  70
0000260    72  69  6e  74  6c  6e  01  00  15  28  4c  6a  61  76  61  2f
0000300    6c  61  6e  67  2f  53  74  72  69  6e  67  3b  29  56  07  00
0000320    16  01  00  04  6d  61  69  6e  01  00  04  43  6f  64  65  01
0000340    00  0f  4c  69  6e  65  4e  75  6d  62  65  72  54  61  62  6c
0000360    65  01  00  16  28  5b  4c  6a  61  76  61  2f  6c  61  6e  67
0000400    2f  53  74  72  69  6e  67  3b  29  56  01  00  0a  53  6f  75
0000420    72  63  65  46  69  6c  65  01  00  09  6d  61  69  6e  2e  6a
0000440    61  76  61  00  21  00  15  00  02  00  00  00  00  00  02  00
0000460    01  00  05  00  06  00  01  00  17  00  00  00  1d  00  01  00
0000500    01  00  00  00  05  2a  b7  00  01  b1  00  00  00  01  00  18
0000520    00  00  00  06  00  01  00  00  00  01  00  09  00  16  00  19
0000540    00  01  00  17  00  00  00  25  00  02  00  01  00  00  00  09
0000560    b2  00  07  12  0d  b6  00  0f  b1  00  00  00  01  00  18  00
0000600    00  00  0a  00  02  00  00  00  03  00  08  00  04  00  01  00
0000620    1a  00  00  00  02  00  1b
0000627
```
컴파일된 클래스 파일의 포멧은 [링크](https://docs.oracle.com/javase/specs/jvms/se7/html/jvms-4.html) 를 통해서 확인할 수 있다.  

### Javap
컴파일된 Java Bytecode는 javap를 통하여 disassemble 할 수 있다. [참조](https://docs.oracle.com/javase/8/docs/technotes/tools/windows/javap.html)  
javap를 통하여 disassemble된 바이트코드나 사용되는 스택 사이즈, 클래스 정보, java version 등을 확인할 수 있다.
```console
foo@bar:~$ ls
main.class main.java
foo@bar:~$ javap main
Compiled from "main.java"
public class main {
  public main();
  public static void main(java.lang.String[]);
}
```

## JVM 구성
-  자바 인터프리터(interpreter)
- 클래스 로더(class loader)
- JIT 컴파일러(Just-In-Time compiler)
- 가비지 컬렉터(garbage collector)
![JVM 구성](https://upload.wikimedia.org/wikipedia/commons/thumb/d/dd/JvmSpec7.png/800px-JvmSpec7.png)  

1. 프로그램이 실행되면 JVM은 OS로부터 프로그램이 필요로 하는 메모리를 할당받는다.
2. 자바 컴파일러가 소스 코드를 읽어들여 자바 바이트 코드로 변환한다.
3. 클래스 로더를 통하여 자바 바이트 코드를 JVM 메모리에 로딩한다.
4. JVM에 로딩된 자바 바이트코드 파일들을 Execution Engine의 Interpreter와 JIT Compiler를 통해 해석한다.
5. 해석된 바이트 코드는 JVM 메모리에 배치되어 수행된다.
6. Interpreter를 통하여 바이트 코드를 해석하며 추가로 필요한 클래스가 있는 경우 클래스 로더를 통하여 읽어들인다.

### 자바 인터프리터
자바 컴파일러에 의해 변환된 바이트 코드를 읽고 한줄씩 기계어로 해석한다.
### 클래스 로더
자바는 동적으로 클래스를 읽어온다.  
필요한 순간에 클래스 파일을 찾아 메모리에 로딩해준다.  
### JIT 컴파일러 (Just-In Time Compiler)
인터프리터 방식의 단점을 보완하기 위해 도입된 컴파일러이다.  
실행 시점에 인터프리터 방식으로 기계어를 생성할 때에 자주 사용되는 메소드의 경우 컴파일하고 기계어를 캐싱한다. 이를 통해 해당 메소드가 여러 번 호출될 때 매번 해석하는 것을 방지한다.  
### 가비지 콜렉터
메모리 관리를 자동으로 해주어 개발자가 메모리에 대한 고려를 하지 않도록 해준다.  

## 클래스 로드 과정
로딩, 링킹, 초기화 과정을 가진다.
### 로딩
class 파일을 읽어들여 JVM 메모리 method area에 저장한다. 
각각의 class 파일들에 JVM은 FQCN(Fully Qualified Class Name)과 부모의 이름, class 파일이 class, Interface, Enum과 관계가 있는지 여부와 변수와 함수 정보 등을 method 영역에 저장한다.

클래스 파일을 로딩한 후에 JVM은 Class 타입의 객체를 heap memory에 생성한다.
이때, Class 타입의 객체는 java.lang 패키지에 미리 선언되어있다.  
이 클래스 객체는 클래스 레벨의 정보를 가져오기 위해서 `getClass()` 메소드를 통하여 프로그래머에 의해 사용될 수 있다.
모든 로딩된 class 파일에 대하여 하나의 클래스의 객체가 생성된다.  
```Java
Student s1 = new Student();
Student s2 = new Student();

System.out.println(s1.getClass() == s2.getClass());  // true
```
### 링킹
검증, 준비, 분석(Optional)을 수행한다.
- 검증: .class 파일의 정확성을 검증한다. 검증된 컴파일러릍 통해 생성되고 포메팅 되었는지 확인한다. 만약 검증이 실패한다면 java.lang.VerifyError을 발생시킨다. 이 행동은 ByteCodeVerifier에 의해 수행된다.
  해당 과정은 다소 오버헤드가 발생하여 성능 향상을 위해 진행하지 않을 수 있다.
- 준비: JVM이 class 스태틱 변수와 기본값을 위한 공간을 준비한다.
- 분석: 선택적으로 진행되는 과정으로 환경에 따라 동작 유무가 정해진다. 심볼릭 메모리 참조를 메소드 영역의 실제 레퍼런스로 교체한다.
### 초기화
링킹의 준비 단계에 확보한 메모리 영역에 값들을 할당한다.
## 클래스 로더 모델
일반적으로 클래스 로더는 3가지 모델이 있다.  
![Class Loader](https://blog.kakaocdn.net/dn/csG0Ot/btqKXinWA1V/wZ7sIqo4mOczdj27bwublk/img.png) 
- Bootstrap Class Loader: `JAVA_HOME/jre/lib`에 있는 core java API 클래스를 로드한다.
- Extension Class Loader: `JAVA_HOME/jre/lib/ext`에 있는 core java API 클래스를 로드한다.
- Application Class Loader: `classpath`에 있는 core java API 클래스를 로드한다.

JVM은 클래스를 로드하기 위해 위임 모델을 사용한다. Application Class Loader는 Extension ClassLoader에, Extension Class Loader는 Bootstrap Class Loader에 요청을 위임한다. 만약 class가 Bootstrap Class Loader에 없다면 Extension Class Loader가, 여기에도 없다면 Application Class Loader가 클래스를 탐색하고 없다면  오류를 반환한다.

### 클래스 로더 원칙
- 위임 원칙: 클래스를 찾기 위한 요청을 받았을 때에 상위 클래스 로더에 책임을 위임한다.
- 가시 범위 원칙: 하위 클래스 로더는 상위 클래스 로더가 로드한 클래스를 볼 수 있지만 상위 클래스 로더는 하위 클래스 로더가 로드한 클래스를 알 수 없다.
- 유일성의 원칙: 하위 클래스 로더가 상위 클래스 로더가 로드한 크래스를 다시 로드하지 않는다.

## JVM 메모리
- Method area: 클래스와 인터페이스의 정보가 저장되는 영역이며, 클래스 생성자 및 메소드 코드(바이트 코드) 등이 저장된다.
- Heap: new 연산자 등으로 생성된 객체와 배열 등을 저장한다. Heap 영역에 저장된 객체나 배열은 다른 객체에서 참조될 수 있다. 가비지 컬렉션이 발생하는 영역이다.
- JVM Language Stacks: 메소드 호출 시 수행중인 메소드 데이터를 저장하기 위한 영역이다. 지역변수, 지역객체 레퍼런스, 메소드 파라미터, 리턴값 등을 저장한다. 스텍 영역은 스레드 별로 독립적으로 생성되고 메소드 진입시 데이터가 생성되며 메소드가 종료되면 POP 되어 사라진다.
- PC Registers: 각 스레드 별로 할당되며 스레드가 시작될 때에 생성된다. 스레드가 실행할 명령의 주소를 저장한다.
- Native Method Area: 자바 이외의 언어(Native Code, JNI)로 작성된 코드를 위한 Stack 영역이다.
