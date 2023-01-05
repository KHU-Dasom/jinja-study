# 다형성
프로그램 언어의 각 요소들이 다양한 자료형에 속하는 것이 허가되는 성질
## 오버로딩
같은 이름의 함수를 여러개 정의하고, 매개변수의 유형과 개수를 다르게 하여 다양한 호출에 응답할 수 있도록 한다. [참조](https://github.com/openjdk/jdk/blob/4999f2cb164743ebf4badd3848a862609528dde3/src/java.base/share/classes/java/io/PrintStream.java#L1076-L1103)   
## 오버라이딩
상속을 받은 하위 객체가 상위 객체의 메서드를 다시 재정의한다.  
## 상속의 다형성
자바에서는 일반적으로 부모 클래스의 타입의 참조 변수로 자식 클래스 타입의 인스턴스를 참조할 수 있도록 하여 구현한다.  

```JAVA
Class Parent {...}
Class Child extends Parent {...}


Parent parent = new Child();  // 가능
Child child = new Child();  // 가능
```
# 추상 클래스
자식 클래스에서 반드시 오버라이딩해야지만 사용할 수 있는 추상 메서드를 자식 클래스에서 반드시 구현하게 하기 위한 클래스이다.  
선언부만 작성하게 되고 구현부는 작성하지 않는다.  
```JAVA
abstract class BasePerson {
	abstract String getName();
}

class Tom {
	String getName() { return "TOM"; }
}
```

파이썬의 ABC 클래스와 유사하다.
```python
class BasePerson(ABC):
	@abstractmethod
	def get_name():
		pass

class Tom(BasePerson):
	def get_name():
		return "TOM"
```
# 인터페이스
자바에서는 메서드 출처의 모호성(다이아몬드 문제) 등 여러 문제가 발생할 수 있는 다중 상속을 지원하지 않는다.  
하지만 다중 상속이 필요한 경우가 다수 존재하고, 이것을 위해 인터페이스를 지원한다.  
인터페이스는 클래스를 작성할 때에 기본이 되는 틀을 제공하고, 클래스 간의 매개 역할을 담당한다.  
인터페이스는 오직 추상 메서드와 상수만을 포함할 수 있다.  
```JAVA
interface Person {
	public abstract String getName();
	public static final int maxAge = 100;
}

class Tom implements Person {
	String getName() {
		return "TOM";
	}
}
```
인터페이스는 클래스와 다르게 모든 필드는 public static final이어야 하고 모든 메서드는 public abstract 이어야 한다.
하지만 JAVA8부터 디폴트 메서드(body를 가진 메서드)를 가질 수 있다.
자바에서는 인터페이스를 통해 상속과 구현을 동시에 할 수 있고, 다중 상속을 구현한다.  
[참조](https://github.com/openjdk/jdk/blob/4999f2cb164743ebf4badd3848a862609528dde3/src/java.base/share/classes/java/io/PrintStream.java#L67-L68)  

### 다이아몬드 문제(Diamond Problem)
![다이아몬드문제](data:image/png;base64,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)
D -> B, C -> A인 상속 관계에서 D의 메서드 구현이 B와 C 중 어떠한 것을 가져와야하는지 모호해지는 문제이다.

# 추상 클래스 vs 인터페이스
## 추상 클래스
- 굉장히 밀접하게 관련된 클래스끼리 코드를 공유하는 경우
- 추상 클래스의 자식 클래스들이 public이 아닌 공통된 필드나 메서드를 많이 공유하는 경우
- non-static이나 non-final 필드로 객체 상태를 변경해야하는 경우
## 인터페이스
- 관련이 없는 클래스끼리 관계를 맺을때
- 동작만 지정하고 동작을 누가 구현하는지 중요하지 않을 때
- 다중 상속이 필요할 때
# 디폴트 메서드
JAVA8에 추가된 문법이다.  
Interface에서 메서드 구현이 가능해졌고 인터페이스를 구현하는 클래스는 default method를 상속받는다.  

만약 Interface에서 추상 메서드만 제공한다면, 모든 메서드를 재정의해야하고, 기존 인터페이스에 신규 메서드를 추가하게되었을때 모든 클래스에 해당 함수를 재정의해야한다.  
디폴트 메서드가 존재한다면 인터페이스의 기본 구현을 그대로 상속하여 하위 호환성을 유지할 수 있다.  

하지만, 다이아몬드 문제가 해결되지 않는다.   
따라서 3가지 규칙을 통해서 문제를 해결한다.  
1. 클래스가 항상 이긴다: 인터페이스의 default method보다 항상 클래스의 method가 우선순위를 가진다.   
2. 1번 이외에는 Sub Interface가 이긴다:  A <- B, C <- D 인 상속 관계에서 D의 default 메서드를 B가 재구현한다면 B가 이긴다.  
3. 그 외에는 명시적 호출을 한다.  
```JAVA
public interface A {
	default String getName() { return "A"; }
}
public interface B {
	default String getName() { return "B"; }
}
public class C implements A, B {
	public String getName() { return B.super.getName(); }
}
```
