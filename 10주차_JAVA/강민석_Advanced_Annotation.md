# 롬복을 만들어보면서 어노테이션이 어떻게? 동작하는지 알아보자.

참고자료: <https://catch-me-java.tistory.com/49> => 원론적인 부분

 <https://catsbi.oopy.io/78cee801-bb9c-44af-ad1f-dffc5a541101>
 > 다른 API를 이용하여 아예 클래스를 컴파일단계에서 새로 만드는 방법을 제시  
 기존에 있는것을 수정이 불가능함

## 🔅 환경구성

- Intellij: 2021.2.1
- Language: Java 8
- ProjectName: customLombok
- 구현내용: @Get(getter), @Set(setter), **@NoArgsConstructor(기본 생성자)**

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
	<modelVersion>4.0.0</modelVersion>
	<parent>
		<groupId>org.springframework.boot</groupId>
		<artifactId>spring-boot-starter-parent</artifactId>
		<version>2.7.8</version>
		<relativePath/> <!-- lookup parent from repository -->
	</parent>
	<groupId>me.kms</groupId>
	<artifactId>customLombok</artifactId>
	<version>0.0.1-SNAPSHOT</version>
	<name>customLombok</name>
	<description>customLombok</description>
	<properties>
		<java.version>1.8</java.version>
		<project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
        <maven.compiler.source>1.8</maven.compiler.source>
        <maven.compiler.target>1.8</maven.compiler.target>
		<auto-service.version>1.0-rc4</auto-service.version>
	</properties>
	<dependencies>
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter</artifactId>
		</dependency>

		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-test</artifactId>
			<scope>test</scope>
		</dependency>

		<dependency>
          <groupId>com.google.auto.service</groupId>
          <artifactId>auto-service</artifactId>
          <version>${auto-service.version}</version>
          <optional>true</optional>
        </dependency>

        <dependency>
          <groupId>com.github.olivergondza</groupId>
          <artifactId>maven-jdk-tools-wrapper</artifactId>
          <version>0.1</version>
        </dependency>
	</dependencies>

	<build>
		<plugins>
			<plugin>
				<groupId>org.springframework.boot</groupId>
				<artifactId>spring-boot-maven-plugin</artifactId>
			</plugin>
		</plugins>
	</build>

</project>

```

- 스프링 부트 3버전은 Java17버전부터 지원하므로 그 아래 단계를 선택



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
@SupportedAnnotationTypes("me.kms.anno.Get")
@AutoService(Processor.class)
public class GetProcessor extends AbstractProcessor {


    @Override
    public SourceVersion getSupportedSourceVersion(){
        return SourceVersion.RELEASE_8;
    }

    @Override
    public boolean process(Set<? extends TypeElement> annotations, RoundEnvironment roundEnv) {
        //logic
        return true;
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

#### **@SupportedSourceVersion(SourceVersion.RELEASE_8)**

- 밑의 `getSupportedSourceVersion()`로 대체가 가능하다. 
- 특정 버전에서의 자바에서만 사용가능하도록 지정할 수가 있다. 위의 예시는 자바8버전에서만 사용가능하다고 명시한 예시다. 
  - 여러 버전을 지정하고 싶다면 `@SupportedSourceVersion(value = {RELEASE_8,RELEASE_11,RELEASE_14})`이런식으로 사용이 가능하다.

#### **@AutoService(Processor.class)**

원래는 `resources / META-INF /javax.annotation.processing.Processor`파일에 다음과 같은 내용을 써주고 컴파일 해야한다.

```text
me.kms.anno.GetProcessor
```

이렇게 작성하고 `mvn clean install`을 수행하면 에러가 발생하는데, 메이븐이 소스 컴파일 하는 시점에 프로세서가 동작하려고 하니, 아직 컴파일 되지 않은 소스를 읽으려 하면서 에러가 나오는 것이다.

그래서 위의 문구를 주석처리하고, 다시 컴파일 하면 된다.

그리고 다시 주석을 풀고, `mvn install`을 수행하면 된다.

이런 일련의 과정이 귀찮기에 `@AutoService`어노테이션을 사용하면 위의 문제를 해결해줄 수 있다. 컴파일 시점에 어노테이션 프로세서를 활용해서 파일을 자동으로 생성해주는 것이다.

![image](https://user-images.githubusercontent.com/30401054/215277667-01cdae6b-abca-4b7d-9f19-409553c8ed87.png)

이제 `process()`에 로직을 구성해줘야한다.

### 1.3 `init()` 구현

```java
    private ProcessingEnvironment processingEnvironment;
    private Trees trees;
    private TreeMaker treeMaker;
    private Names names;
    private Context context;

    @Override
    public synchronized void init(ProcessingEnvironment processingEnv) {
        JavacProcessingEnvironment javacProcessingEnvironment = (JavacProcessingEnvironment) processingEnv;
        super.init(processingEnv);
        this.processingEnvironment = processingEnv;
        this.trees = Trees.instance(processingEnv);
        this.context = javacProcessingEnvironment.getContext();
        this.treeMaker = TreeMaker.instance(context);
        this.names = Names.instance(context);
    }
```

`process()`를 구현하기 전에 필요한 정보들을 설정해야한다. 그에 대한 정보를 맞춰주는 메서드다.

`JavacProcessingEnvironment javacProcessingEnvironment = (JavacProcessingEnvironment) processingEnv;`이 부분 코드 때문에 에러가 발생한다.

![image](https://user-images.githubusercontent.com/30401054/215315151-925c802b-b299-4e6d-a3a4-7d8c44231b29.png)


이러한 에러로, <https://github.com/mplushnikov/lombok-intellij-plugin/issues/988>를 참고하여 해결하면 된다.

- `javacProcessingEnvironment.getContext()`에서 반환하는 컨택스트는 컴파일러 정보, 등을 반환하고 또한 해당 컨택스트를 통해 메서드나 클래스, 어노테이션 프로세서를 만들 수 있게 도와주는 정보를 제공한다.

다른 변수들은 밑에서 어떻게 쓰이는지 확인하면서 찾아가보는게 낫겠다 생각했다.

### 1.4 `process()` 구현


```java

for (final Element element : roundEnv.getElementsAnnotatedWith(Get.class)) {
            System.out.println("element:" + element);
            if(element.getKind() != ElementKind.CLASS){
                processingEnv.getMessager().printMessage(Diagnostic.Kind.ERROR, "@Get annotation cant be used on" + element.getSimpleName());
            }else{
                processingEnv.getMessager().printMessage(Diagnostic.Kind.NOTE, "@Get annotation Processing " + element.getSimpleName());
                final TreePath path = trees.getPath(element);
                scanner.scan(path, path.getCompilationUnit());
            }
        }

```

이 코드는 어렵지는 않다. 저 `for`문은 프로젝트 내에 있는 모든 파일의 클래스를 검사해 `Get`어노테이션이 붙어 있는것만 가져오는 문장이다.

![image](https://user-images.githubusercontent.com/30401054/215317922-3f50d976-f820-44bf-a8b2-56dcc5ba7119.png)

이렇듯 다른 패키지나 다른 곳에 클래스가 있어도

![image](https://user-images.githubusercontent.com/30401054/215317938-d1cd6223-e84c-4b07-8860-a99ad8e10179.png)

컴파일할 때 `@Get`어노테이션이 있으면 가져온다.

가져오고 나서는 `element.getKind() != ElementKind.CLASS`라는 문구를 통해 해당 어노테이션이 **클래스**타입에 붙어있는지 확인하고 맞다면 `else`문을, 아니면 `if`문을 진행하여 에러를 발생시키고 끝내버린다.

내가 `InterfaceLombok`이라는 인터페이스를 만들고, `@Get` 어노테이션을 달았다면 컴파일할 때 에러가 뜰 것이다.

![image](https://user-images.githubusercontent.com/30401054/215318053-0a84448c-8d1a-4b43-afce-f512fd99d899.png)











