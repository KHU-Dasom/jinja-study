# Java 컨벤션 규칙

## ✏️ 개요

<https://google.github.io/styleguide/javaguide.html>에서 제공하는 문서 번역

## ✏️ 1. Introduction

- `class`라는 용어는 일반적인 클래스, `enum`, `interface` 또는 `@interface`을 포함한다.
- `member`라는 용어는 중첩 클래스, 필드, 메소드, 생성자를 포함한다. 즉, 클래스의 초기화 코드와 주석 제외한 것들을 의미한다.
- '주석'이라는 용어는 항상 구현 주석을 의미한다. 보통 `javadoc`라고 한다.

-----

## ✏️ 2. Source file basics

### 2.1 File name

- 소스 파일은 최상위 클래스의 대소문자를 구분하는 이름과 .`java`확장자로 구분된다.

### 2.2 File encoding: UTF-8

- 파일 인코딩은 `UTF-8`로 설정한다.

### 2.3 Special characters

### 2.3.1 Whitespace characters

줄 끝을 나타내는 문자(`\n`, 등)을 제외하고, 띄어쓰기(0x20)는 소스파일에서 어떤 곳에서도 나타나는 유일한 공백 문자이다.

- 탭 문자는 들여쓰기로 사용되지 않는다. 즉, 코드의 들여쓰기는 스페이스 문자나 스페이스 문자 여러개를 사용해야한다는 뜻이다.
    - 그 이유는 탭 문자를 사용할 경우 들여쓰기가 일관적이지 않을 수 있기 때문이다.

```java
if (x > 0) {
  // 탭 문자가 사용된 들여쓰기
    System.out.println("x is positive.");
} else {
  // 스페이스 문자가 사용된 들여쓰기
  System.out.println("x is non-positive.");
}
```

### 2.3.2 Special escape sequences

`\b`,`\t`,`\n`,`\f`,`\r`,.. 등과 같은 특수한 이스케이프 시퀸스는 이대로 사용되어야지, 유니코드나 8진수로 표현하면 안된다.

```java
//correct
String s = "This is a string with a newline.\n";
char c = '\n';
//wrong
String s = "This is a string with a newline.\u000a";
char c = '\u000a';
```

-----

## ✏️ 3. Source file structure

소스파일은 다음의 순서로 구성된다.

1. 라이선스, 저작권 정보(있다면)
2. 패키지 선언
3. 임포트 선언
4. 정확히 한 개의 최상위 클래스

각 세션 사이는 하나의 라인으로 분리할 것.

- 1) 라이선스, 저작권 정보: 해당 정보는 파일 바로 아래에 적으면 된다.
- 2) 패키지 선언: 줄 바꿈이 되지 않고, 열 제한은 **100**은 **패키지 선언에 적용되지 않는다.**
- 3) 임포트 선언
  - `*`를 사용한 와일드카드 임포트나 `static`을 사용한 임포트를 사용하지 않는다.
    > 그 이유는 해당 심볼이 어디서 가져와진건지 이해하기 어려울 수도 있고, 여러 모듈에서 동일한 심볼을 호출할 경우 **충돌**이 발생할 수 있다.
  - 여러 심볼을 임포트할 때 여러줄로 나누어 가져오거나, 줄 바꿈을 해서 가져오면 안된다. 100자를 초과하더라도 한 줄로 가져와야 한다.

    ```java
    //이러면 안 됨.
    import java.util.List, java.util.ArrayList,
        java.util.Map, java.util.HashMap;

    ```

  - `static`이든 `non-static`이든 import문은 한 줄에 하나씩 작성한다.

    ```java
    import mypackage.MyClass;
    import mypackage.MyInterface;

    import java.util.List;
    import java.util.ArrayList;
    import java.util.Map;
    import java.util.HashMap;

    import com.example.util.StringUtils;
    import org.example.data.DataModel;
    ```

  - static import할 때 중첩 클래스를 임포트하지 않도록 권고한다.

- 4) 한 개의 최상위 클래스가 소스파일에 존재해야한다.
  - 메소드나 멤버변수 등이 추가될 때는 어떠한 논리로 의해 순서가 정해져있으며, 잘 설명할 수 있기만 하면된다.즉, 습관적으로 멤버변수나 메소드를 바로 밑에 추가하는 방식을 하는게 아니다.

-----

## ✏️ 4. Formatting

### 4.1 Brace

- `if`,`else`,`for`,`do`,`while` 등을 사용할 때, 내용이 비어있거나 단일 표현식이라해도 Braces를 사용해야 한다.
- Nonempty blocks
  - `{`뒤에는 줄바꿈이 있어야한다.
  - `}` 전에는 줄바꿈이 있어야한다.
  - 몇 가지 예외가 있는데, `}`뒤에 `;`가 나오는 경우나 `else if`문, `else`문 등의 경우가 있다.

```java
return () -> {
  while (condition()) {
    method();
  }
};

return new MyClass() {
  @Override public void method() {
    if (condition()) {
      try {
        something();
      } catch (ProblemException e) {
        recover();
      }
    } else if (otherCondition()) {
      somethingElse();
    } else {
      lastThing();
    }
    {
      int x = foo();
      frob(x);
    }
  }
};
```  

- Empty blocks: `{}`이 구문 쓰일 수 있다. 하지만, **multi-block** statement에서는 불가능하다.

예를 들어서

```java
// 가능
void doNothing() {}

// 가능
void doNothingElse() {
}

// 불가능
try {
  doSomething();
} catch (Exception e) {}
```

### 4.2 Block Indentation

- block안에 코딩을 할때는 2칸을 띄어라. block이 끝나면 이 전 idenet를 따르라는 의미도 포함.

### 4.3 One statement per line

- 각각의 표현식은 한 줄로 작성한다.

### 4.4 Column limit: 100

- Java 코드는 줄마다 100자의 `character` 제한이 있다. 
  - 예외1: javadoc의 긴 URL 등을 참조하는 경우
  - 예외2: 제일 첫 상단의 package구문이나 `import`구문
  - 예외3: 매우긴 식별자(대부분 써드파티 라이브러리나 API를 말하는 듯. 통상적으로는 길게 작성하는건 안됨.)  

  ```text
  customer_order_transaction_history와 같은 DB에 접근할 때 등
  ```

### 4.5 Line-wrapping

**Line-wrapping**이란, 한 줄을 합법적(?)으로 코드가 여러줄로 분할시키는 일련의 작업을 말한다.

이 작업을 통해 코드 가독성을 높이고, 더 이해하기 쉽게 만든다.

- 줄바꿈에 대한 정확한 가이드라인은 없지만, 어떠한 패턴이 있다고 한다.
- 함수나 지역변수로 추출하는 방식은 굳이 Line-wrapping을 사용하게 안할 수도 있다.
- 일반적으로 열 제한에 걸리는 경우 사용하지만, 굳이 걸리지 않더라도 개발자의 재량에 따라 사용할 수 있다.

#### Where to break

- 라인을 나누는 곳은 대입 연산자 이외의 연산자 앞에서 나누는 것을 권장한다고 한다.

```java
int result = number1
             + number2
             + number3;

float sum = 
            float1 +
            float2;

```

- 대입 연산자 앞 또는 뒤에 라인을 나누는것도 가능은 하다.
- 메소드나 생성자에 구문 다음에 나오는 `(`는 같은 줄에 작성해야 한다.

```java
//wrong
public void 
    doSomething() {
    // code here
}

//correct
public void doSomething() {
    // code here
}
```

- 람다식에 나오는 화살표는 바로 라인을 분리할 수 없도록 해야한다. 단, 다음에 나오는 구문이 한 줄인 경우는 가능하다.

```java
MyLambda<String, Long, Object> lambda =
    (String label, Long value, Object obj) -> {
        ...
    };

Predicate<String> predicate = str ->
    longExpressionInvolving(str);
```

- 줄바꿈을 수행할 때 첫 줄은 원래 줄의 +4 이상 들여쓰기를 해야한다.

### 4.6 Whitespace

#### Vertical Whitespace

가독성을 위해 공백을 추가하는 것이다.

```java
class MyClass {
    public void myMethod() {
        // code here
    }
}

보다는

class MyClass {

   public void myMethod() {

       // code here

   }
}
```

이런식으로 각 메서드 사이나, 생성자와 멤버변수 사이 등 집어 넣을 수 있다.

#### Horizontal whitespace

- `if`, `for`, `catch` 등의 구문에서 `(`를 분리하여 작성하도록 한다.

```java
if(x>0)

//보다는
if (x > 0)

//를 사용하자고 한다.
```

- `else`, `catch`와 같은 예약어는 `}`를 분리하여 작성하도록 권장하고 있다.

```java
}else

} else
```

- `<T extends Foo & Bar>`처럼 `&`로 두 타입을 이을때도 분리하여 사용하도록 권장하고 있다.
- `catch (FooException | BarException)`처럼 `|`(파이프라인)연산자를 사용할 경우에도 분리하도록 권장하고 있다.
- `foreach()` 구문의 `:`연산자를 사용할 때에도
- `->`를 이용한 람다식에서도 ex) `(String str) -> str.length()` 분리하도록 권장하고 있다.

단, `::`같은 메서드 참조나 `.`을 이용한 메서드 접근자는 제한다.

- `:`, `,`, `;` 혹은 `)` 다음에는 공백을 두지 않도록 한다. (사이는 공백을 두는것을 권장)
- 배열같은 요소를 초기화할때는 이 두 용법 전부 허용한다.

```java
new int[] {5, 6}
new int[] { 5, 6 }
```

#### Horizontal alignment: never required

수평정렬 하지마.

```java
private int    x; // 허용하지만 나중에 수정할 가능성이 크다.
private String str; 
```

### 4.7 Grouping parentheses: recommended

```java
if (x > 0 && x < 10)
```

처럼 어떠한 연산을 위해 **괄호**를 통해 그룹으로 묶어버리는것을 말한다. 이를 적극 활용하라는 이야기.

### 4.8 Specific constructs

#### Enum classes

보통 `,`뒤에 줄바꿈을 해주는데 특별한 경우는 추가적인 공백을 넣어줄 수 있다.

```java
private enum Answer {
  YES {
    @Override public String toString() {
      return "yes";
    }
  },

  NO,
  MAYBE
}
```

나는 이렇게 쓰는 경우가 있는지 잘 모르겠으니, 알아서 그렇구나 하고 넘어가자.

그리고 `enum`에 함수가 없거나 추가적인 언급이 없는 경우 **배열**처럼 쓸 수 있다.

```java
private enum Suit { CLUBS, HEARTS, SPADES, DIAMONDS }
```

#### Variable declarations

```java
int a, b;  //(x)
int a;
int b;
```

- 즉, 선언은 한 변수당 한 줄에 쓰자고 권장한다. `for`문에서 선언되는 경우는 제외
- 지역변수는 습관적으로 맨 상단에 선언하는 경우가 있는데, 이 보다는 범위를 최소화하기 위해 실질적으로 쓰이는 곳 주변에 적절히 선언하는 것이 좋다.
- 초기화의 경우는 선언과 같이 해주거나, 선언 직후 해주는 편이 좋다.

#### Array

```java
new int[] {           new int[] {
  0, 1, 2, 3            0,
}                       1,
                        2,
new int[] {             3,
  0, 1,               }
  2, 3
}                     new int[]
                          {0, 1, 2, 3}
```

다 된다.

- `C`스타일로 쓰지마라. `String[] args`지, `String args[]`가 아니다.

#### Switch statements

- `Switch`구문의 indent 는 최소 **+2**로 한다.
- `fall-through`을 의도적으로 발생시키면 주석을 달아 놓는다.

```java
switch (input) {
  case 1:
  case 2:
    prepareOneOrTwo();
    // fall through
    // 일부러 case1을 통과한 이유를 적으면 된다.
  case 3:
    handleOneTwoOrThree();
    break;
  default:
    handleLargeNumber(input);
}
```

- 추가적인 구문이 없다해도 `default`는 적어준다. 단 `enum`클래스를 사용하는 경우 생략할 수 도 있다.

#### Annotations

- 만약 자료형 어노테이션을 사용하면 특정 자료형 앞에 나와야 한다.

```java
final @Nullable String name;
```

- 만약 클래스에 붙는 어노테이션을 사용할 경우 각 어노테이션은 하나에 한 줄을 사용한다. 

```java
@Deprecated
@CheckReturnValue
public final class Frozzler { ... }
```

- 메소드나 생성자에 붙는 어노테이션도 위와 동일하다.
- 필드형 어노테이션도 위와 동일하지만, 여러개의 어노테이션이 붙는 경우에는 같은 라인에 쓸 수도 있다.

```java
@Partial @Mock DataLoader loader;
```

#### Comments

```java
/*
 * 만약 이렇게 새 줄이 시작 되면 '*'을 처음 넣어주면 좋다.
 * This is          // And so           /* Or you can
 * okay.            // is this.          * even do this. */
 */
```

다 된다.

#### Modifiers

다음과 같은 순서로 선언문을 작성하면 된다.

```text
public protected private abstract default static final transient volatile synchronized native strictfp
```

#### Numeric Literals

`Long`타입을 사용하여 변수를 초기화 해주는 경우 `l`을 쓰지말자. 1과 혼동한다. 즉, `3000L`처럼 `L`을 사용하자.

-----

## ✏️ 5. Naming

### 5.1 Rules common to all identifiers

**Google Style**에서는 특별한 접두사나 접미사가 불필요하다. `name_`, `mName`, `s_name`, `kName`과 같은거 필요 없다.

### 5.2 Rules by identifier type

- `package` 이름은 소문자와 숫자만을 사용하여 작성한다. 

- `Class`의 이름은 **카멜규칙**을 적용한다. 첫 글자는 대문자를 사용한다.

- `Test` 클래스의 이름은 끝에 `Test`를 붙인다. 

- `Method`의 이름은 **카멜규칙**을 사용하지만, 클래스와 다르게 첫 글자는 소문자로 쓴다.

- `Constant`의 이름은 **스네이크규칙**을 적용한다. 모두 **대문자**를 쓴다.

```java
// Constants
static final int NUMBER = 5;
static final ImmutableList<String> NAMES = ImmutableList.of("Ed", "Ann");
static final Map<String, Integer> AGES = ImmutableMap.of("Ed", 35, "Ann", 32);
static final Joiner COMMA_JOINER = Joiner.on(','); // because Joiner is immutable
static final SomeMutableType[] EMPTY_ARRAY = {};
```

- `Non-Constant`의 이름은 **카멜규칙**을 적용한다. 첫 글자만 소문자를 사용한다.

- `Parameter`의 이름도 위와 동일하다.

- `Local variable`도 동일하다.

- `Type varaible`은 하나의 문자인 경우 `T`, `E`, `X2`처럼 대문자를 사용하고 문자 뒤에 숫자가 나온다. 그렇지 않은 경우 `Class`의 규칙을 적용한다. 그리고 뒤에 `T`를 붙인다. ex) `RequestT` ...

### 5.3 Camel case: defined

![image](https://user-images.githubusercontent.com/30401054/213167696-82ad474f-0589-47a2-889b-aeac72feac6a.png)

눈으로 확인하라

## ✏️ 6. Programming Practies

### 6.1 @Override: always used

- `@Override` 무조건 써라. 단 한 가지의 예외가 있다. 만약 부모의 함수가 `@Deprecated`에 의해 생략될 수 있는 경우에만

### 6.2 Caught exceptions: not ignored

- 예외를 잡았을때 해당 예외를 무시하지말라.

- 예시로 아무런 조치를 취하지 않는다면 주석을 달아놓은 코드

```java
try {
  int i = Integer.parseInt(response);
  return handleNumericResponse(i);
} catch (NumberFormatException ok) {
  // it's not numeric; that's fine, just continue
}
return handleTextResponse(response);
```

예외를 잡았다면 어떤 조치를 취해야한다.

```java
e.printStatckTrace()
```

같은 코드를 넣으면서 자신의 Custom Error를 발생시키는것도 방법이다.

### 6.3 Static members: qualified using class

- 만약 정적 멤버(static member)를 사용할 때 해당 멤버를 소유하는 클래스를 참조하도로 권고한다.

```java
Foo aFoo = ...;
Foo.aStaticMethod(); // good
aFoo.aStaticMethod(); // bad 인스턴스를 만들어서 사용하는 방법
somethingThatYieldsAFoo().aStaticMethod(); // very bad 어떤 함수가 반환하는 객체의 메서드를 호출하는 방법
```

### 6.4 Finalizers: not used

- `finalizer`를 사용하지마. 자바18에서부터 얘를 없앨 준비를 하고 있다.

- 해당 메서드는 객체의 리소스를 해제시키는 등의 처리를 할 수 있는데, 보통 가비지 컬렉터가 실행될 때 사용된다. 즉, 프로그래머가 해당 메서드를 사용하면 가비지컬렉터의 효율성이 저하될 수 있으며, 안정성 또한 저하될 수 있다.

## 🔅 Reference

- ChatGPT
- <https://google.github.io/styleguide/javaguide.html>