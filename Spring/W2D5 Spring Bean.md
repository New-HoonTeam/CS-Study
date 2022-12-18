## Bean

스프링 빈은 자바 빈과 큰 차이가 있다.

### **자바 빈**

- 반복적인 작업을 효율적으로 하기 위해 사용하는 클래스
- private필드, getter/setter, 생성자를 가지고 있음
- 직렬화가 가능해야 한다.

### **스프링 빈**

스프링 IoC 컨테이너가 관리하는 Java 객체를 뜻한다.

- **IoC(Inversion of Control)**
    
    일반적으로 자바에서는 각 객체들이 프로그램의 흐름을 결정하고 각 객체를 직접 생성하고 호출했다. 
    즉 사용자가 제어하는 방식
    
    Spring IoC가 적용되면 객체의 생성을 특별한 주체에게 맡긴다. 사용자는 객체를 직접 생성하지 않고, 다른 주체가 객체의 생명주기를 컨트롤 한다. 즉 제어권이 다른 주체(스프링 컨테이너)에게 넘어가는 것을 뜻한다.
    
- **스프링 IoC 컨테이너의 역할**
    1. 객체의 생성, 의존성 관리
    2. POJO ( 위에서 말한 자바 빈과 비슷한 개념 ) 생성, 초기화, 서비스, 소멸 권한
    3. 개발자가 비즈니스 로직에 집중하게 도와준다.
    

## **스프링 빈을 컨테이너에 등록하는 방법**

1. 어노테이션
    
    @Component 어노테이션을 사용하면 스프링이 자체적으로 Bean을 등록한다.
    
    ```java
    @Controller
    @RequestMapping("/basic")
    public class BasicController {
    	...
    }
    ```
    
    Controller, Service등 다른 어노테이션은 Component어노테이션을 상속받고 있음
    
    ```java
    @Target(ElementType.TYPE)
    @Retention(RetentionPolicy.RUNTIME)
    @Documented
    @Component
    public @interface Controller {
    
    	@AliasFor(annotation = Component.class)
    	   String value() default "";
    }
    ```
    
2. Bean Configuration에 직접 등록 @Configuration과 @Bean 어노테이션을 이용해 등록할 수 있다.
    
    ```java
    @Configuration
    public class BasicConfiguration{
        @Bean
        public BasicConfiguration sampleController() {
            return new SampleController;
        }
    }
    ```
    
3. XML파일에 설정
    
    claass = 자바 Class의 위치 (필수)
    
    id = bean 의 id (클래스이름)
    
    scope = 객체의 범위 (singleton, prototype 등등)
    
    constructor-arg = 생성 시 생성자에 전달할 인수
    
    property = 생성 시 bean setter에 전달할 인수
    
    init-method, destroy-method
    
    ```xml
    <bean id="아이디" class="클래스이름" scope="스코프종류 ....>
    		<property name="이름" value="내용"/>
    </bean>
    ```
    
    Bean은 개발자가 **컨트롤 할 수 없는** 외부라이브러리를 등록하고 싶은 경우에 사용할 수 있으며 메소드 또는 어노테이션 단위에 붙일 수 있다. Component는 개발자가 **컨트롤 할 수 있는** 경우에 사용하며 클래스 또는 인터페이스에 붙일 수 있다.
    

## 스프링에서 빈 객체를 관리하는 방법

- **스프링이 스캔하는법**
    1. 어노테이션
        
        스프링 Container를 만들어 Bean을 등록하는데 사용되는 interface들을 Life Cycle Callback이라 한다.
        
        Life Cycle Callback은 @Component가 붙은 모든 Class의 interface를 만들어 Bean으로 등록한다.
        
        이 후 @Component가 붙은 클래스는 component scan 대상이 되어 스캔 후 등록된다.
        
    2. Bean Configuration에 등록
        
        @Configuration어노테이션도 내부에 @Component를 사용하기 떄문에 component scan의 대상이 되고, @Bean을 등록한다.
        
    3. XML
        
        XML을 읽어서 BeanDefinition을 만든다.
        
        ```java
        public static void main(String[] args) {  
          ApplicationContext context = 
        							new ClassPathXmlApplicationContext("application.xml");
        }
        ```
        
1. 빈 등록
    
    스프링 컨테이너는 파라미터로 넘어온 Config정보를 사용해서 스프링 빈을 등록한다.
    
    [ bean 메서드 이름 / bean 객체 ]형식으로 저장한다. 이 때 객체 인스턴스를 싱글톤으로 관리한다.
    
    이때  bean이름이 같으면 무시되거나 덮어버리는 오류가 발생하기 때문에 
    bean이름은 항상 다른이름을 부여해야 한다.
    
2. 의존관계 설정
    
    스프링 컨테이너는 설정 정보를 참고해 의존관계를 주입한다(DI)
    
    상속관계가 있는 빈을 조회하면 자식타입은 모두 조회된다.
    

**스프링 컨테이너의 인터페이스들**

1. BeanFactory
    
    스프링 컨테이너의 최상위 클래스
    
    스프링 빈을 관리하고 조회하는 역할
    
2. ApplicationContext
    
    BeanFactory를 모두 상속해서 사용한다.
    
    BeanFactory기능 외에도 애플리케이션을 개발할 때 수 많은 부가기능을 제공한다.(환경변수, 국제화, 이벤트 등등)
    

참고

---

[What is a Spring Bean? | Baeldung](https://www.baeldung.com/spring-bean)
