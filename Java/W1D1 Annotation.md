## 1. Java Annotation

자바에서의 어노테이션은 주석 또는 **메타 데이터**라고 한다.
**컴파일**이나 **런타임**시에 애플리케이션이 처리해야 할 **추가적인 정보**를 알려주기 위해 사용한다.
Heap 메모리 영역의 **metaspace**에 저장된다.
자바 어노테이션의 문법은 **골뱅이(@)** 기호를 앞에 붙여서 사용하며 **JDK 1.5** 이상에부터 지원한다.

- 코드 작성 문법 에러를 체크하도록 정보 제공 (컴파일)
- 빌드나 배포시 코드를 자동으로 생성할 수 있도록 정보 제공
- 특정 기능을 실행하도록 정보 제공 (런타임)

자바에서 기본으로 제공하는 빌트인, 메타 어노테이션들도 있고 커스텀 어노테이션을 정의해서 사용할 수 있다.

```java
@Target({ElementType.METHOD})
@Retention(RetentionPolicy.RUNTIME)
public @interface MyAnnotation {
	String value() default "MyAnnotation : default value"
```

## 2. Built-in Annotation

### @Override

메소드를 오버라이드(재정의) 한다는 정보를 제공한다.
상속받은 부모 클래스 또는 구현체에서 해당 메소드가 없다면 **컴파일 오류**가 발생한다.

### @Deprecated

메서드 자체를 없애진 않고 더 이상 사용하지 말 것을 알린다.
이 어노테이션이 붙은 메서드를 애플리케이션에서 사용하는 경우 **컴파일 경고**가 발생한다.

### @SuppressWarnings

컴파일러에 경고를 출력하지 않도록 알린다.
개발자가 경고 상황을 알고 의도적으로 작업 중인데 불필요하게 경고를 띄우면 중요한 로그의 가시성이 떨어지기 때문에 사용한다. 경고를 억제하는 수준을 다르게 가져갈 수 있으며 아래의 인자를 넣으면 된다.

all / cast / dep-ann / deprecation / fallthrough / finally / null / rawtypes / unchecked / unused

### @SafeVarargs

제네릭 같은 가변인자 매개변수를 사용할 때 **경고를 무시**하도록 한다.
안전하지 않은 곳에서는 사용하지 않는 것을 권장한다.

### @FunctionalInterface

인터페이스의 Default, Static 메서드를 제외한 메서드가 두 개 이상이라면, 즉 함수형 인터페이스에 적합하지 않다면 **컴파일 오류**를 발생시켜준다.

## 3. Meta Annotation

Custom Annotation을 만들 때 사용하는 메타 어노테이션도 있다.

### @Retention

어노테이션의 리텐션(유지) 기간을 명명한다.

- RetentionPolicy.Class
    - 바이트 코드 파일까지 어노테이션 정보 유지
    - 리플렉션을 이용해 어노테이션 정보를 얻을 순 없음
- RetentionPolicy.Runtime (대부분 사용하는 옵션)
    - 바이트 코드 파일까지 어노테이션 정보 유지
    - 리플렉션을 이용해 런타임에 어노테이션 정보를 가져올 수 있음
- RetentionPolicy.Source
    - 어노테이션 정보는 컴파일 이후에 삭제

### @Document

자바 문서에도 어노테이션 정보가 표현된다.

### @Target

생성한 어노테이션이 적용될 수 있는 위치를 나열한다.

```java
@Target({ElementType.***METHOD***, ElementType.***FIELD***})
```

- ElementType.TYPE
    - 클래스, 인터페이스, ENUM
- ElementType.ANNOTATION_TYPE
- ElementType.FIELD
- ElementType.CONSTRUCTOR
- ElementType.METHOD
- ElementType.LOCAL_VARIABLE
- ElementType.PACKAGE

### @Inherited

자식 클래스가 어노테이션을 상속 받을 수 있도록 하는 옵션이다. (= 상속의 전파)

```java
@Inherited
@interface SuperAnno {
}

@SuperAnno
class Parent {
}

// <- 여기에 @SuperAnno 가 붙은 것으로 인식
class Child extends Parent {
}
```

### @Repeatable

반복적으로 어노테이션을 선언할 수 있도록 한다.

## 부록. Reflection을 통해 어노테이션 정보를 가져오자!

```java
@Target({ElementType.METHOD})
@Retention(RetentionPolicy.RUNTIME)
public @interface MyAnnotation {
        String value() default "MyAnnotation : default value"
}
```

```java
class MyObject {
    @MyAnnotation
    public void testMethod1() {
        System.out.println("This is testMethod1");
	  }

    @MyAnnotation(value = "My new Annotation")
    public void testMethod2() {
        System.out.println("This is testMethod2");
		}
}
```

```java
public class MyMain {
	public static void main(String[] args) {
		Method[] methodList = MyObject.class.getMethods();
		
		for (Method method : methodList) {
			if(method.**isAnnotationPresent**(MyAnnotation.class)) {	
				MyAnnotation annotation = method.**getDeclaredAnnotation**(MyAnnotation.class);
		
				String value = annotation.value();
				System.out.println(method.getName() + ":" + value);
			}
		}
	}
}
```

## 부록2. 명명 패턴보다 어노테이션을 사용하라

전통적으로 도구나 프레임워크가 특별히 다뤄야 할 프로그램 요소에는 딱 구분되는 명명 패턴을 적용해왔다.

JUnit 3까지는 테스트 메서드 이름을 test로 시작하게끔 했다.
이 때문에 개발자가 실수로 test가 아닌 tset으로 입력해서 오타가 날 경우에는 JUnit이 무시하고 지나치기 때문에 개발자가 해당 테스트는 정상적으로 통과했다고 판단할 수 있는 여지가 있었다.
또한 테스트가 예외를 발생시키는 것을 테스트하려고 메서드에 예외를 표현하면 가시성이 떨어질 뿐 아니라 특정 문자열이 예외인지를 JUnit이 이해할 수 있는 메서드 명으로 표현하기 매우 까다로웠다.

어노테이션은 이 문제들을 해결해주는 개념으로 JUnit 4부터 도입했다.
@Test를 통해 테스트 메서드 임을 알리고 여러 어노테이션을 활용해서 다양하게 테스트할 수 있도록 지원한다.

```java
...

@Retention(RetentionPolicy.RUNTIME)
@Target(ElementType.METHOD)
public @interface Test {
}
```

```java
...

@Retention(RetentionPolicy.RUNTIME)
@Target(ElementType.METHOD)
public @interface ExceptionTest {
	Class<? extends Throwable>[] value();
}
```

## 참고

- [https://hbase.tistory.com/169](https://hbase.tistory.com/169)
- 이펙티브 자바 (3/E) (Item 39)
