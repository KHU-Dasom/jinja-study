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

- 라이선스, 저작권 정보: 해당 정보는 파일 바로 아래에 적으면 된다.
- 패키지 선언: 줄 바꿈이 되지 않고, 열 제한은 **100**은 **패키지 선언에 적용되지 않는다.**
- 임포트 선언
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




