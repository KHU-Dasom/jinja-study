# **⚡️ 직렬화(Serialize)**

## **직렬화란?**

- 객체(object)를 바이트 스트림으로 변환하는 것과 그 반대(역직렬화: 바이트 스트림 데이터를 다시 객체로 변환하는 것)를 아울러 말한다.

![serialization_deserialization](https://media.geeksforgeeks.org/wp-content/cdn-uploads/gq/2016/01/serialize-deserialize-java.png)

객체를 저장하거나 메모리, 데이터베이스 혹은 파일로 옮기려고 할 때, 서로 다른 시스템 간에 객체를 전송하려고 할 때 사용된다. 시스템적으로는 JVM 메모리(스택 또는 힙)에 상주되어 있는 객체 데이터를 바이트 형태로 변환 및 역변환하는 것을 말한다.
직렬화의 대상은 객체 그 자체, 객체의 값이나 객체의 내용이며 해당 객체가 어떤 클래스인지에 대한 정보는 포함하지 않는다. 즉, 메소드와 같은 클래스의 정보는 직렬화 대상에서 제외된다.

### **마샬링(Marshalling)?**

직렬화와 비슷한 개념으로 마샬링이라는 개념이 있다. 직렬화와 다른 점은 직렬화는 **객체를 바이트 스트림으로 변환**하는 것이고, 마샬링은 어떠한 형태로든 **객체를 변환하는 일련의 과정**을 아울러 말한다. 즉, 직렬화가 마샬링에 포함되는 개념이다.

![marshalling_vs_serialization](https://daeakin.github.io//images/%EC%A7%81%EB%A0%AC%ED%99%94%EB%A7%88%EC%83%AC%EB%A7%81%EA%B5%90%EC%A7%91%ED%95%A9.png)

마샬링의 경우 하나의 객체를 멀리 떨어진 곳과 통신하기 위해 사용되는데, 이 때 해당 객체의 상태와 **코드베이스**를 함께 기록한다. 그 이유는, 마샬링된 객체가 언마샬링될 때 해당 객체의 상태를 복원하기 위해서이다. 이 때, 코드베이스는 객체의 상태를 복원하기 위해 필요한 클래스의 정보를 담고 있다.

## **JAVA의 직렬화**

JAVA에서 직렬화 가능한 객체는 원시 타입(Primitive Type)과 Serializable 인터페이스를 구현하는 객체이다. `ArrayList`의 구현부를 보면,

```java
public class ArrayList<E> extends AbstractList<E>
        implements List<E>, RandomAccess, Cloneable, java.io.Serializable
{
    private static final long serialVersionUID = 8683452581122892189L;
```

`java.io.Serializable`을 구현하고 있음을 알 수 있다. 따라서, `ArrayList`는 직렬화가 가능하다.


### **Serializable 인터페이스**

`java.io.Serializable` 인터페이스는 다음과 같다.

```java
public interface Serializable {
}
```

해당 인터페이스는 아무런 메소드도 정의하지 않는다. `Serializable` 인터페이스는 단지 해당 인터페이스를 구현하는 클래스가 직렬화가 가능하다는 것을 JVM에게 알려주기 위한 것이다. 이러한 인터페이스를 **마커 인터페이스(Marker Interface)**라고 한다. 해당 인터페이스를 구현한 클래스가 특정 능력을 가지고 있다는 것을 표시하기 위해 사용한다.

> 대표적인 마커 인터페이스로는 `Serializable`, `Cloneable`, `EventListener`가 있다.

직렬화 가능한 클래스를 정의할 때 주의해야 할 점은, **모든 필드가 직렬화 가능한 필드**여야 한다는 것이다. 즉, 해당 클래스의 필드에 `Serializable` 인터페이스를 구현하지 않은 객체가 있다면, 해당 클래스는 직렬화가 불가능하다.

### **SerialVersionUID**

앞에서 보여준 `ArrayList`의 구현부를 보면,

```java
public class ArrayList<E> extends AbstractList<E>
        implements List<E>, RandomAccess, Cloneable, java.io.Serializable
{
    private static final long serialVersionUID = 8683452581122892189L;
```

`serialVersionUID`라는 필드가 있다. 이 필드는 직렬화된 객체를 역직렬화할 때, 해당 객체의 클래스가 변경되었는지 확인하기 위해 사용된다. 직렬화된 객체를 역직렬화 할 때 직렬화 당시의 클래스와 같은 클래스를 사용해야 하는데, 이를 판단하기 위한 것이 `serialVersionUID`이다.

`serialVersionUID`는 필수적으로 선언해야 하는 필드가 아닌데, 만약 선언하지 않는다면 컴파일러(JVM)가 자동으로 생성해준다. 이 때, 컴파일러가 생성하는 `serialVersionUID`는 클래스의 구조를 기반으로 생성되기 때문에, 클래스의 구조가 변경되면 `serialVersionUID`도 변경된다. 또한 클래스의 구조가 같더라도 JVM의 종류에 따라 다른 값을 가지게 된다. 따라서, `serialVersionUID`를 직접 선언해주는 것이 좋다.

### **직렬화 방법**

다음은 회원 정보를 담는 클래스이다.

```java
public class User {
    private String username;
    private String password;

    public User(String username, String password) {
        this.username = username;
        this.password = password;
    }

    public String toString() {
        return String.format("username: %s\npassword: %s\n",
                username, password);
    }
}
```

해당 클래스를 직렬화 가능한 클래스로 만들기 위해서는 `Serializable` 인터페이스를 구현하기만 하면 된다.

```java
public class User implements Serializable {
    private String username;
    private String password;

    public User(String username, String password) {
        this.username = username;
        this.password = password;
    }

    public String toString() {
        return String.format("username: %s\npassword: %s\n",
                username, password);
    }
}
```

객체를 생성하고 직렬화해보자. 직렬화는 `ObjectOutputStream`을 사용한다.

```java
@Test
public void User_객체_직렬화() {
    User user = new User("user", "password");
    byte[] serializedUser;
    try (ByteArrayOutputStream baos = new ByteArrayOutputStream()) {
        try (ObjectOutputStream oos = new ObjectOutputStream(baos)) {
            oos.writeObject(user);
            serializedUser = baos.toByteArray();
        }
    } catch (IOException e) {
        throw new RuntimeException(e);
    }
    System.out.println(Base64.getEncoder().encodeToString(serializedUser));
}
```

실행 결과는 다음과 같다.

![serialized_user](https://user-images.githubusercontent.com/39883609/212702775-50514a15-558c-4df6-8def-5bb7027869f7.png)

`User` 객체가 바이트 스트림으로 직렬화 된 것을 볼 수 있다.

이제 직렬화된 바이트 스트림을 역직렬화해보자. 역직렬화는 `ObjectInputStream`을 사용한다.

```java
public void User_객체_역직렬화() {
    byte[] serializedUser = Base64.getDecoder().decode("rO0ABXNyABpvcmcuZXhhbXBsZS5zZXJpYWxpemUuVXNlcgJFATbY1UryAgACTAAIcGFzc3dvcmR0ABJMamF2YS9sYW5nL1N0cmluZztMAAh1c2VybmFtZXEAfgABeHB0AAhwYXNzd29yZHQABHVzZXI");
    try (ByteArrayInputStream bais = new ByteArrayInputStream(serializedUser)) {
        try (ObjectInputStream ois = new ObjectInputStream(bais)) {
            Object objectUser = ois.readObject();
            User user = (User) objectUser;

            System.out.println(user.toString());
        } catch (ClassNotFoundException e) {
            throw new RuntimeException(e);
        }
    } catch (IOException e) {
        throw new RuntimeException(e);
    }
}
```

실행 결과는 다음과 같다.

![deserialized_user](https://user-images.githubusercontent.com/39883609/212703181-339114d2-3987-4c75-b12d-ff21b3137059.png)

바이트 스트림으로 직렬화된 `User` 객체가 역직렬화된 것을 볼 수 있다. 역직렬화의 경우 직렬화된 바이트 스트림은 해당 클래스에 대한 코드베이스를 가지고 있지 않기 때문에 직렬화된 객체의 클래스가 Class Path에 존재해야 하며 import된 상태여야 한다.

### **직렬화 조건을 만족하지 않는 클래스**

직렬화 가능한 클래스는 다음의 조건을 만족해야 한다.

1. `Serializable` 인터페이스를 구현해야 한다.
2. 모든 필드는 직렬화 가능해야 한다.

#### **`Serializable` 인터페이스를 구현하지 않은 경우**

```java
public class User {
    private String username;
    private String password;

    public User(String username, String password) {
        this.username = username;
        this.password = password;
    }

    public String toString() {
        return String.format("username: %s\npassword: %s\n",
                username, password);
    }
}
```

`Serializable` 인터페이스를 구현하지 않은 클래스를 직렬화하려고 하는 경우 `NotSerializableException`이 발생한다.

![NotSerializableException](https://user-images.githubusercontent.com/39883609/212704092-a42c1da1-d37a-48f0-932f-8a75a3a3484c.png)

이는 JVM에서 `Serializable` 인터페이스를 통해 직렬화 가능한 클래스임을 확인하기 때문이다.

#### **직렬화 가능하지 않은 필드를 가진 경우**

```java
public class Profile {
    private String ProfileUrl;
    private String ProfileName;

    public Profile(String profileUrl, String profileName) {
        ProfileUrl = profileUrl;
        ProfileName = profileName;
    }

    public String getProfileName() {
        return ProfileName;
    }
}

public class User implements Serializable {
    private String username;
    private String password;
    private Profile profile;

    public User(String username, String password, Profile profile) {
        this.username = username;
        this.password = password;
        this.profile = profile;
    }

    public String toString() {
        return String.format("username: %s\npassword: %s\nprofileName: %s",
                username, password, profile.getProfileName());
    }
}
```

`Profile` 클래스는 `Serializable` 인터페이스를 구현하지 않았다. 따라서 `User` 클래스의 `profile` 필드는 직렬화 가능하지 않다. 만약 `User` 객체를 직렬화하려고 하면 `NotSerializableException`이 발생한다.

```java
@Test
public void User_객체_직렬화() {
    Profile profile = new Profile("example.org", "example profile");
    User user = new User("user", "password", profile);
    byte[] serializedUser;
    try (ByteArrayOutputStream baos = new ByteArrayOutputStream()) {
        try (ObjectOutputStream oos = new ObjectOutputStream(baos)) {
            oos.writeObject(user);
            serializedUser = baos.toByteArray();
        }
    } catch (IOException e) {
        throw new RuntimeException(e);
    }
    System.out.println(Base64.getEncoder().encodeToString(serializedUser));
}
```

![NotSerializableException_1](https://user-images.githubusercontent.com/39883609/212705802-f8d25635-a729-4941-8063-bfbe944d4114.png)

`Profile` 클래스가 `Serializable` 인터페이스를 구현하도록 수정하면 정상적으로 직렬화가 가능하다.

![Serializable_1](https://user-images.githubusercontent.com/39883609/212706169-ef97da54-5130-4083-bef0-a0c662baa0dd.png)

![Serializable_2](https://user-images.githubusercontent.com/39883609/212706227-c688f144-eab2-463f-b59f-1372ab51b237.png)

### **serialVersionUID에 대해**

위 예제에서는 `serialVersionUID를` 정의하지 않았다. `serialVersionUID`를 정의하지 않은 경우 JVM은 클래스의 구조를 기반으로 자동으로 `serialVersionUID`를 생성한다. 이 때 클래스의 구조란 클래스의 이름, 패키지, 필드, 메서드, 생성자 등을 의미한다. 따라서 클래스의 구조가 변경되면 `serialVersionUID`도 변경된다.

자동으로 생성되는지 한번 확인해보자. 위에서 처음 사용한 `User` 클래스와 `Profile` 클래스 멤버 변수를 추가한 `User` 클래스의 직렬화된 Base64 문자열은 다음과 같다.

```
User: rO0ABXNyABpvcmcuZXhhbXBsZS5zZXJpYWxpemUuVXNlcgJFATbY1UryAgACTAAIcGFzc3dvcmR0ABJMamF2YS9sYW5nL1N0cmluZztMAAh1c2VybmFtZXEAfgABeHB0AAhwYXNzd29yZHQABHVzZXI

User + Profile:
rO0ABXNyABpvcmcuZXhhbXBsZS5zZXJpYWxpemUuVXNlchyoxx1cF/T1AgADTAAIcGFzc3dvcmR0ABJMamF2YS9sYW5nL1N0cmluZztMAAdwcm9maWxldAAfTG9yZy9leGFtcGxlL3NlcmlhbGl6ZS9Qcm9maWxlO0wACHVzZXJuYW1lcQB+AAF4cHQACHBhc3N3b3Jkc3IAHW9yZy5leGFtcGxlLnNlcmlhbGl6ZS5Qcm9maWxlJCIagR5qtUoCAAJMAAtQcm9maWxlTmFtZXEAfgABTAAKUHJvZmlsZVVybHEAfgABeHB0AA9leGFtcGxlIHByb2ZpbGV0AAtleGFtcGxlLm9yZ3QABHVzZXI=
```

처음의 `User` 객체를 수정 후의 `User`로 역직렬화를 시도하면 다음과 같은 에러가 발생한다.

![suid](https://user-images.githubusercontent.com/39883609/212741389-a97ea9ca-c48b-4dd6-9f69-767cd77fbc2e.png)

`serialVersionUID`를 직접 정의하지 않았지만 에러 메세지를 보면 자동으로 생성되어 `serialVersionUID`가 변경되었다는 것을 알 수 있다. 이는 클래스의 구조가 변경되었기 때문이다.

같은 컴파일러에서 직렬화된 클래스라면 클래스의 상태가 동일할 경우 `serialVersionUID`는 동일할 것이다. 하지만 다른 시스템에서 다른 컴파일러를 사용한다면 `serialVersionUID`가 다를 수 있다. 따라서 `serialVersionUID`를 직접 정의하는 것이 좋다.

`serialVersionUID`는 다음과 같이 선언한다.

```java
public class User implements Serializable {
    private static final long serialVersionUID = 1L;

    private String username;
    private String password;
    private Profile profile;

    public User(String username, String password, Profile profile) {
        this.username = username;
        this.password = password;
        this.profile = profile;
    }

    public String toString() {
        
        return String.format("username: %s\npassword: %s\nprofileName: %s",
                username, password, profile.getProfileName());
    }
}
```

위의 `serialVersionUID`는 임의로 정의한 것이며, 통상적으로 중복 가능성이 낮은 난수로 설정한다.

### **transient 키워드**

직렬화는 객체를 바이트 스트림으로 변환하여 외부와 통신하기 위해 사용한다. 하지만 객체의 데이터 중 일부(e.g. 비밀번호)와 같이 보안상의 이유로 직렬화하지 않고 싶은 경우가 있다. 이러한 변수는 직렬화에서 제외해야 하며 이를 위해 `transient` 키워드를 사용한다.

또한 객체의 필드 중 직렬화가 불가능한 필드가 있을 경우 직렬화가 불가능하다. 이러한 필드는 `transient` 키워드를 사용하여 직렬화에서 제외할 수 있다.

```java
public class User implements Serializable {
    private static final long serialVersionUID = 1L;

    private String username;
    private transient String password;
    private Profile profile;

    public User(String username, String password, Profile profile) {
        this.username = username;
        this.password = password;
        this.profile = profile;
    }

    public String toString() {

        return String.format("username: %s\npassword: %s\nprofileName: %s",
                username, password, profile.getProfileName());
    }
}
```

직렬화에서 제외시키고 싶은 필드 에 `transient` 키워드를 붙여주면 직렬화 시 제외된다.

![transient](https://user-images.githubusercontent.com/39883609/212743452-3f25b8e7-3739-4bd0-a9aa-36a5b7f7dc1f.png)

직렬화 후 역직렬화된 결과를 보면 `password` 필드가 제외되었음을 알 수 있다.