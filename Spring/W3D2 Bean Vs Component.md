두 어노테이션 모두 IoC 컨테이너에 Bean을 등록하기 위해 사용합니다

@Component : 개발자가 작성한 class를 기반으로 실행시점에 인스턴스 객체를 1회(싱글톤) 생성합니다

@Controller, @Service, @Repository 는 모두 @Component 이며 실행시점에 자동으로 의존성을 주입합니다

@Bean : 개발자가 작성한 method를 기반으로 메서드에서 반환하는 객체를 인스턴스 객체로 1회(싱글톤) 생성합니다

### Bean과 Component를 비교하기 전에 먼저 Component와 Configuration을 비교해보자

**@Component**

```java
@Target({ElementType.TYPE})
@Retention(RetentionPolicy.RUNTIME)
@Documented
@Indexed
public@interfaceComponent {
  String value()default"";
}
```

- Class, Enum, Interface에 어노테이션을 사용할 수 있다.
- 서버 실행시 빈으로 등록된다.
- ComponentScan의 대상이 된다.

**@Configuration**

```java
@Target({ElementType.TYPE})
@Retention(RetentionPolicy.RUNTIME)
@Documented
@Component
public@interfaceConfiguration {
  @AliasFor(
    annotation = Component.class
)
  String value()default"";

booleanproxyBeanMethods()default true;
}

```

- 내부에 @Component를 가지고 있다.
- Component와 동일하게 Class, Enum, Interface에 어노테이션을 사용할 수 있다.

### Configuration 어노테이션 내부에 Component 어노테이션을 가지고 있기 때문에 이 둘을 비교할 수 없다.

### @Component와 @Bean을 비교해보자

**@Bean**

```java
@Target({ElementType.METHOD, ElementType.ANNOTATION_TYPE})
@Retention(RetentionPolicy.RUNTIME)
@Documented
public@interfaceBean {
  @AliasFor("name")
  String[] value()default{};

  @AliasFor("value")
  String[] name()default{};

/**@deprecated*/
@Deprecated
  Autowire autowire()defaultAutowire.NO;

booleanautowireCandidate()default true;

  String initMethod()default"";

  String destroyMethod()default"(inferred)";
}
```

- Configuration을 어노테이션을 사용한 클래스 내부에서 사용한다.
- 메서드에 어노테이션을 사용할 수 있다.
- 서버 실행시 메서드의 반환값을 빈으로 등록한다.

### 정리

- @Component와 @Configuration은 별개의 개념이 아니다.
- @Component는 클래스를 빈으로, @Configuration + @Bean은 메서드의 반환값을 빈으로 등록한다.

### 그렇다면 다른점은 무엇일까?

@Component와 Bean의 Target이 클래스와, 메서드로 다르다.

이를 통해서 만들 수 있는 차별점은 이와 같다.

개발자가 직접 작성한 클래스의 경우 @Component 어노테이션을 사용할 수 있다. 
하지만 다음과 같이 내장 클래스 또는 라이브러리와 같이 개발자가 직접 제어할 수 없는 클래스의 경우 @Component 어노테이션을 사용할 수 없기 때문에 메서드에 @Bean을 붙여 빈으로 등록하여 사용할 수 있다.

```java
@Bean
publicPasswordEncoder passwordEncoder() {
return newBCryptPasswordEncoder();
}
```

### PS

@Component와 @Bean을 동시에 사용하면 어떻게 될까? (Light Mode)

```java
@Component
public class ComponentTest {
    @Bean
    public TestClass testComponent() {
        return new TestClass();
    }
}
```

결론부터 말하자면 실행이 된다.

@Configuration을 사용하면 스프링은 빈을 등록할 때 CGLIB을 이용한 프록시 객체로 등록한다. 
하지만 @Component를 사용하기 때문에 Light Mode는 실제 인스턴스를 생성한다.

그렇기 때문에 proxy객체가 필요한 스프링의 몇몇 기능을 사용할 수 없다.(AOP, 접근제어)

참고

---

https://www.baeldung.com/spring-component-annotation

[@Component vs @Configuration](https://hyune-c.tistory.com/entry/Component-vs-Configuration)
