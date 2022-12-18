# IoC Container의 역할

# IoC?

- Inversion of Control의 약자로 제어의 역전이란 뜻이다
- 말 그대로 제어가 역전되었다는 뜻이다
- 메소드나 객체의 호출 작업을 개발자가 결정하는 것이 아니라 **외부**에서 결정하는 것

기존의 객체 생성 순서

1. 객체 생성
2. 의존성 객체 생성
3. 의존성 객체 메소드 호출

스프링에서의 객체 생성 순서

1. 객체 생성
2. 의존성 객체 주입(스스로가 만드는것이 아니라 제어권을 스프링에게 위임하여 스프링이 만들어놓은 객체를 주입한다.)

3.의존성 객체 메소드 호출

# IoC Container?

> 컨테이너는 객체의 수명 주기를 담당하고, 추가 기능을 관리하는 역할을 수행다는 주체
> 

Spring에서는 Bean의 생명 주기를 담당, 책임지고 의존성을 관리해주는 객체가 존재하는데 이게 바로 IoC Container. 객체의 생성부터 사용, 소멸까지 담당해주므로 개발자는 객체를 관리할 필요가 없기 때문에 비즈니스 로직에만 집중할 수 있음

![Untitled](https://user-images.githubusercontent.com/39071638/208289853-5a2d4dda-269d-49a6-9cbb-00c85c407885.png)
## Bean

> IoC Container가 관리하는 객체를 빈 이라고 부름
즉 Spring이 관리해주는 객체
> 

Spring 프레임워크에서의 IoC Container의 위치

`org.springframework.beans` , `org.springframework.context` 

- `BeanFactory` interface
    - BeanFactory 계열의 인터페이스만 구현한 클래스는 단순히 컨테이너에서 객체를 생성하고 DI를 처리하는 기능만 제공함
    - Bean을 등록, 생성, 조회, 반환 관리를 한다
    - 팩토리 디자인 패턴을 구현한 것으로 BeanFactory는 빈을 생성하고 분배하는 책임을 지는 클래스이다
    - Bean을 조회할 수 있는 getBean() 메소드가 정의되어 있다.
    - 보통은 BeanFactory를 바로 사용하지 않고, 이를 확장한 ApplicationContext를 사용한다
- `ApplicationContext`
    - BeanFactory의 자식 인터페이스
    - Bean을 등록, 생성, 조회, 반환 관리하는 기능은 BeanFactory와 같다
    - Spring의 AOP 구성 요소들과 더 쉽게 통합할 수 있게 도와줌
        - 국제화가 지원되는 텍스트 메시지를 관리 해준다.
        - 이미지같은 파일 자원을 로드할 수 있는 포괄적인 방법을 제공해준다.
        - 리스너로 등록된 빈에게 이벤트 발생을 알려준다.
    - 그래서 대부분의 경우에는 ApplicationContext 를 사용하는 것이 좋아요

### Application  Context 종류

- StaticApplicationContext
    - StaticApplicationContext는 코드를 통해 빈 메타정보를 등록하기 위해 사용한다. 스프링의 기능에 대한 학습 테스트를 만들 때를 제외하면 실제로 사용되지 않는다.
    
    > 스태틱 애플리케이션 컨텍스트는 실전에서는 사용하면 안 된다. 테스트 목적으로 코드를 통해 빈을 등록하고 컨테이너가 어떻게 동작하는지 확인하고 싶을 때를 대비해 이런 컨테이너가 있다는 정도만 기억해두자.
    > 
- GenericApplicationContext
    - 가장 일반적인 애플리케이션 컨텍스트의 구현 클래스다. 실전에서 사용될 수 있는 모든 기능을 갖추고 있는 애플리케이션 컨텍스트다. 컨테이너의 주요 기능을 DI를 통해 확장할 수 있도록 설계되어 있다.
- WebApplicationContext
    - 스프링에서 가장 많이 사용되는 AppContext
    - 웹 환경에서 사용할 때 필요한 기능이 추가된 애플리케이션 컨텍스트다
    - 스프링 App은 대부분
    - 서블릿 기반의 독립 웹 애플리케이션(WAR)으로 만들어지기 때문

![Untitled 1](https://user-images.githubusercontent.com/39071638/208289858-7b21549c-1ab6-4e8e-a328-c149f70f9b0a.png)
```java
public interface ApplicationContext extends EnvironmentCapable, ListableBeanFactory, HierarchicalBeanFactory,
		MessageSource, ApplicationEventPublisher, ResourcePatternResolver{
...
}
```

> 실제로 IoC 컨테이너로 말하는 객체들은 Application Context를 구현한 클래스의 객체이다
> 

Spring App은 하나 이상의 IoC 컨테이너를 가지고 있다.

[5. The IoC container](https://docs.spring.io/spring-framework/docs/3.2.x/spring-framework-reference/html/beans.html)

[[Spring] IoC, DI 란?](https://jobc.tistory.com/30)

[[Spring] IoC 컨테이너와 Bean](https://m.blog.naver.com/PostView.naver?isHttpsRedirect=true&blogId=pjok1122&logNo=221744895053)

[[Spring] IoC 컨테이너 (Inversion of Control) 란?](https://dev-coco.tistory.com/80)

[토비의 스프링 - IoC 컨테이너와 DI](https://gunju-ko.github.io/toby-spring/2019/03/25/IoC-%EC%BB%A8%ED%85%8C%EC%9D%B4%EB%84%88%EC%99%80-DI.html)
