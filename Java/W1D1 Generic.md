# Generic?

JDK 1.5부터 도입된 개념

**제네릭(Generic)은** 타입을 마치 함수 파라미터 처럼 사용하여  
**클래스 내부에서 지정하는 것이 아닌 외부에서 사용자에 의해 지정되도록 하는 것**

제네릭(Generic)은 '일반적인'이라는 뜻
'특정(Specific) 타입 데이터 형식에 의존하지 않고, 
여러 다른 데이터 타입들을 가질 수 있도록 하는 방법’

```java
public class ClassName <T, K> { ... }
public Interface InterfaceName <T, K> { ... }
public class HashMap <K, V> { ... }
```

# **Generic(제네릭)의 장점**

**1.** 타입 안정성 : 제네릭을 사용하면 잘못된 타입이 들어올 수 있는 것을 컴파일 단계에서 방지할 수 있다.

**2.** 간결한 코드 작성 : 클래스 외부에서 타입을 지정해주기 때문에 따로 타입을 체크하고 변환해줄 필요가 없다. 

**3.** 재사용성 : 비슷한 기능을 지원하는 경우 코드의 재사용성이 높아진다.

# 제네릭 타입 컨벤션
![제네릭타입컨벤션](https://user-images.githubusercontent.com/86050295/206861235-6364b8f8-ce5f-445c-aec5-6992d4e3ea5c.png)


  ```java
List<E>
public class HashMap<K, V> { . . . }
```

참조 타입만 가능, primitive type(int, double, char, boolean) 안된다!

[자바 [JAVA] - 제네릭(Generic)의 이해](https://st-lab.tistory.com/153)


# 제네릭 클래스 / 인터페이스

타입 파라미터를 가지는 클래스와 인터페이스

```java
class ClassName<E> {

	private E element;	// 제네릭 타입 변수

	void set(E element) {	// 제네릭 파라미터 메소드
		this.element = element;
	}

	E get() {	// 제네릭 타입 반환 메소드
		return element;
}
```

```java
class Main {
public static void main(String[] args) {

	ClassName<String> a = new ClassName<String>();
	ClassName<Integer> b = new ClassName<Integer>();

	a.set("10");
	b.set(10);

	System.out.println("a data : " + a.get());
	// 반환된 변수의 타입 출력
	System.out.println("a E Type : " + a.get().getClass().getName());

	System.out.println();
	System.out.println("b data : " + b.get());
	// 반환된 변수의 타입 출력
	System.out.println("b E Type : " + b.get().getClass().getName());
}
```

![Untitled](https://user-images.githubusercontent.com/86050295/206861419-04a92d68-0529-4fa2-ab11-a648fa6117a6.png)

```java
// 제네릭 클래스 
class ClassName<K, V> {	
	private K first;	// K 타입(제네릭)
	private V second;	// V 타입(제네릭)
	
	void set(K first, V second) {
		this.first = first;
		this.second = second;
	}
	
	K getFirst() {
		return first;
	}
	
	V getSecond() {
		return second;
	}
}
```

```java
class Main {
	public static void main(String[] args) {
		
		ClassName<String, Integer> a = new ClassName<String, Integer>();
		
		a.set("10", 10);

		System.out.println("  fisrt data : " + a.getFirst());
		// 반환된 변수의 타입 출력
		System.out.println("  K Type : " + a.getFirst().getClass().getName());
		
		System.out.println("  second data : " + a.getSecond());
		// 반환된 변수의 타입 출력
		System.out.println("  V Type : " + a.getSecond().getClass().getName());
	}
}
```

# 제네릭 메서드
![제네릭 메스드](https://user-images.githubusercontent.com/86050295/206861481-3cc062dc-6499-440b-9470-ca51409af05d.png)

- 리턴 타입 앞에 제네릭 타입 명시
- 제네릭 클래스가 아니더라도 사용가능

```java
public class Student<T> {
    static T getName(T name) {   
        return name;
    }
}
```

먼저 메소드를 static 으로 선언하면 제네릭을 사용 할 수 없다. 

⇒ Student가 인스턴스화 되기 전에 static method가 메모리에 올라가는데 
이 때 타입 T가 결정되지 않았기 때문에 사용할 수 없는 것

```java
public class Student<T> {
    static <T> T getName(T name) {   
        return name;
    }
}
```

```java
public class Student<T> {
    static <T> void getName(T name) {   
        ...
        return ;
    }
}
```

# 제네릭 클래스 상속

```java
class Coffee<T> {
	protected T taste;
}

```

- 클래스명 : Coffee <T>
- 메소드명 : protected T taste : 커피의 맛 변수

```java
interface Food<W> {
	public W getWeight();
}
```

- 인터페이스명 : `Food<W>`
- 메소드명 : `public W getWeight()` - 음식 무게 반환 메서드

제네릭 타입도 부모-자식간 상속이 가능하다.

그러나 **상속관계에서 자식 제네릭 타입은 
부모의 제네릭 타입 파라미터를 반드시 선언**해야함을 주의해야 한다.

□  **부모 클래스를 상속 받는경우**

```java
public class 클래스명<부모 제네릭 타입, 자식 제네릭 타입> extends 부모 클래스{...}
```

□ **부모 인터페이스를 상속 받는경우**

```java
public class 클래스명<부모 제네릭 타입, 자식 제네릭 타입> implements 부모 인터페이스{...}
```

□ **부모 클래스, 인터페이스를 동시에 상속 받는경우**

아래와 같이 커피(Coffee) 클래스와 푸드(Food) 인터페이스를 상속받는 
아이스아메리카노 제네릭 클래스를 선언해주었습니다.

- 부모 제네릭 클래스 : Coffee<T>
- 부모 제네릭 인터페이스 : Food<W>
- 자식 제네릭 클래스 : IceAmericano<T,W,C>

자식 제네릭 클래스는 부모 제네릭 타입인 T, W 는 필수로 선언해주어야 하며, 
자식 타입 파라미터인 C를 추가한 코드

```java
public class IceAmericano<T, W, C> extends Coffee<T> implements Food<W> {
  public W weight;

  @Override
  public W getFoodWeight() {
    return weight;
  }

  public T getCoffeeTaste(T taste) {
    return taste;
  }

  public C isCold(C cold) {
    return cold;
  }
}
```

# 제한된 제네릭 클래스

```java
<T extends A> // A와 A를 상속한 자손 타입들만 허용 - 하한경계
<T super B> // B와 B의 부모 타입들만 허용 - 상한경
```

## 와일드 카드

```java
<?> == <? extends Object>
```

```java
/*
 * Number와 이를 상속하는 Integer, Short, Double, Long 등의
 * 타입이 지정될 수 있으며, 객체 혹은 메소드를 호출 할 경우 K는
 * 지정된 타입으로 변환이 된다.
 */
<K extends Number>
 
 
/*
 * Number와 이를 상속하는 Integer, Short, Double, Long 등의
 * 타입이 지정될 수 있으며, 객체 혹은 메소드를 호출 할 경우 지정 되는 타입이 없어
 * 타입 참조를 할 수는 없다.
 */
<? extends T>	// T와 T의 자손 타입만 가능
```

# 참고

[자바 제네릭(Generics) 기초](https://tecoble.techcourse.co.kr/post/2020-11-09-generics-basic/)

[코딩교육 티씨피스쿨](http://www.tcpschool.com/java/java_generic_concept)

[[JAVA] 자바 제네릭(Generic) 기본 및 활용](https://life-with-coding.tistory.com/489)

[[Java] 제네릭(Generic) 사용법 & 예제 총정리](https://coding-factory.tistory.com/573)

[1. Java 자바 제네릭 - 제네릭 (Generic) 타입](https://kephilab.tistory.com/111)

[29편. 제네릭(Generic)](https://blog.hexabrain.net/379)

[[Java] 자바 타입 제네릭(Generic) 쉽게 알아보기](https://seeminglyjs.tistory.com/184)

[[Java] 자바에서 Generic 이란 무엇일까?](https://devlog-wjdrbs96.tistory.com/338)

[[Java] 제네릭](https://ksabs.tistory.com/205)

[자바 [JAVA] - 제네릭(Generic)의 이해](https://st-lab.tistory.com/153)
