# 롬복을 만들어보면서 어노테이션이 어떻게? 동작하는지 알아보자.

참고자료: <https://catch-me-java.tistory.com/49>

## 🔅 환경구성

- Intellij: 2021.2.1
- Language: Java 19
- ProjectName: MyLombok
- 구현내용: @Get(getter), @Set(setter), **@NoArgsConstructor(기본 생성자)**

```ruby
plugins {
    id 'java'
}

group 'org.example'
version '1.0-SNAPSHOT'

repositories {
    mavenCentral()
}

dependencies {
    implementation 'com.google.auto.service:auto-service'
    testImplementation 'org.junit.jupiter:junit-jupiter-api:5.7.0'
    testRuntimeOnly 'org.junit.jupiter:junit-jupiter-engine:5.7.0'
}

test {
    useJUnitPlatform()
}
```


## 🔅1. Annotation Get

### 1.1 @Get 코드

```java
package mylombok;

import java.lang.annotation.*;
/**
 * @Target의 ElementType.TYPE에 의해 클래스/인터페이스/열거/레코드 타입에 어노테이션을 붙일 수 있게 되었다.
 * @Get의 기본방식은 컴파일 이후에는 쓰이지 않게 구성되어 있으므로, RentetionPolicy를 SOURCE로 해둬서 컴파일 이전까지 쓰이게 한다.
 *
 * @author kms
 * */
@Documented
@Target(ElementType.TYPE)
@Retention(RetentionPolicy.SOURCE)
public @interface Get {
}
```

### 1.2 Get 어노테이션 프로세스

```java
@SupportedAnnotationTypes("mylombok.Get")
@SupportedSourceVersion(SourceVersion.RELEASE_17)
@AutoService(Processor.class)
public class GetProcessor extends AbstractProcessor {
    @Override
    public boolean process(Set<? extends TypeElement> annotations, RoundEnvironment roundEnv) {
        return false;
    }
}
```

#### **@SupportedAnnotationTypes**

- 인자로 들어온 값의 경로를 찾고 해당 어노테이션의 값을 사용하여, 어떠한 **처리**를 할 수 있게 도와준다.
  - 이는 `Processor`라는 인터페이스에 정의된 `getSupportedAnnotationTypes`라는 메서드를 통해 인자로 들어온 어노테이션의 이름을 반환하여 작업을 할 수 있게 도와준다.
  - 우리가 extends한 `AbstractProcessor` 클래스는 위의 `Processor`를 구현한 구현체이며, 우리가 필요한것들만 오버라이드 해서 사용할 것이다.


- **Annotation Processor**를 지원하는 어노테이션 인터페이스를 나타내는 어노테이션임을 뜻한다.
  - Annotation Processor란 어노테이션을 처리하는 일종의 프로그램이다.
- 이를통해 Annotation Processor는 특정 어노테이션 인터페이스를 사용하는 클래스를 검색하여 처리할 수 있게 된다.

즉, 코드에서는 `mylombok`패키지에 있는 `Get`어노테이션을 처리하도록 도와주는 소스이다.






