# Java Reflection

## ✏️ 개요

- Java 1.1버전에서 나온 `Reflection`은 런타임때 클래스나 객체를 생성, 호출하여 동적으로 객체를 다루는 기술을 말한다.

## ✏️ 나오게 된 배경

Java 프레임워크나 라이브러리 개발시 클래스의 메타데이터를 조작하는데 기능을 제공하기 위해 고안되었다.

즉, 프레임워크에서 제공하는 API를 이용하여 클래스를 호출하고 생성하는 방식이 아닌, **클래스의 이름**을 이용하여 동적으로 클래스를 생성하고 호출할 수 있도록 하여 프레임워크의 사용성을 높이게 한다.

## ✏️ 동적으로 생성시의 이점

`Reflection`을 이용해 객체를 동적으로 다루는 것은 어떤 이점이 있나

- 유연성: 코드 내에서 클래스나 객체를 생성하거나 호출하는 방법을 동적으로 바꿔 유연성을 갖게 한다.

```java
public class MyClass {

    public void testFunction(){
        System.out.println("hello function");
    }
}

@Test
void Reflection_유연성테스트() throws Exception {
    //경로명을 적어줘야한다.
    String className = "MyClass";
    Class<?> clazz = Class.forName(className);
    Object obj = clazz.getDeclaredConstructor().newInstance();
    Method method = clazz.getMethod("testFunction");
    method.invoke(obj);
}
```

대표적으로 `Spring FrameWork`에서 의존성 주입으로 자주 쓰이는 `@Autowired`를 처리하는 부분은 `Reflection`을 사용하고 있다.

이는 `AutowiredAnnotationBeanPostProcessor`라는 클래스로 다음과 같이 구현되어 있다.

```java
protected boolean checkAutowiredAnnotation(AnnotatedElement ae) {
    Autowired autowired = ae.getAnnotation(Autowired.class);
    if (autowired != null) {
        if (Modifier.isStatic(ae.getModifiers())) {
            throw new IllegalStateException("Autowired annotation is not supported on static fields");
        }
        return true;
    }
    else {
        return false;
    }
}
```

`@Autowired`를 탐색할때 `java.lang.reflect.Field`, `java.lang.reflect.Constructor` 클래스를 이용해서 클래스를 사용하고, 해당 필드나 생성자에 값을 주입할 때도 **Reflection**을 사용한다. 

간단하게 인스턴스를 생성해서 해당 인스턴스 필드값을 바꾸는 예제를 보겠다.

```java
public class MyClass {

    private String name;

    public MyClass(String name){
        this.name = name;
    }

    public MyClass() {
    }


    public void testFunction(){
        System.out.println("hello function");
    }

    public String getName() {
        return name;
    }
}

```

- `private` 이라는 필드에 접근하여 해당 값을 바꿔줄 것이다.

```java
    @Test
    void Reflection_Field_search() throws Exception{
        MyClass clazz = new MyClass();
        // clazz.getField("name"); -> public만 접근가능
        Field name = clazz.getClass().getDeclaredField("name");
        name.setAccessible(true);
        name.set(clazz,"kang min seok");
        System.out.println(clazz.getName());
    }
```

- `getField()`는 **public** 필드에만 접근이 가능하다. 
- 때문에 `Field.setAccessible(true)`를 통해 접근제어자를 풀어줘야한다.

![image](https://user-images.githubusercontent.com/30401054/212940465-8e7bd562-9595-413c-a4f6-215f5d42b3c1.png)

성공적으로 값이 바뀌였다.

이를 활용해서 `@Autowired`기능을 구현해보았다.

```java
public class MyClass {
    private String name;

    @Autowired
    private int age;

    @Autowired
    private OtherClass otherClass;
}

public class OtherClass {
}
```

```java
@Test
void Reflection_AutowiredTest() throws Exception{
    MyClass bean = new MyClass();

    Field[] fields = bean.getClass().getDeclaredFields();
    for(Field field: fields){
        System.out.println(field.getType());
        if(field.isAnnotationPresent(Autowired.class)){
            Object dependency = applicationContext.getBean(field.getType());
            field.setAccessible(true);
            field.set(bean,dependency);
        }
    }
    //return bean;
}
```

간단하게 짰지만,

이런식으로 `@Autowired`가 붙은 모든 필드를 탐색하고 의존성 주입을 넣고 해당 빈을 반환해줄 수 있다.

## ✏️ 다른 예시

다른 예시로, Java Reflection을 통해 `ORM`을 구현할 수 있다.

- ORM은 데이터베이스 테이블과 객체를 매핑하는 기술로, 객체의 필드와 데이터베이스 컬럼을 자동으로 매핑할 수 있다.

```java
@Entity
@Builder
public class User {
    @Id
    private int id;
    private String name;
    private int age;
    //getter, setter 생략
}



public class SimpleORM {
    public static void insert(Connection conn, Object object) throws SQLException, IllegalAccessException {
        Class<?> clazz = object.getClass();
        String tableName = clazz.getSimpleName();
        StringBuilder columns = new StringBuilder();
        StringBuilder values = new StringBuilder();

        for (Field field : clazz.getDeclaredFields()) {
            field.setAccessible(true);
            columns.append(field.getName()).append(", ");
            values.append("'").append(field.get(object)).append("', ");
        }

        String sql = "INSERT INTO " + tableName + "(" + columns.substring(0, columns.length() - 2) + ") VALUES(" + values.substring(0, values.length() - 2) + ")";
        PreparedStatement stmt = conn.prepareStatement(sql);
        stmt.execute();
    }
}

```

이 코드는 만약 `SimpleORM.insert(user)`라는 코드를 실행하였을때, `user`에 있는 필드값들을 쿼리에 순차적으로 넣어서 테이블에 저장이 되는 코드이다.

## ✏️ 보안적 이슈?

보면 `private`에도 접근이 가능하고, 다른사람이 만든 클래스에 무작위로 접근해서 값을 바꿀 수도 있을 것 같다.

보안적 이슈가 생길 것 같아서 이를 해결할 수 있는 방법들을 찾아보았다.

- Security Manager: Java에서는 보안 관리자를 사용하여 클래스나 메소드에 대한 접근 권한을 제어할 수 있다.
- Input Validation: 특정 클래스에 대한 Reflection을 허용하는 경우 입력값을 적절하게 검증한다. 잘못된 입력값으로 인해 발생할 수 있는 취약점을 방지할 수 있다.

```java
//Security Manager
import java.lang.reflect.ReflectPermission;

public class MySecurityManager extends SecurityManager {
    @Override
    public void checkPermission(Permission perm) {
        if (perm instanceof ReflectPermission) {
            if (perm.getName().startsWith("suppressAccessChecks")) {
                if (!perm.getName().startsWith("suppressAccessChecks-com.mypackage")) {
                    super.checkPermission(perm);
                }
            } else {
                super.checkPermission(perm);
            }
        }
    }
}

//main Function
MySecurityManager securityManager = new MySecurityManager();
System.setSecurityManager(securityManager);

```

이 코드는 `com.mypackage`패키지에 포함된 클래스만 `Reflection`을 허용하고, 그 외는 금지시키는 코드이다.

인자로 들어오는 `perm`의 속성이 `com.mypackage`로 시작하는지 확인하고 맞다면 권한 체크를 하지 않고 넘어가고, 아니면 `SecurityManage`에 정의된 메서드를 호출하여 권한 체크를 한다.

```java
//Input Validation
String className = "com.example.MyClass";
if(!className.startsWith("com.example")) {
    throw new IllegalArgumentException("Invalid class name");
}
Class<?> clazz = Class.forName(className);
Object obj = clazz.newInstance();
Method method = clazz.getMethod("performFunction");
method.invoke(obj);
```

이렇게 입력값을 검증하여 체크하는 방식도 있다. 이 방식을 사용하여 메서드 인자도 검증하여 **SQL Injection, XSS**등의 취약점을 방지하는데 도움이 될 수 있다니 알아두자.


## 🙌결론

**Java Reflection**은 런타임때 클래스, 인터페이스, 필드, 메서드 등을 동적으로 조작하여 코드의 유연함과 코드를 분리하여 유지보수성과 재사용성이 높아지게 하는 기능이다.

