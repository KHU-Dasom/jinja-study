# **[Java] String**

## **`String` 클래스**

Java에서 `String` 클래스는 문자열을 다루는 클래스이다. 클래스 선언부는 다음과 같다.

```java
public final class String
    implements java.io.Serializable, Comparable<String>, CharSequence {

    @Stable
    private final byte[] value;

    private final byte coder;

    private int hash;

    private static final long serialVersionUID = -6849794470754667710L;

    /* ... */
    }
```

String 클래스가 `Serializable`, `Comparable` 그리고 `CharSequence` 인터페이스를 구현하는 것을 볼 수 있다. 또한 내부 변수로 바이트 배열의 `value`를 가지며 해당 변수를 통해 문자열을 나타낸다.

클래스 내부의 `coder` 변수는 문자열 인코딩 방식을 나타내는데, `LATIN1` 방식을 사용하면 `0`을, `UTF-16` 방식을 사용하면 `1`을 가진다.

String 객체를 생성하는 방식은 크게 두 가지이다.

```java
// 1. 리터럴을 이용하여 초기화
String str1 = "Hello World!";

//2. new 연산자를 이용하여 초기화
String str2 = new String("Hello World!");
```

두 방식에는 큰 차이가 있는데, 해당 차이를 알려면 String Constant Pool에 대한 이해가 필요하다. String Pool이 무엇일까?

![Java_String_Pool](https://d1jnx9ba8s6j9r.cloudfront.net/blog/wp-content/uploads/2017/05/String-pool-1.png)

일반적으로 `String`은 원시 타입이 아닌 레퍼런스 타입이기 때문에 힙 영역에 데이터를 가지며 이를 가리키는 변수를 스택 영역에 가진다. 

리터럴을 사용하여 `String` 객체를 초기화할 때는 JVM에서 관리되는 String Constant Pool에 해당 리터럴 값을 가지는 데이터를 저장하고 해당 주소를 가리킨다. 반면에 `new` 키워드를 사용하여 `String` 객체를 생성하는 경우, String Pool이 아닌 힙 영역에 데이터를 저장하고 해당 주소를 가리킨다.

해당 방식의 차이는 값을 저장하는 효율성의 차이를 보이는데, 같은 값을 가지는 `String` 객체를 리터럴로 생성하는 경우 String Pool에 저장되기 때문에 단순히 같은 값을 가리키므로 값을 재생성하지 않는다. 반면에 `new` 키워드를 사용하여 String 객체를 생성하는 경우 새로운 데이터를 생성해야 한다.

![Literal_vs_new](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FCZS5V%2FbtqFPrKJ713%2F7k48Gu8Hr6mdNmGO9wtTMK%2Fimg.png)

## **`CharSequence` 인터페이스**

String 클래스는 `CharSequence` 인터페이스를 구현한다. `CharSequence` 인터페이스의 구현부는 다음과 같다.

```java
public interface CharSequence {

    int length();

    char charAt(int index);

    CharSequence subSequence(int start, int end);

    public String toString();

    /* ... */
}
```

`CharSequence` 인터페이스는 문자열을 다루는 클래스들을 위한 인터페이스이다. 내부적으로 바이트 배열 및 캐릭터 배열을 다루는 메서드들을 가지며 String 클래스와 같이 문자열을 다루는 클래스들은 이를 구현하여 사용할 수 있다.

## **Java String 다루기**

### **Basic Methods**

```java
@Test
    public void BasicMethodsTest() {
        String str = "Hello World!\n";

        // 1. 문자열 길이
        assertThat(str.length()).isEqualTo(13);

        // 2. 인덱스로 접근
        assertThat(str.charAt(3)).isEqualTo('l');

        // 3. 캐릭터로 인덱스 검색
        assertThat(str.indexOf('l')).isEqualTo(2);

        // 4. 문자열로 인덱스 검색
        assertThat(str.indexOf("llo")).isEqualTo(2);

        // 5. 문자열 안에 해당 문자열 포함 여부 판별
        assertThat(str.contains("World")).isEqualTo(true);

        // 6. 문자열 앞, 끝 부분문자열 판별
        assertThat(str.startsWith("Hello")).isEqualTo(true);
        assertThat(str.endsWith("World!\n")).isEqualTo(true);

        // 7. 빈 문자열, 공백 문자열 판별
        assertThat(str.isEmpty()).isEqualTo(false);
        assertThat(str.isBlank()).isEqualTo(false);

        // 8. 문자열 정규표현식 기반 스플릿
        assertThat(str.split(" ")).isEqualTo(new String[]{"Hello", "World!\n"});

        // 9. 문자열 속 부분 교체
        assertThat(str.replace("H", "h")).isEqualTo("hello World!\n");
        assertThat(str.replace("Hello", "hello")).isEqualTo("hello World!\n");
    }
```

다양한 문자열 조작 메서드들을 제공한다.

### **문자열 연산**

기본적으로 두 문자열을 연결할 때 `+` 연산자를 사용한다.

```java
String str1 = "Hello";
String str2 = " World!\n";
String str3 = str1 + str2;

System.out.println(str3) // Hello World!
```

`String` 클래스에서 제공하는 `concat()` 메서드를 사용할 수도 있다.

```java
String str1 = "Hello";
String str2 = " World!\n";
String str3 = str1.concat(str2);

System.out.println(str3) // Hello World!
```

`concat()` 메서드는 내부적으로 다음과 같이 구현되어 있다.

```java
public String concat(String str) {
    if (str.isEmpty()) {
        return this;
    }
    if (coder() == str.coder()) {
        byte[] val = this.value;
        byte[] oval = str.value;
        int len = val.length + oval.length;
        byte[] buf = Arrays.copyOf(val, len);
        System.arraycopy(oval, 0, buf, val.length, oval.length);
        return new String(buf, coder);
    }
    int len = length();
    int olen = str.length();
    byte[] buf = StringUTF16.newBytesFor(len + olen);
    getBytes(buf, 0, UTF16);
    str.getBytes(buf, len, UTF16);
    return new String(buf, UTF16);
}
```

`concat()` 메서드를 사용하여 두 문자열을 연결하는 경우 총 3개의 메모리 영역이 생성된다. 반면에 `+` 연산자를 사용하여 두 문자열을 연결하는 경우 `concat()` 메서도와는 다른 방식으로 구현되어 있다. `+` 연산자는 Java 1.5부터 지원하는 `StringBuilder`를 사용하여 문자열을 연결한다. `StringBuilder`는 문자열을 연결할 때 기존 문자열을 변경하는 방식으로 구현되어 있다. 따라서 `+` 연산자를 사용하여 문자열을 연결하는 경우 기존 문자열을 변경하는 방식이므로 기존 문자열을 변경하지 않는 `concat()` 메서드보다 성능이 좋다.

```java
StringBuilder sb = new StringBuilder();
sb.append("Hello");
sb.append(" World!\n");
System.out.println(sb.toString()); // Hello World!
```

`+` Java 1.5 이전에는 `concat()` 메서드를 사용했지만 1.5 이후부터는 `StringBuilder`의 등장으로 내부적으로 `StringBuilder`를 사용하여 문자열을 연결한다.

```java
String str1 = "Hello";
String str2 = " World!\n";
String str3 = str1 + str2; // new StringBuilder().append(str1).append(str2).toString();
System.out.println(str3) // Hello World!
```

다만 예외적으로 하나의 라인 안에서 리터럴 값들의 결합은 `StringBuilder`를 사용하지 않고 컴파일 시 결합된다.

```java
String str = "Hello"; + " World!\n";
System.out.println(str); // Hello World!

/* Decompile */
String str = "Hello World!\n";
System.out.println(str); // Hello World!
```

### **StringBuilder와 StringBuffer**

위에서 두 문자열을 결합할 때 `+` 연산자를 사용하면 내부적으로 `StringBuilder`를 사용하여 문자열을 결합한다고 했다. 그렇다면 `StringBuilder`와 `StringBuffer`는 무엇이고 `String`과 어떤 차이가 있는지 알아보자.

`String` 객체는 레퍼런스 타입이기 때문에 일반적으로 Imuutable한 객체이다. 즉, 한 번 생성된 `String` 객체의 값을 변경할 수 없다는 뜻이다.

```java
String str = "Hello";
str = "World"; // 값이 변경되는 것이 아닌 새로운 객체가 생성되고 str은 새로 생성된 객체를 참조하게 된다.
```

![String_Immutable](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2F0aoee%2Fbtq9lm9Dbig%2FAdcxOsq4dLK2uH74BrVu3K%2Fimg.jpg)

하지만 `StringBuilder`와 `StringBuffer`는 Mutable한 객체이다. 즉, 한 번 생성된 객체의 값을 변경할 수 있다. 값이 변경되면 새로운 객체를 생성하지 않고 기존 객체의 메모리 공간을 변경하여 처리한다.

```java
StringBuilder str = new StringBuilder("Hello");
str = "World"; // 값이 변경된다.
```

![StringBuilder_Mutable](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FPzX74%2Fbtq9uBEzj7c%2FAQVRek6rZMngQeRUOSdBj0%2Fimg.jpg)

`StringBuilder`와 `StringBuffer`의 차이점은 **다중 스레드 환경에서 동기화를 지원**하는지에 대한 여부이다. `StringBuilder`의 경우 동기화를 지원하지 않지만 `StringBuffer`의 경우 동기화를 지원하기 때문에 멀티 스레드 환경에서 사용하기 적합하다.

단일 스레드 환경이라면 `StringBuilder`의 속도가 빠르므로 `StringBuilder`를 사용하는 것이 좋다.

### **문자열 비교**

Java에서 문자열을 비교하는 방법에는 크게 세 가지가 있다.

1. `==` 연산자를 통한 문자열 비교
2. `equals()` 메서드를 통한 문자열 비교
3. `compareTo()` 메서드를 통한 문자열 비교

#### **`==` 연산자를 통한 문자열 비교**

앞에서도 언급했듯이 `==` 연산자는 두 객체의 주소값을 비교한다. 따라서 `==` 연산자를 통해 문자열을 비교하는 것은 두 문자열의 주소값을 비교하는 것과 같다. 

```java
String str1 = "Hello";
String str2 = "Hello";
String str3 = new String("Hello");

System.out.println(str1 == str2); // true
System.out.println(str1 == str3); // false
```

#### **`equals()` 메서드를 통한 문자열 비교**

`equals()` 메서드는 두 객체의 내용을 비교한다. 따라서 `equals()` 메서드를 통해 문자열을 비교하는 것은 두 문자열의 내용을 비교하는 것과 같다. 

```java
String str1 = "Hello";
String str2 = "Hello";
String str3 = new String("Hello");

System.out.println(str1.equals(str2)); // true
System.out.println(str1.equals(str3)); // true
```

`equals()` 메서드는 내부적으로 다음과 같이 문자열 속 문자들을 하나씩 비교한다.

```java
public static boolean equals(byte[] value, byte[] other) {
    if (value.length == other.length) {
        for (int i = 0; i < value.length; i++) {
            if (value[i] != other[i]) {
                return false;
            }
        }
        return true;
    }
    return false;
}
```

#### **`compareTo()` 메서드를 통한 문자열 비교**

`compareTo()` 메서드는 두 문자열을 사전순으로 비교한다. 메서드의 반환값은 0, 음수, 양수의 Integer 중 하나이다.

- 0: 두 문자열이 동일한 경우.
- 양수: 첫 번째 문자열이 두 번째 문자열보다 사전순으로 뒤에 위치하는 경우.
- 음수: 첫 번째 문자열이 두 번째 문자열보다 사전순으로 앞에 위치하는 경우.

```java
String str1 = "Apple";
String str2 = "Banana";
String str3 = "Cherry";

System.out.println(str2.compareTo(str1)); // 1
System.out.println(str2.compareTo(str2)); // 0
System.out.println(str2.compareTo(str3)); // -1
```

`compareTo()` 메서드는 내부적으로 다음과 같이 두 문자열의 길이와 문자들을 하나씩 비교한다.

```java
public static int compareTo(byte[] value, byte[] other) {
    int len1 = value.length;
    int len2 = other.length;
    return compareTo(value, other, len1, len2);
}

public static int compareTo(byte[] value, byte[] other, int len1, int len2) {
    int lim = Math.min(len1, len2);
    for (int k = 0; k < lim; k++) {
        if (value[k] != other[k]) {
            return getChar(value, k) - getChar(other, k);
        }
    }
    return len1 - len2;
}
```