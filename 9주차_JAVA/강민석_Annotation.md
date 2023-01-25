# Annotation 

## 📃배경

어노테이션의 의미는 ‘주석’이다. 주석은 코드로만 알기 힘든 내용이나 코드로 설명하기 어려운 디테일한 부분을 설명하기 위해 추가하기도 한다.

어노테이션은 그렇기에 소스코드 안에 다른 프로그램을 위한 정보를 미리 약속된 형식으로 포함시키기 위해 작성한다.

기존의 자바 웹 어플리케이션은 구성과 설정값들을 외부 XML설정 파일에 명시하여 프로그래밍 되었다. 외부에서 변경될 수 있는 값을 관리하기에 재컴파일 없이 쉽게 변경사항을 적용할 수 있었지만, 프로그램의 규모가 커질수록 설정을 다 적어줘야했고, 프로그램을 작성할때 마다 많은 설정을 작성해야했기에 문제점이 있었다. 이를 해결하기 위해 고안된 문법이 어노테이션이다. 어노테이션은 런타임 또는 컴파일에 어노테이션은 해석될 수 있다. 어노테이션은 보통 문서화, 컴파일러 체크, 코드 분석을 위한 용도로 사용되는데 본질적인 목적은 소스 코드에 메타데이터를 표현하는 것이다.

소스 코드에 메타데이터를 표현하다는 것은 정보에 대한 정보를 표시한다는 의미인데..

@Override라는 어노테이션은 보통 많이 접해봤으니까 이를 통해 설명을 하자면 `@Override` 어노테이션의 기능은 뒤에서도 설명하겠지만 명시적으로 이 함수가 오버라이딩 됨을 알려주고 오버라이딩을 하지 않으면 빨간 밑줄이 생기면서 컴파일러에서 경고를 해준다. 이것은 ‘이 함수는 오버라이딩 해야해!’라는 정보를 포함하고 있기에 함수(정보)에 대한 정보를 표시한다는 것이다.

```java
// Annotation을 사용한방식
@Service
public class MyService {
    // ...
}

//xml을 사용한 방식
<bean id="myService" class="com.example.MyService">
   <!-- ... -->
</bean>

```

## 📃종류

어노테이션은 Java에 기본적으로 내장되어 있는 것들도 있는데, 이를 Built-in-Annotation이라고 한다. 주로 컴파일러를 위한 것으로 컴파일러에 유용한 정보를 준다.

대표적으로

### Built-in-Annotation

**@Override**

위에서 설명했지만 컴파일러에게 현재 메소드가 수퍼클래스의 메소드를 오버라이드한 메소드라는 것을 명시한다.

이를 통해 메소드 이름 오타나 오버라이딩을 하지 않으면 컴파일러가 잡아준다.

**@Deprecated**

차후 버전에 지원되지 않을 수 있기 때문에 더 이상 사용되지 말아야 할 메소드를 말한다.

예로는 최근 스프링에서 웹설정을 위한 어댑터에 @Deprecated가 달리게 되었다.

```java
@Deprecated
public abstract class WebSecurityConfigurerAdapter implements WebSecurityConfigurer<WebSecurity> {
    ...
}
```

**@SupreesWarning**

프로그래머의 의도를 컴파일러에게 전달하여 경고를 제거한다.

회사에서 쓴걸 본 적이 있는데 이걸 지우면 노란줄(warning)이 뜨던것이 이 어노테이션을 달면 사라지는것을 알 수 있었다.

**@SafeVarargs**

@SupressWarning과 비슷한데, 변수나 타입에 대한 경고성도 무시한다는 의미다.

**@FunctionalInterface**

컴파일러에게 해당 인터페이스가 함수형 인터페이스라는 것을 알린다. @Override와 비슷하게 프로그래머가 실수를 하는것을 미연에 방지한다. 자바의 람다식은 함수형 인터페이스로만 접근이 가능하기 때문이다.


**@Native**

Java 8버전 이후의 새로운 어노테이션이다.

이는 필드에만 적용할 수 있고 native code영역과 관련이 있다는것을 명시적으로 알려준다.

```java
public final class Integer {
    @Native public static final int MIN_VALUE = 0x80000000;
    // omitted
}
```

네이티브 영역은 외부 언어로 작성된 파일을 저장하는 메모리 영역인데 보통 C언어로 작성되어 있기에 이를 명시하는 의미로 쓰인다. Native 메서드에서 참조되는 상수 앞에 붙인다고 한다.

### Meta-Annotation

어노테이션에 사용되는 어노테이션으로 해당 어노테이션의 동작대상을 결정한다. 주로 새로운 어노테이션을 정의할 때 사용한다.

**@Target**

어노테이션이 적용가능한 대상을 지정하는데 사용한다. {}를 통해 열거할 수도 있다. 패키지, 어노테이션, 멤버 변수 등에 따라 지정할 수 있다.

**@Retention**

어노테이션이 유지되는 기간을 지정한다. 세 가지 유지정책(retention policy)를 사용할 수 있다.

SOURCE : 소스코드(.java)까지 남아있는다.
CLASS : 클래스 파일(.class)까지만 남아있는다. = 바이트 코드
RUNTIME : 런타임까지만 남아있는다.
이렇게만 설명하면 이해가 되지 않는다.

주의!! 레퍼런스가 있긴하지만 제 생각을 곁든 개인적인 견해입니다.

먼저 JVM이 동작하는 방식을 알아야할 필요가 있다.

```text
      compile              load
.java    ->     .class      ->      classLoader in JVM
```

이렇게 되기에 SOURCE는 .java시점에 살아있고, CLASS는 .class시점에 살아있을것이고 실제 프로그램이 구동되면서는 런타임이 된것이므로 RUNTIME에 살아있을 것이다.

대표적으로 롬복의 @Getter는 정책이 SOURCE로 되어있는데 컴파일될 때 메서드들의 get()함수를 바이트코드로 생성해 컴파일을 시켜버리고 사라지게 되는것이다.

@NonNull의 정책은 CLASS인데 동작방식은 이게 붙어있는데 null을 넣게되면 노란색 경고로 프로그래머에게 알려주는 동작을 한다.
Maven/Gradle로 다운받은 라이브러리와 같이 jar파일에는 소스가 포함되어 있지 않기에 SOURCE정책을 사용하는데에 한계가 있다. 이미 컴파일된 파일이기에 어노테이션 정보가 남아있지 않기 때문이다. 그렇기에 .class만 남아있는 라이브러리 경우에 대하여 타입체커, IDE부가기능 등을 사용할 수 있으려면 CLASS에 대한 정책이 필요하게 되는 것이다.

RUNTIME정책에 대해서는 자바의 Reflection API등을 통하여 런타임에 어노테이션 정보를 알 수 있다는 의미이다. @Controller, @Service와 같이 스프링이 올라오는 런타임 시점에 스캔이 컴포넌트 스캔이 가능해야하기에 RUNTIME정책을 필요로 한다.

**@Documented**

어노테이션에 대한 정보가 javadoc로 작성한 문서에 포함되도록 할 때 사용하는 어노테이션이다.

**@Inherited**

어노테이션이 자손 클래스에도 상속되도록 하는 어노테이션이다. 즉, 조상 클래스에 붙이면 자손 클래스도 이 어노테이션이 붙은것과 같이 인식된다.

## 📃 커스텀 어노테이션 만들기

커스텀을 해주기 전에 어느상황에서 쓰일 수 있는지 알아야한다.

나는 스프링 시큐리티에서 제공하는 기능들을 사용했을때, 

```java
@GetMapping
public UserResponse currentUser(@AuthenticationPrincipal UserAuth userAuth) {
    return userService.getCurrentUser(userAuth);
}
```

라는 코드를 테스트하고 싶었다. 하지만 테스트 코드에서는 `@AuthenticationPrincipal`로 들어와야하는 인자처리를 어떻게 할까?

크게 2가지 방법이 있지만 커스텀하기 쉬워서 대표적으로 쓰이는 방법 한가지를 쓰겠다.

`User`객체는 `Id`, `email`,`username`의 필드를 가진다고 친다.

먼저 이를 해결하기 위해서는 유저정보를 스프링 시큐리티 컨텍스트에 넣고 이를 테스트할때 꺼내서 쓰거나 그래야할 것이다.

이 과정을 테스트 코드 거치기 전에 해줘야할 것인데 어떻게 해줄까?

가장 원론적인 방법은 다음과 같다.

```java
UserAuth userAuth = UserAuth.builder()
                            .username("kms")
                            .email("test@gmail.com")
                            .build();
Authentication auth = new UsernamePasswordAuthenticationToken(userAuth, "", null);
SecurityContextHolder.getContext().setAuthentication(auth);
```

이런식으로 `UserAuth` 인스턴스를 만들어서 시큐리티 인증정보 컨택스트에 넣어주면 된다.

이 방식은 매 테스트코드마다 넣는것도 번거롭고, 필요한곳에만 가져다 쓰기에도 불편하다.

테스트코드위에 어노테이션을 달고, 그 부분에만 설정값을 컨택스트에 넣고 쓰면 좋을것 같다.

스프링 시큐리티에서는 `WithSecurityContextFactory`라는 인터페이스를 제공한다.

이 인터페이스를 사용하면 사용자 인증정보를 컨택스트에 넣을 수 있게 **강제**하는 인터페이스로, 어노테이션 타입과 하나의 메소드를 가진다. 그 메소드는 컨택스트 객체를 생성해야만 한다.

```java
public interface WithSecurityContextFactory<A extends Annotation> {
    SecurityContext createSecurityContext(A annotation);
}
```

구현해야할 인자로 어노테이션 타입이 들어와야한다. 해당 어노테이션을 가지고 스프링 시큐리티 컨택스트에 유저정보 값을 넣어줄 것이다.

그러면 해당 어노테이션을 만들어야한다.

```java
@Retention(RetentionPolicy.RUNTIME)
@WithSecurityContext(factory = WithAuthUserSecurityContextFactory.class)
public @interface WithAuthUser {
    long id() default 1L;
    String email() default "test@gmail.com";
    String username() default "kms";
}
```

- 커스텀 어노테이션을 만들기 위해서는 `@interface`를 붙여야한다. 
- `@Retention()`을 통해 해당 어노테이션이 런타임때까지 남아있어야한다고 한다.
- `@WithSecurityContext()`라는 어노테이션은 우리가 만들 `WithSecurityContextFactory`구현체 클래스 정보를 넣으면 된다. 즉, `WithAuthUserSecurityContextFactory`는 `WithSecurityContextFactory`를 구현한 구현체이다.

이제 구현체를 만들어보겠다.
```java
public class WithAuthUserSecurityContextFactory implements WithSecurityContextFactory<WithAuthUser> {

    @Override
    public SecurityContext createSecurityContext(WithAuthUser annotation) {
        Long id = annotation.id();
        String email =  annotation.email();
        String username = annotation.username();

        UserAuth userAuth = UserAuth.builder()
                .email(email)
                .username(username)
                .id(id)
                .build();
        Authentication authentication = new UsernamePasswordAuthenticationToken(userAuth,"",null);
        SecurityContext context = SecurityContextHolder.getContext();
        context.setAuthentication(authentication);
        return context;

    }

}
```

인터페이스에서 어노테이션 타입을 받았으므로 우리가 만든 어노테이션을 인자로 넣어주고, 어노테이션에서 값을 꺼내서 컨택스트에 넣는 클래스로 볼 수 있다.

정리하자면 `WithSecurityContextFactory`는 스프링 시큐리티에서 제공하는 컨택스트에 유저정보를 넣고, 그 컨택스트를 반환하는 메소드를 구현하는 메소드를 강제하는 인터페이스이다.

이를 구현한 것이 `WithAuthUserSecurityContextFactory`이고 **어노테이션**을 인자로 받게 되어있고, 그 어노테이션은 개발자가 만든 커스텀 어노테이션이 되겠다.

그럼 어떤 방식으로 이 컨택스트를 테스트때마다 가져다 쓴다는 것인가?

그러면 `@WithSecurityContext(factory = WithAuthUserSecurityContextFactory.class)`의 의미는 무엇일까?

`@WithSecurityContext()`라는 어노테이션은 다음과 같이 구현되어 있다.

```java
@Target({ElementType.ANNOTATION_TYPE})
@Retention(RetentionPolicy.RUNTIME)
@Inherited
@Documented
public @interface WithSecurityContext {
    Class<? extends WithSecurityContextFactory<? extends Annotation>> factory();

    TestExecutionEvent setupBefore() default TestExecutionEvent.TEST_METHOD;
}
```

위에서부터 해석을 하자면

- `@Target({ElementType.ANNOTATION_TYPE})`: 어노테이션에 붙을 수 있다.
- `@Retention(RetentionPolicy.RUNTIME)`: 런타임때까지 살아있다. 이를 통해 `Refelction`을 사용할 수 있다.
- `@Inherited`: 상속을 가능하게 한다.
- `@Documented`: 문서화가 되어있다.

- `factory()`메서드는 구현된 `WithSecurityContextFactory`를 리턴하도록 되어있다.
- `setupBefore()`는 기본값으로 `TestExecutionEvent.TEST_METHOD`를 가지도록 되어있다. 이 뜻은 테스트 메서드가 수행되기 전에 어노테이션을 실행한다는 뜻이다.


즉, 테스트 메서드가 실행되기 전에 `factory()`메서드에 반환된 컨택스트의 설정 및 초기화를 실행하고 해당 설정값을 가지고 테스트 메서드의 인증 과정을 수행한다.


## 📃 해석

아무런 정보가 없는데 해석을 어떻게 한 것인가? 사실 어노테이션의 내부 메서드 구현방식은 보기는 굉장히 까다롭다 IDE에서도 타고갈 수도 없기에 이를 해석하기 위해서는..

공식문서롤 보거나 여러 커뮤니티를 돌아다니면서 알아보거나 직접 여러 상황을 테스트코드를 작성해서 유추하거나 하는 등의 방법밖에 없다고 나는 알고 있다.

더 찾아봐도, 코드 단에서 내부 구현방식을 정확히 알기 위해서는 본인이 직접 유추하는 수 밖에 없다는 말이다.

<img width="1698" alt="image" src="https://user-images.githubusercontent.com/30401054/214232944-d6acee78-f797-44ef-ba59-fb4ead65c773.png">

왜 그런식으로 만들었을까? **chatGPT**의 말을 요약하자면 다음과 같이 정리할 수 있다.

우리가 인터페이스를 사용하는 이유와 비슷한데, 내부 구현은 외부 사용자에게 보여주지 않도로 함으로써, 캡슐화와 추상화를 보장하고 사용자 입장에서는 내부 구현방식을 몰라도 사용만 할 수 있게 만든다.

사실 `Reflection`을 통해서 어노테이션 속성값들을 찾고 그 내부구현을 유추할 수 있기도 한다.






