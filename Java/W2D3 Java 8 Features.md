# 람다 표현식

- 메서드를 하나의 식으로 표현하는 것이다
- 람다식으로 표현하면 메서드의 이름이 없어지므로, 
람다식을 ‘익명 함수(anonymous function)’라고도 한다.
- 자바 클래스를 만들고, 객체를 생성해야 메서드를 호출할 수 있었던 과거에 비해 간결해졌다.
- 람다식은 매개변수로 전달되고, 메서드의 결과로 반환될 수도 있다.

## 작성법

```java
int max (int a, int b) {
	return a > b ? a : b;
}
```

리턴 타입과 메서드이름 생략

```java
(int a, int b) -> { return a > b ? a : b; }
```

1문장이라면 괄호와 리턴 생략 가능

```java
(int a, int b) -> a > b ? a : b 
```

매개변수 타입 생략

```java
(a, b) -> a > b ? a : b // 매개변수의 타입은 추론이 가능한 경우 생략 가능
```

# 함수형 인터페이스

- 추상 메서드를 단 하나만을 가진 인터페이스 
(static method와 default method에 대한 제약은 없다)

람다식과 인터페이스의 메서드가 1:1로 연결될 수 있다

- `@FunctionalInterface`를 붙이면, 컴파일러가 
함수형 인터페이스를 올바르게 정의하였는지 확인해준다.

# Stream API

군집의 데이터를 저장하기 위해서 배열이나 컬렉션을 사용합니다.

또한, 이렇게 저장된 데이터에 접근하기 위해서는 
반복문이나 반복자(iterator)를 사용하여 매번 코드를 작성해야 했다.

⇒ 이렇게 작성된 코드는 길이가 너무 길고 가독성도 떨어지며, 코드의 재사용이 떨어진다.

스트림은 데이터 소스를 추상화하고, 데이터를 다루는데 자주 사용되는 메서드들을 정의해 놓았다.

⇒ 데이터 소스가 무엇이든 같은 방식으로 다룰 수 있게되어 코드의 재사용형을 높였다.

## 스트림은 데이터 소스를 변경하지 않는다.

스트림은 데이터 소스로 부터 데이터를 읽기만할 뿐, 데이터 소스를 변경하지 않는다

필요하다면 결과를 컬렉션이나 배열에 담아서 반환할 수 있다.

## 스트림은 일회용이다

Iterator로 컬렉션의 요소를 모두 읽고 나면 다시 사용할 수 없는 것 처럼, 스트림도 한번 사용하면 닫혀서 다시 사용할 수 없다. 필요하면 스트림을 다시 생성해야 한다.

```java
strStream1.sorted().forEach(System.out::println);
int numOfStr = strStream1.count(); // 에러
```

## 스트림은 작업을 내부 반복으로 처리한다.

반복문을 메서드의 내부에 숨길 수 있게 되어 간결한 코드 작성을 하게 해준다.
매개변수에 대입된 람다식을 데이터 소스의 모든 요소에 적용한다.

```java
void forEach
```

```java
for (String str : strList) }
		System.out.println(str);
}
```

```java
stream.forEach(System.out::println);
```

```java
void forEach(Consumer<? super T> action) {
	Objects.requireNonNull(action); // 매개변수의 널 체크
	for (T t : src) { // 내부 반복
		action.accept(T);
	}
}
```

# 인터페이스 default method & static method 추가

- 인터페이스에서 추상 메서드외에도 default, static method를 정의 할 수 있게 되었다.

```java
public interface InterfaceExample {

    void testMethod();

    default String hi(String name) {
        return "Hello " + name;
    }

    static int add(int a, int b) {
        return a + b;
    }

}
```

```java
public class InterfaceExampleImp implements InterfaceExample {
    @Override
    public void testMethod() {
        System.out.println("it's test method");
    }
}
```

```java
public class Main {
    public static void main(String[] args) {
        InterfaceExample interfaceExample = new InterfaceExampleImp();

        interfaceExample.testMethod();
        System.out.println(interfaceExample.hi("Jake"));
        System.out.println(InterfaceExample.add(1, 2));

    }
}
```

# Optional

자바 8이전에는 NPE 발생 가능성으로 인해 null 체크를 위한 
많은 보일러플레이트 코드를 추가해야만 했다. 

⇒ null 대신 Optional을 사용함으로 NPE 발생을 방지하고,
Optional에 정의된 메서드를 통해 간결한 코드를 작성할 수 있게 되었다.

# 날짜 관련 `java.time` 클래스 추가

JDK 1.0에서는 Date 클래스를 사용하여 날짜에 관한 처리를 수행했습니다.

하지만 Date 클래스는 현재 대부분의 메소드가 사용을 권장하지 않고(deprecated) 있습니다.

JDK 1.1부터 새롭게 제공된 Calendar 클래스 문제점

1. Calendar 인스턴스는 불변 객체(immutable object)가 아니다.
2. 윤초(leap second)와 같은 특별한 상황을 고려하지 않는다.
3. Calendar 클래스에서는 월(month)을 0부터 11까지로 표현해야 하는 불편함이 있다.

`java.time` 패키지는 위와 같은 문제점을 모두 해결했으며, 
다양한 기능을 지원하는 다수의 하위 패키지를 포함하고 있습니다.

# `String` `join()` 메소드

```java
import java.util.StringJoiner;

public class Main {
    public static void main(String[] args) {
        String[] nums = {"1", "2", "3"};
        System.out.println(String.join("+", nums)); // 1+2+3
    }
}
```

# `StringJoiner`

```java
import java.util.StringJoiner;

public class Main {
    public static void main(String[] args) {
        StringJoiner stringJoiner = new StringJoiner(",", "접두사/", "/접미사");
        stringJoiner.add("하나");
        stringJoiner.add("둘");
        stringJoiner.add("셋");

        System.out.println(stringJoiner); // 접두사/하나,둘,셋/접미사
    }
}

```

[Java 8에 추가된 것들](https://medium.com/@inhyuck/java-8%EC%97%90-%EC%B6%94%EA%B0%80%EB%90%9C-%EA%B2%83%EB%93%A4-8c66023cbbae)

[[Java] Java 8 에서 추가된 기능 사용해보기](https://gogomalibu.tistory.com/97)

[JAVA 8 새로 추가된 기능! 개발자 면접 대비](https://developsd.tistory.com/113)

[1. Java 8의 새로운 기능](https://mslim8803.tistory.com/36)

[Java 8에 추가된 것은?](https://velog.io/@jsj3282/Java-8%EC%97%90-%EC%B6%94%EA%B0%80%EB%90%9C-%EA%B2%83%EC%9D%80)

[코딩교육 티씨피스쿨](http://www.tcpschool.com/java/java_intro_java8)

[[Java] Java8 버전에서 추가된 기능들 Stream API, 람다표현식, 동작 파라미터화, 병렬처리](https://ksr930.tistory.com/223)

[Date, Calendar API의 문제점(자바 날짜,시간 API)](https://hanbi97.tistory.com/237)
