# **`Comparable`과 `Comparator`**

자바에서 두 변수 또는 객체를 비교하기 위해서는 `Comparable`과 `Comparator` 인터페이스를 구현해야 한다. `Comparable` 인터페이스와 `Comparator` 인터페이스의 차이점이 무엇이고 어떻게 사용하는지 알아보자.

## **`Comparable`**

`Comparable` 인터페이스의 구현부를 보면,

```java
public interface Comparable<T> {
    public int compareTo(T o);
}
```

내부에 `compareTo(T o)`라는 하나의 메서드를 가지며, `Comparable` 인터페이스를 구현한 클래스에서 해당 메서드를 오버라이딩하게 되면 해당 클래스의 객체 간 비교가 가능하다.

일반적으로 원시 타입의 실수 변수들은 부등호 또는 등호로 비교가 가능하다.

```java
int a = 1;
int b = 2;

System.out.println(a < b); //true
```

하지만 객체의 경우에는 당연하게도 등호 또는 부등호 연산자를 사용하여 비교할 수 없다.

```java
Car carA = new Car();
Car carB = new Car();

System.out.println(carA < carB); //error
```

만약 객체 자체를 비교해야 할 경우, `Comparable` 인터페이스를 구현하여 `compareTo(T o)` 메서드를 오버라이딩하여 비교 로직을 작성해줄 수 있다.

만약 상품 ID와 상품 이름을 가진 자동차 클래스가 있을 때,

```java
public class Car {
    private int productId;
    private String productName;

    public Car(int productId, String productName) {
        this.productId = productId;
        this.productName = productName;
    }
}
```

해당 `Car` 클래스의 객체를 상품 ID를 기준으로 비교하고 싶다면, `Comparable` 인터페이스를 구현해주면 된다.

```java
public class Car implements Comparable<Car> {
    private int productId;
    private String productName;

    public Car(int productId, String productName) {
        this.productId = productId;
        this.productName = productName;
    }

    @Override
    public int compareTo(Car o) {
        return 0;
    }
}
```

`compareTo()` 메서드에 비교 로직을 작성해주면 되는데, 반환값은 어떻게 해야할까? 공식 문서에서는 다음과 같이 설명하고 있다.

> Returns:
a negative integer, zero, or a positive integer as this object is less than, equal to, or greater than the specified object.
>
> 반환값: 0보다 작은 정수, 0, 0보다 큰 정수 중 하나를 반환한다. 이는 이 객체가 지정된 객체보다 작거나 같거나 크다는 것을 의미한다.

즉, `compareTo()` 메서드의 반환값은 다음과 같은 규칙을 가진다.

- 비교하는 객체가 매개변수로 넘어온 객체보다 작을 경우: 음수를 반환
- 비교하는 객체가 매개변수로 넘어온 객체와 같을 경우: 0을 반환
- 비교하는 객체가 매개변수로 넘어온 객체보다 클 경우: 양수를 반환

위 규칙을 기반으로 `compareTo()` 메서드를 작성해보자.

```java
public class Car implements Comparable<Car> {
    private int productId;
    private String productName;

    public Car(int productId, String productName) {
        this.productId = productId;
        this.productName = productName;
    }

    @Override
    public int compareTo(Car o) {
        if (this.productId < o.productId) {
            return -1;
        } else if (this.productId == o.productId) {
            return 0;
        } else {
            return 1;
        }
    }
}
```

이제 `Car` 클래스의 객체를 상품 ID를 기준으로 비교할 수 있다. 두 객체를 비교하기 위해서는 `compareTo()` 메서드를 호출하면 된다.

```java
@Test
public void compareTest() {
    Car carA = new Car(123, "carA");
    Car carB = new Car(234, "carB");

    System.out.println(carA.compareTo(carB));
}
```

비교하려는 객체 `carA`의 상품 ID가 비교 대상인 `carB`의 상품 ID보다 작으므로, `-1`을 반환한다.

![compareTest](https://user-images.githubusercontent.com/39883609/215426879-91fa0b2f-2621-45e6-9f17-d8c0791542da.png)

## **`Comparator`**

`Comparable` 인터페이스는 객체가 해당 인터페이스를 구현함으로써 객체 자신과 파라미터로 들어온 다른 객체를 비교할 수 있게 해준다. 그럼 `Comparator` 인터페이스는 무엇일까?

`Comparator` 인터페이스의 구현부를 보자.

```java
@FunctionalInterface
public interface Comparator<T> {
    int compare(T o1, T o2);

    boolean equals(Object obj);

    default Comparator<T> reversed() {
        return Collections.reverseOrder(this);
    }

    /* ... */
}
```

`Comparator` 인터페이스는 `Comparable` 인터페이스와 다르게 `compare()` 메서드를 포함한 여러 메서드들을 가지고 있는 것을 볼 수 있다. 또한 함수형 인터페이스로 선언되어 있는 것을 볼 수 있다. 또한 `compare(T o1, T o2)` 메서드가 2개의 인자를 받는 것을 볼 수 있다.

`Comparator` 인터페이스는 `Comparable`과 유사하게 사용할 수도 있는데,

```java
public class Car implements Comparator<Car> {
    private int productId;
    private String productName;

    public Car(int productId, String productName) {
        this.productId = productId;
        this.productName = productName;
    }

    @Override
    public int compare(Car o1, Car o2) {
        if (o1.productId < o2.productId) {
            return -1;
        } else if (o1.productId == o2.productId) {
            return 0;
        } else {
            return 1;
        }
    }
}
```

기본적으로 `compareTo()` 메서드와 작동 방식은 같지만 `compare()` 메서드의 경우 두개의 인자를 받아 이를 비교한다. 즉 `Comparator()` 인터페이스를 구현하여 비교 로직을 작성해주게 되면 다음과 같은 문제점이 발생한다.

```java
@Test
public void comparatorTest() {
    Car carA = new Car(123, "carA");
    Car carB = new Car(234, "carB");
    Car carC = new Car(345, "carC");

    System.out.println(carA.compareTo(carA, carB));
    System.out.println(carA.compareTo(carB, carC));
    System.out.println(carC.compareTo(carC, carA));
}
```

위 코드를 보면 `compare()` 메서드를 호출하는 객체에 상관없이 비교가 가능하다, 즉 일관성이 떨어진다. 또한 비교를 위한 객체를 새로 하나 만들게 되면 해당 객체의 내부 값들은 필요가 없는데도 생성이 되어버린다는 문제점이 있다.

사실 Java에서는 `Comparator` 인터페이스를 익명 클래스 혹은 람다식 형태로 사용하기를 권장하고 있다. 그렇다면 `Comparator` 인터페이스를 익명 클래스 혹은 람다식 형태로 사용하는 방법을 알아보자.

> 익명 클래스와 람다식에 대한 간략한 설명은 다음 링크를 참조해주세요:
>
> [JAVA 8 - 함수형 인터페이스와 익명 클래스, 람다식](https://github.com/KHU-Dasom/jinja-study/blob/main/06%EC%A3%BC%EC%B0%A8_JAVA/%EA%B0%95%EB%AF%BC%EC%84%9D_JAVA8.md#%EF%B8%8F-1-functional-interface-lambda-%EC%B6%94%EA%B0%80)

```java
public class Car {
    private int productId;
    private String productName;

    public Car(int productId, String productName) {
        this.productId = productId;
        this.productName = productName;
    }
}

Car carA = new Car(123, "carA");
Car carB = new Car(234, "carB");
Car carC = new Car(345, "carC");

// 익명 클래스 사용
@Test
public void compareTest() {
    Comparator<Car> comparator = new Comparator<Car>() {
        @Override
        public int compare(Car o1, Car o2) {
            if (o1.getProductId() < o2.getProductId()) {
                return -1;
            } else if (o1.getProductId() == o2.getProductId()) {
                return 0;
            } else {
                return 1;
            }
        }
    };

    System.out.println(comparator.compare(carA, carB));
    System.out.println(comparator.compare(carB, carC));
    System.out.println(comparator.compare(carC, carA));
}

//람다식 사용

@Test
public void compareTestWithLambda() {
    Comparator<Car> comparator = (Car o1, Car o2) -> {
        if (o1.getProductId() < o2.getProductId()) {
            return -1;
        } else if (o1.getProductId() == o2.getProductId()) {
            return 0;
        } else {
            return 1;
        }
    };

    System.out.println(comparator.compare(carA, carB));
    System.out.println(comparator.compare(carB, carC));
    System.out.println(comparator.compare(carC, carA));
}
```

위와 같이 `Comparator` 인터페이스를 익명 클래스 또는 람다식 형태로 사용하게 되면 `compare()` 메서드를 호출하는 객체에 상관없이 일관성 있게 비교가 가능하다. 또한 클래스 내부에 `Comparator`를 구현해줄 필요가 없다.

또 하나의 장점은 `Comparator`를 자유롭게 생성할 수 있다는 점이다. 클래스 내부에 비교 로직을 작성하는 경우 클래스의 특정 멤버 변수 하나로만 비교할 수 있었지만 `Comparator`를 사용할 경우 원하는 비교 로직을 갯수에 상관 없이 작성할 수 있다. 만약 자동차 상품 ID가 아닌 상품 이름을 기준으로 비교하고 싶다면 다음과 같이 작성할 수 있다.

```java
Car carA = new Car(123, "carA");
Car carB = new Car(234, "carB");
Car carC = new Car(345, "carC");

// 익명 클래스 사용
@Test
public void compareTest() {
    Comparator<Car> comparator = new Comparator<Car>() {
        @Override
        public int compare(Car o1, Car o2) {
            return o1.getProductName()
                .compareTo(o2.getProductName());
        }
    };

    System.out.println(comparator.compare(carA, carB));
    System.out.println(comparator.compare(carB, carC));
    System.out.println(comparator.compare(carC, carA));
}
```

상품 이름을 비교할 때 `compareTo()` 메서드를 사용했는데, 이는 `String` 클래스가 `Comparable` 인터페이스의 `compareTo()` 메서드를 구현해주었기 때문이다.

```java
public int compareTo(String anotherString) {
    byte v1[] = value;
    byte v2[] = anotherString.value;
    if (coder() == anotherString.coder()) {
        return isLatin1() ? StringLatin1.compareTo(v1, v2)
                            : StringUTF16.compareTo(v1, v2);
    }
    return isLatin1() ? StringLatin1.compareToUTF16(v1, v2)
                        : StringUTF16.compareToLatin1(v1, v2);
    }
```

## **Comparator 인터페이스와 정렬(Sorting)**

앞에서 `Comparable`과 `Comparator` 인터페이스를 사용하여 객체를 비교하는 방법을 알아보았다. 왜 해당 인터페이스들이 정렬에 사용되는지 알아보자.

Java에서는 `Collections.sort()` 또는 `Arrays.sort()` 메서드를 사용하여 배열을 정렬할 수 있다. Java의 정렬 알고리즘은 기본적으로 Merge Sort와 Tim Sort 기반으로 이루어지는데, Tim Sort 역시 Insertion Sort와 Merge Sort 기반이기 때문에 내부적으로 배열의 원소들을 비교하여 정렬한다. 즉, 정렬을 위해선 두 원소 간의 비교가 필요한 것이다.

```java
private static <T> void binarySort(T[] a, int lo, int hi, int start,
                                       Comparator<? super T> c) {
        assert lo <= start && start <= hi;
        if (start == lo)
            start++;
        for ( ; start < hi; start++) {
            T pivot = a[start];
            int left = lo;
            int right = start;
            assert left <= right;

            while (left < right) {
                int mid = (left + right) >>> 1;
                if (c.compare(pivot, a[mid]) < 0)
                    right = mid;
                else
                    left = mid + 1;
            }
            assert left == right;

            int n = start - left;
            switch (n) {
                case 2:  a[left + 2] = a[left + 1];
                case 1:  a[left + 1] = a[left];
                         break;
                default: System.arraycopy(a, left, a, left + 1, n);
            }
            a[left] = pivot;
        }
    }
```

`Comparator`의 `compare()` 메서드를 사용하여 두 원소 간의 대소 비교를 한 후, 음수일 경우 두 원소의 순서를 바꾸고, 양수일 경우 그대로 두는 방식으로 정렬을 한다.

즉, 정렬 알고리즘에서 두 원소 간의 비교를 할 때, 원시 타입의 경우 비교 연산자를 사용하여 비교가 가능하지만, 객체의 경우 `Comparator`를 사용하여 비교 로직을 넣어 줘야 이를 사용하여 대소 비교를 한다는 것이다.

그렇기 때문에 `Collections.sort()`와 `Arrays.sort()` 메서드에서 `Comparator`를 받는 것을 볼 수 있다.

```java
// Collections.sort()
public static <T> void sort(List<T> list, Comparator<? super T> c) {
    list.sort(c);
}

// Arrays.sort()
public static <T> void sort(T[] a, Comparator<? super T> c) {
    if (c == null) {
        sort(a);
    } else {
        if (LegacyMergeSort.userRequested)
            legacyMergeSort(a, c);
        else
            TimSort.sort(a, 0, a.length, c, null, 0, 0);
    }
}
```

만약 클래스 내부에 `Comparable` 인터페이스와 `compareTo()` 메서드를 구현해주었다면 해당 비교 로직을 기준으로 정렬 알고리즘이 수행된다.

```java
public class Car implements Comparator<Car> {
    private int productId;
    private String productName;

    public Car(int productId, String productName) {
        this.productId = productId;
        this.productName = productName;
    }

    @Override
    public int compare(Car o1, Car o2) {
        return o1.productName.compareTo(o2.productName);
    }
}

@Test
public void compareTestWithAscendingSort() {
    List<Car> carList = new ArrayList<>(List.of(
            new Car(234, "carB"),
            new Car(123, "carA"),
            new Car(345, "carC")
    ));
    Collections.sort(carList);

    for (Car car :
            carList) {
        System.out.println(car);
    }
}
```

Java의 기본 정렬은 오름차순이므로 자동차 상품 이름을 기준으로 오름차순으로 정렬될 것이다.

![compareTestWithAscending](https://user-images.githubusercontent.com/39883609/215737378-fa96af1b-c688-4733-b1cf-7c76bda87994.png)

만약 `Comparable` 인터페이스를 구현해주지 않았다면 익명 클래스 또는 람다식 형태로 `Comparator`를 구현하여 사용할 수 있다.

```java
@Test
public void compareTestWithAscendingSort() {
    List<Car> carList = new ArrayList<>(List.of(
            new Car(234, "carB"),
            new Car(123, "carA"),
            new Car(345, "carC")
    ));

    // 익명 클래스 사용
    Collections.sort(carList, new Comparator<Car>() {
        @Override
        public int compare(Car o1, Car o2) {
            return o1.getProductName().compareTo(o2.getProductName());
        }
    });

    // 람다식 사용
    Collections.sort(carList, ((o1, o2) -> {
        return o1.getProductName().compareTo(o2.getProductName());
    }));

    // Method Reference 사용
    Collections.sort(carList, (Comparator.comparing(Car::getProductName)));

    for (Car car :
            carList) {
        System.out.println(car);
    }
}
```

결과는 `Comparable` 인터페이스를 구현하여 사용한 것과 동일할 것이다.

`Comparable`과 `Comparator`를 사용하여 비교 로직을 작성해주었을 경우 비교가 불가능한 클래스로 이루어진 컬렉션을 정렬할 수 있다는 것을 알았다. 하지만 로직을 직접 작성해주기 때문에 원하는 방식으로 정렬할 수도 있다.

통상적으로 컬렉션을 역순으로 정렬할 때 어떻게 할까?

```java
@Test
public void sortingReverse() {
    List<Integer> integerList = new ArrayList<>(List.of(
            1, 2, 3, 4, 5
    ));

    integerList.sort(Collections.reverseOrder());
}
```

`Collections.reverseOrder()`를 사용하면 된다. `reverseOrder()`의 구현부를 보면

```java
public static <T> Comparator<T> reverseOrder() {
    return (Comparator<T>) ReverseComparator.REVERSE_ORDER;
}
```

`Comparator` 객체를 반환하는 것을 볼 수 있다. 즉, `Comparator`를 자신이 원하는 정렬 방식으로 구현하여 사용할 수 있다는 것이다.

예를 들어, 자동차 상품 이름을 기준으로 내림차순으로 정렬하고 싶다면

```java
@Test
public void compareTestWithAscendingSort() {
    List<Car> carList = new ArrayList<>(List.of(
            new Car(234, "carB"),
            new Car(123, "carA"),
            new Car(345, "carC")
    ));

    // 익명 클래스 사용
    Collections.sort(carList, new Comparator<Car>() {
        @Override
        public int compare(Car o1, Car o2) {
            return o2.getProductName().compareTo(o1.getProductName());
        }
    });
}
```

`compare()` 메서드의 반환값이 음수일 때 두 원소의 위치가 바뀌기 때문에 `o2`와 `o1`의 순서를 바꿔주면 된다.

이를 응용하여 더욱 복잡한 조건을 가진 정렬이 가능하다.
만약, 문자열 배열이 있고 이를 길이 순으로 오름차순 정렬하되 길이가 같은 문자열은 사전 역순으로 정렬하고 싶다면

```java
@Test
public void sortingAscendingReversedIfSameSize() {
    List<String> stringList = new ArrayList<>(List.of(
            "abc",
            "ab",
            "abcd",
            "acb",
            "abdc",
            "a",
            "b"
    ));

    // 익명 클래스 사용
    Collections.sort(stringList, new Comparator<String>() {
        @Override
        public int compare(String o1, String o2) {
            if (o1.length() == o2.length()) {
                return o2.compareTo(o1);
            }
            return o1.length() - o2.length();
        }
    });

    for (String string :
            stringList) {
        System.out.println(string);
    }
}
```

위와 같이 원하는 비교 로직을 작성하여 정렬 알고리즘을 구현할 수 있다. 결과는 다음과 같다.

![sortingAscendingReversedIfSameSize](https://user-images.githubusercontent.com/39883609/215742700-f32bb226-6c7f-4245-8089-936911c21308.png)