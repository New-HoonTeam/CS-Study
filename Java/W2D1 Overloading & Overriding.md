자바에서는 다형성을 지원하기 위해 **오버로딩**과 **오버라이딩**을 제공한다.

### Overloading

**같은 이름의 메서드** 여러 개를 가지면서 **리턴 타입**과 **매개변수의 유형과 개수**가 다르도록 하는 기법

```java
class Cat {
	void meow() {
		System.out.println("야옹");
	}

	void meow(int repeat) {
		for (int i = 0; i < repeat; i++) {
			System.out.println("야옹");
		}
	}

	void meow(String word) {
		System.out.println(word);
	}
}
```

- **정적 바인딩** (컴파일 시에 중복된 메서드 중 호출되는 메서드를 결정)
- 너무 많이 메서드를 오버로딩할 경우 개발자가 파악하기 어려워질 수도 있다는 단점

### Overriding

상위 클래스가 가지고 있는 메서드를 하위 클래스가 재정의해서 사용

```java
class Parent {
	void greeting() {
		System.out.println("좋은 아침이구나...");
	}
}

class Child extends Parent {
	@Override
	void greeting() {
		System.out.println("안녕히 주무셨습니까");
	}
}
```

- 일반적으로 재정의할 메서드에 **@Override** 어노테이션을 붙인다.
    - 시그니처(리턴 타입, 메서드명, 매개변수 타입과 개수)가 모두 동일하지 않으면 컴파일시 에러 발생
- **동적 바인딩** (실행 시간에 재정의된 메서드를 찾아서 호출)

### 동적 바인딩

Overriding(재정의)된 메서드가 항상 우선적으로 호출된다 !

```java
class DObject {
    public void draw() {
        System.out.println("DObject draw");
    }
}

class Line extends DObject {
    public void draw() {
        System.out.println("Line");
    }
}
```

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/78df97ab-8698-4245-af8c-44372aea2206/Untitled.png](https://p1dgey.notion.site/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2F78df97ab-8698-4245-af8c-44372aea2206%2FUntitled.png?id=2457729b-e5dd-4ab4-b483-d4390a09c3c9&table=block&spaceId=b76551b9-9f24-4a91-9bcd-340caa404f60&width=1380&userId=&cache=v2))

### 동적 바인딩 vs. 정적 바인딩

오버라이딩 관점에서 두 가지 방법의 차이는 아래와 같다.

- 동적 바인딩 (Dynamic Binding)
    - 다형성을 사용하여 메서드를 호출할 때 발생하는 현상
    - 실행 시간(Runtime), 즉 파일을 실행하는 시점에 성격이 결정
    - 실제 참조하는 객체는 서브 클래스이니 서브 클래스의 메서드를 호출
- 정적 바인딩 (Static Binding)
    - 컴파일(Compile) 시간에 성격이 결정
    - 변수의 타입이 슈퍼 클래스이니 슈퍼 클래스의 메서드를 호출

### static 메서드의 오버라이딩 여부는?

정적 메서드는 컴파일 시, 메모리에 올가가고 메서드 영역에 존재한다.
즉, 객체 생성과 관련이 없고 해당 클래스로부터의 모든 인스턴스가 공유한다.
따라서 정적 메서드가 오버라이딩 된다면 논리적으로 맞지 않는다.

→ 불가능하다.

그런데 @Override를 붙이지 않고 실제로 정적 메서드를 재정의하면 컴파일 오류가 발생하지 않는다?

### Hiding (하이딩)

```java
class Animal{
   public static void staticMethod(){
      System.out.println("Animal static method");
   }
   public void instanceMethod(){
      System.out.println("Animal instance method");
   }
}

class Cat extends Animal{
   **// @Override 컴파일 에러** 
   public static void staticMethod(){
      System.out.println("Cat static method");
   }
   @Override public void instanceMethod(){
      System.out.println("Cat instance method");
   }
}
```

```java
public class Main {
   public static void main(String[] args) {
      Animal myAnimal = new Animal();
      Animal myAnimalCat = new Cat();
      Cat myCat = new Cat();

      myAnimal.instanceMethod();      // Animal instance method
      **myAnimalCat.instanceMethod(); // Cat instance method (Overriding)**
      myCat.instanceMethod();         // Cat instance method
   }
}
```

```java
public class Main{
   public static void main(String[] args) {
      Animal myAnimal = new Animal();
      Animal myAnimalCat = new Cat();
      Cat myCat = new Cat();

      myAnimal.staticMethod();      // Animal static method
      **myAnimalCat.staticMethod(); // Animal static method (Hiding)**
      myCat.staticMethod();         // Cat static method
   }
}
```

### Inheritance Rules

만약 상속한 상위 클래스와 구현한 인터페이스에 완전히 똑같은 시그니처가 있다면 무엇을 호출할까?

→ **결합력이 더 높은 메서드가 우선**권을 갖는다 (인터페이스보다 추상 클래스, 클래스가 우선)

아래의 케이스들에 대해서는  [이 블로그](https://loosie.tistory.com/m/709)에 요약이 잘 돼있으니 참고할 것 !

1. 같은 시그니처를 같는 메서드를 보유한 두 개의 인터페이스와 한 개의 클래스를 구현-상속하는 경우
2. 공통의 조상을 상속한 두 개의 인터페이스를 구현하는 경우
3. 같은 시그니처를 같는 default 메서드를 포함한 두 개의 인터페이스를 구현하는 경우
4. 같은 시그니처를 같은 인터페이스와 클래스를 각각 구현, 상속하는 경우

### 참고

[20. 메소드 오버라이딩](https://rap0d.github.io/study/2019/08/29/java_20_overriding/)

[오버로딩과 오버라이딩 차이와 예제](https://private.tistory.com/25)

[[Java] 동적바인딩 vs 정적바인딩](https://woovictory.github.io/2020/07/05/Java-binding/)

[[Java] 메서드 오버라이딩과 하이딩에 대해 (Overriding and Hiding Methods)](https://loosie.tistory.com/m/709)
