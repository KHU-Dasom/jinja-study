# Generic
데이터 타입을 generalize 한다.  
여러 타입을 받을 수 있는 클래스나 메소드의 인자 및 반환값을 컴파일 시에 미리 지정하는 방법이다.  
JDK 5.0 이전에는 제너릭이 존재하지 않아 반환값을 Object로 두었고 타입 검사나 변환을 해야 했지만 제너릭을 통하여 컴파일 시에 미리 타입을 정할 수 있다.  
컴파일 시에 모든 제너릭 타입이 제거되고 컴파일된 파일에는 제너릭 타입이 남아있지 않는다.  

```Java
// Generic 사용 클래스
class GenericNode<T> {  
    T data;  
    void setData(T data) { this.data = data; }  
    T getData() { return this.data; }  
}  
// Object 사용 제너릭 클래스
class GenericObjectNode {  
    Object data;  
    void setData(Object data) { this.data = data; }  
    Object getData() { return this.data; }  
}

GenericNode<Integer> intNode = new GenericNode<>();  
intNode.setData(5);  
System.out.println(intNode.getData().getClass());
// class java.lang.Integer

GenericObjectNode intObjectNode = new GenericObjectNode();  
intObjectNode.setData(5);  
System.out.println(intObjectNode.getData().getClass());  
// class java.lang.Integer
```

E: Element
K: Key
N: Number
T: Type
V: Value
로 주로 사용하고, 가능하면 한글자 캐릭터를 사용하기를 권장한다. [참조](https://docs.oracle.com/javase/tutorial/extra/generics/simple.html)  

# 서브타이핑
```Java
class Parent {}  // 1
class Child extends Parent {}  //2

List<Child> childList = new List<>();  // 3
List<Parent> parentList = childList;  // 4
```

위에서 4번째 라인은 컴파일 에러가 발생한다.
Child가 Parent의 subtype이라고 해서 `List<Child>`가 `List<Parent>`의 subtype은 아니기 때문이다.  
`List<Child>`가 저장된 영역은 다른 Child 객체가 저장되어있는 Heap 영역이다. 만약 `List<Parent>`가 허용이 된다면 이후 Child 영역에 다른 타입이 들어올 수 있어 타입 안정성이 깨지게된다.  
이것을 방지하기 위해 `List<Child>`가 `List<Parent>`의 subtype이 아니게 된다.   

# 와일드 카드
`List<Child>`가 `List<Parent>`의 subtype이 아니게 되면서 
```Java
static void printCollection(List<Object> c) {  
    for (Object e : c) {  
        System.out.println(e);  
    }  
}

List<Integer> intList = new ArrayList<>();
// intList가 List<Object> 타입이 아니라 컴파일 에러
printCollection(intList);
```
제너릭한 리스트를 인자로 받을 수 없게 되었다.
이것을 처리하기 위하여 wildcard type이 등장하게 된다.
```Java
static void printCollection(List<?> c) {  
    for (Object e : c) {  
        System.out.println(e);  
    }  
}

List<Integer> intList = new ArrayList<>();
// intList가 List<Object> 타입이 아니라 컴파일 에러
printCollection(intList);
```
이 때 위의 `List<?>`를 collection of unknown이라고 부른다.
```Java
List<?> anonymousList = new ArrayList<>();  
anonymousList.add(3); // 컴파일 에러
```
단, 위와 같은 구문으로 사용할 수는 없다. wildcard type으로 선언된 컬렉션은 데이터를 추가하는데에 사용할 수 없다.  
# Bounded Wildcards
```Java
class Animal {  
    String getType() { return "Animal"; }  
}  
  
class Dog extends Animal {  
    String getType() { return "Dog"; }  
}
```
만약 위와 같은 상속 구조를 갖는 Dog과 Animal이 있다고 하자.
Animal만 순회하면서 getType 메서드를 호출하고 싶을때에 wildcard type에 type bound를 정의해 줄 수 있다.
```Java
static void printCollection(List<? extends Animal> animals) {  
    for (Animal animal : animals) {  
        animal.getType();  
    }  
}

List<Animal> animalList = new ArrayList<>();  
animalList.add(new Animal());  
animalList.add(new Dog());  
animalList.add(new Cat());  
  
printCollection(animalList);
```

