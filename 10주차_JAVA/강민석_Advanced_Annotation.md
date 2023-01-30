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

### `final TreePath path = trees.getPath(element);`

**TreePath**란 자바 프로그램에서 사용되는 코드, 메서드, 클래스, 변수등을 하나의 트리형태의 자료구조로 관리하는 클래스이다. 

그래서 `getPath()`메소드를 통해 개발자가 원하는 특정 노드의 경로를 찾을 수 있다.

이를 출력하려면 `getPathCoponent()`메서드를 통해 호출 할 수 있다.

```java
//..위에 for문으로 @Get이 달린 모든 요소들을 element에 넣고 있다.
final TreePath path = trees.getPath(element);
System.out.println(path.getCompilationUnit());
//scanner.scan(path, path.getCompilationUnit());
```

실제로 이를 호출해보면

![image](https://user-images.githubusercontent.com/30401054/215502751-de0b27e3-a62a-40dc-997e-4a5c4d6cf750.png)

`@Get`어노테이션이 달린 파일의 소스코드를 모두 가지고 있음을 알 수 있다. 여담으로 컴파일러가 생성자가 없는 경우에는 기본 생성자도 알아서 만들어주고, 한글 문자가 유니코드로 변환됨을 알 수 있다.

### `scanner.scan(path, path.getCompilationUnit());`

scanner는 위에서 변수로 작성되어있는데, 어떻게 작성되어있는지 보겠다.

```java
TreePathScanner<Object, CompilationUnitTree> scanner = new TreePathScanner<Object, CompilationUnitTree>(){
            
            @Override
            public Trees visitClass(ClassTree classTree, CompilationUnitTree unitTree){
                JCTree.JCCompilationUnit compilationUnit = (JCTree.JCCompilationUnit) unitTree;
                // .java 파일인지 확인후 accept 를 통해 treeTransLator, 작성 메소드 생성
                if (compilationUnit.sourcefile.getKind() == JavaFileObject.Kind.SOURCE){
                    compilationUnit.accept(new TreeTranslator() {
                        @Override
                        public void visitClassDef(JCTree.JCClassDecl jcClassDecl) {
                            super.visitClassDef(jcClassDecl);
                            // Class 내부에 정의된 모든 member 를 싹다 가져옴.
                            List<JCTree> members = jcClassDecl.getMembers();
                            // Syntax tree 에서 모든 member 변수 get
                            for(JCTree member : members){
                                if (member instanceof JCTree.JCVariableDecl){
                                    // member 변수에 대한 getter 메서드 생성
                                    List<JCTree.JCMethodDecl> getters = createGetter((JCTree.JCVariableDecl) member);
                                    for(JCTree.JCMethodDecl getter : getters){
                                        jcClassDecl.defs = jcClassDecl.defs.prepend(getter);
                                    }
                                }
                            }
                        }
                    });
                }
                return trees;
            }
        };
```

이 코드를 하나씩 까볼 필요가 있다.

#### `TreePathScanner<Object, CompilationUnitTree>`

이 클래스는 자바 컴파일러API가 제공하는 클래스로, 자바 소스 트리 구조를 순회하면서 개발자의 명령어를 수행할 수 있는 트리구조이다.

각 요소에 대해서, 첫 번째 자료형은 해당 클래스의 **리턴**타입을 말하고, 두 번째 자료형은 **트리의 루트 요소 타입**을 말한다.

즉, 이 클래스는 `Object`형을 반환할 것이고, 트리의 루트 요소 타입은 `ComilationUnitTree`타입이 될 것이다.

그리고 이는 `visitClass()`를 오버라이드 하게 되어있는데 이는

`scanner.scan(path, path.getCompilationUnit());`이 소스에서 호출하고 있다.

디버그를 찍어보면

![image](https://user-images.githubusercontent.com/30401054/215509661-fbc8b052-a5ea-4c87-ad70-56fc6a0a915a.png)

`scan()`메소드의 `accept()`라는 메서드가 있는데, 이 부분을 들어가게 되면

![image](https://user-images.githubusercontent.com/30401054/215510743-b6f589bc-6dfb-4c96-bc27-f174acfde345.png)

오버라이드한 `visitClass()`를 호출하게 되어있다.

`public Trees visitClass(ClassTree classTree, CompilationUnitTree unitTree)`에서 그러면 `classTree`는 무엇이고, `unitTree`는 무엇인가

`classTree`는 패키지, 임포트 문을 제외한 자바 소스코드 그 자체이고, `unitTree`는 패키지, 임포트문을 포함한 자바 소스 코드이다.

```java
//classTree

@Get()
public class Test {
    
    public Test() {
        super();
    }
}

//unitTree
package me.kms.animal;

import me.kms.anno.Get;

@Get()
public class Test {
    
    public Test() {
        super();
    }
}
```

그 밑은 자바 코드인지 확인하고 맞다면 `TreeTranslator`클래스를 만들어서 특정 행동을 수행하게 된다.

```java
// java파일인지 확인
if (compilationUnit.sourcefile.getKind() == JavaFileObject.Kind.SOURCE){
                    compilationUnit.accept(new TreeTranslator() {
                        @Override
                        public void visitClassDef(JCTree.JCClassDecl jcClassDecl) {
                            super.visitClassDef(jcClassDecl);
                            // Class 내부에 정의된 모든 member 를 싹다 가져옴.
                            List<JCTree> members = jcClassDecl.getMembers();
                            // Syntax tree 에서 모든 member 변수 get
                            for(JCTree member : members){
                                if (member instanceof JCTree.JCVariableDecl){
                                    // member 변수에 대한 getter 메서드 생성
                                    List<JCTree.JCMethodDecl> getters = createGetter((JCTree.JCVariableDecl) member);
                                    for(JCTree.JCMethodDecl getter : getters){
                                        jcClassDecl.defs = jcClassDecl.defs.prepend(getter);
                                    }
                                }
                            }
                        }
                    });
                }
```

즉, 우리는 다시 `visitClassDef()`가 언제 호출되는지, 호출 내용에 대해 알 필요가 있다.

- 먼저, `compilationUnit.accept`로 들어갈 필요가 있다.

```java
public void accept(JCTree.Visitor var1) {
    var1.visitTopLevel(this);
}
```


이렇게 구현되어있으며, 이를 또 타고 들어가면

![image](https://user-images.githubusercontent.com/30401054/215526338-863e5e13-cece-4fd2-8f46-81032378e809.png)

`translate()` 함수는 

```java
public <T extends JCTree> List<T> translate(List<T> var1) {
        if (var1 == null) {
            return null;
        } else {
            for(List var2 = var1; var2.nonEmpty(); var2 = var2.tail) {
                var2.head = this.translate((JCTree)var2.head);
            }

            return var1;
        }
    }
```

이렇게 구현되어있다. `var`는 import문과 소스코드를 나눈 스트링을 리스트로 가지고 있다.

```java
//var1[0]
import me.kms.anno.Get;

//var1[1]
@Get()
public class Test {
    
    public Test() {
        super();
    }
}
```

그래서 실질적으로 `var1`을 돌면서 `this.translate((JCTree)var2.head)`이 문장을 수행해준다.

여기서 `head`란 인덱스로 봐도 무방하다. `tail`은 끝의 인덱스를 지칭한다.

그래서 `this.translate()`도 봐야하는데,

```java
public <T extends JCTree> T translate(T var1) {
    if (var1 == null) {
        return null;
    } else {
        var1.accept(this);
        JCTree var2 = this.result;
        this.result = null;
        return var2;
    }
}
```

이렇게 구현이 되어있다.

내부는 좀 더 복잡하지만, 간단하게 `import`로 선언된 경로를 하나하나 파싱해가면서 찾아가는 역할을한다. **me.kms.anno.Get**으로 되어있다면 **me**를 들리고 그다음 **kms**를 들리고 .. 이런 작업을 진행한다.

그래서 `import`문을 이렇게 찾아가고 위의 코드에서 `var1[1]`인 소스코드 부분도 `accept()`함수를 호출하게 처리하게 된다.

이는 좀 더 다르게 동작하는데, 그 이유는 둘의 자료형이 다르기 때문이다.

![image](https://user-images.githubusercontent.com/30401054/215530654-b1246e13-6234-474c-923a-8f0d1808e736.png)

그래서 `var1[1]`은 `JCClassDecl`의 `accept()`를 호출하게 되는데 다음과 같이 구현되어있다.

```java
public void accept(JCTree.Visitor var1) {
    var1.visitClassDef(this);
}
```

여기서 우리가 오버라이드해준 `vistiClassDef()`가 호출이 되는 것이다.

그럼 다시 돌아가야한다.

그래서 일단 오버라이드한 함수 인자로 임포트문을 제외한 소스코드가 있다고 인지하고 시작하자.

```java
public void visitClassDef(JCTree.JCClassDecl jcClassDecl) {
    super.visitClassDef(jcClassDecl);
    // Class 내부에 정의된 모든 member 를 싹다 가져옴.
    List<JCTree> members = jcClassDecl.getMembers();
    // Syntax tree 에서 모든 member 변수 get
    for(JCTree member : members){
        if (member instanceof JCTree.JCVariableDecl){
            // member 변수에 대한 getter 메서드 생성
            List<JCTree.JCMethodDecl> getters = createGetter((JCTree.JCVariableDecl) member);
            for(JCTree.JCMethodDecl getter : getters){
                jcClassDecl.defs = jcClassDecl.defs.prepend(getter);
            }
        }
    }
}
```

내부 구현을 보자

`super.visitClassDef()는 


