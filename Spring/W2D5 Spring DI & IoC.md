흔히 IoC와 DI를 헷갈리거나 동일시하고는 한다. 하지만 다른 개념이기 때문에 명확히 이해할 필요가 있다.
물론 IoC가 DI를 포함한다고도 할 수 있지만 DI 없이도 IoC를 만족하는 프로그램을 만들 수 있다고 한다.

우리는 [**마틴 파울러의 글**](https://martinfowler.com/articles/injection.html#InversionOfControl)에서 IoC와 DI가 다른 의미임을 알 수 있다.

> As a result I think we need a more specific name for this pattern. `Inversion of Control` is too generic a term, and thus people find it confusing. As a result with a lot of discussion with various IoC advocates we settled on the name `Dependency Injection`.
> 
> 
> 그 결과 이 패턴에 대해 좀 더 구체적인 이름이 필요하다고 생각한다. `Inversion of Control`은 너무 일반적인 용어이기 때문에 사람들은 그것을 혼동한다. 그 결과 다양한 IoC 옹호자들과 많은 논의를 거쳐 `Dependency Injection`이라는 이름을 정했다.
> 

## Inversion of Control (IoC, 제어의 역전)

**IoC(제어의 역전)**은 **프로그램의 제어 흐름을 직접 제어하는 것이 아니라 외부에서 관리**하는 것
코드의 최종 호출은 개발자가 제어하는 것이 아닌 **프레임워크** 내부에서 결정된 대로 이루어진다.

개발자가 작성한 코드가 직접 제어의 흐름을 담당하는 것은 **라이브러리**이다.

> ***프레임워크는 단지 미리 만들어준 반제품이나, 확장해서 사용할 수 있도록 준비된 추상 라이브러리의 집합이 아니다. 프레임워크가 어떤 것인지 이해하려면 라이브러리와 프레임워크가 어떻게 다른지 알아야 한다.***
> 
> 
> ***라이브러리를 사용하는 애플리케이션 코드는 애플리케이션 흐름을 직접 제어한다.***
> 
> ***단지 동작하는 중에 필요한 기능이 있을 때 능동적으로 라이브러리를 사용할 뿐이다.***
> 
> ***반면에 프레임워크는 거꾸로 애플리케이션 코드가 프레임워크에 의해 사용된다.***
> 
> ***프레임워크에는 분명한 [제어의 역전] 개념이 적용되어 있어야 한다.***
> 
> ***애플리케이션 코드는 프레임워크가 짜 놓은 틀에서 수동적으로 동작해야 한다.***
> 
> *토비의 스프링中*
> 

## Dependency Injection (DI, 의존관계 주입)

SOLID 원칙 중 개방 폐쇄 원칙(OCP), 의존성 역전 원칙(DIP)를 만족하게 하는 지원하는 일종의 패턴
결합성을 낮추기 위해 추상화한 **인터페이스에 의존**하고 이를 런타임시 **외부에서 주입**시켜주는 방법

> *이일민, 토비의 스프링 3.1, 에이콘(2012), p114*
> 
> - ***클래스 모델이나 코드에는 런타임 시점의 의존관계가 드러나지 않는다.
> 그러기 위해서는 인터페이스만 의존하고 있어야 한다.***
> - ***런타임 시점의 의존관계는 컨테이너나 팩토리 같은 제3의 존재가 결정한다.***
> - ***의존관계는 사용할 오브젝트에 대한 레퍼런스를 외부에서 제공(주입)해줌으로써 만들어진다.***

### 의존관계(또는 의존성) 주입 방법

각각의 방법과 장단점은 **[이전에 정리한 자료](https://github.com/New-HoonTeam/CS-Study/blob/main/Java/W2D4%20Dependency%20%26%20DI.md)**를 참고하면 좋을 것 같다.

## Spring DI & IoC 동작 원리

Spring에서는 **스프링 컨테이너**인 **ApplicationContext**를 이용하여 **설정 정보 기반으로 생성, 등록**하고 필요한 객체를 위 의존성 주입 방법(3가지)를 통해 **주입**한다.

### Configuration (설정 정보)

구성 정보를 설정하는 객체 (@Configuration 어노테이션이 붙은 클래스)

### Bean (스프링 빈)

스프링 컨테이너에 등록되어 관리되어지는 객체 (@Bean)

### Spring Container (컨테이너)

스프링 컨테이너, IoC 컨테이너, DI 컨테이너는 일반적으로 **같은 의미**이다.
그 외 어셈블러, 오브젝트 팩토리 등의 이름도 있지만 최근에는 잘 안 불리는 추세이다.

스프링 컨테이너는 XML 또는 자바 기반 설정 정보를 통해 스프링 빈을 등록한다.
자바 기반 설정 정보가 편하기 때문에 보통 **@Configuration**이 붙은 클래스를 설정 정보로 사용한다.
Spring Boot는 AutoConfiguration을 지원하여 프로젝트에 필요한 기본 설정 정보를 자동으로 생성, 등록한다.

스프링 컨테이너에서는 **@Bean으로 등록된 객체를 관리**하고 있다.
BeanFactory 인터페이스를 상속한 ApplicationContext를 통해서 Bean 객체를 조회할 수 있다.

![코드](https://p1dgey.notion.site/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2Fd5a93222-c4e6-4e62-a88e-49d3a907b811%2FUntitled.png?id=70b9244d-6f7c-4182-9d00-53a2fedfeca9&table=block&spaceId=b76551b9-9f24-4a91-9bcd-340caa404f60&width=990&userId=&cache=v2)

### **BeanFactory vs. ApplicationContext**

- BeanFactory
    - 스프링 컨테이너의 최상위 인터페이스
    - 스프링 빈을 관리하고 조회하는 역할
- ApplicationContext
    - BeanFactory 기능을 모두 상속받고 아래와 같이 부가 기능을 추가적으로 제공하는 인터페이스
        - MessageSource (메세지 소스를 활용한 국제화 기능)
        - EnvironmentCapable (로컬, 개발, 운영 등을 구분해서 처리할 환경 변수 관련)
        - ApplicationEventPublisher (이벤트를 발행하고 구독하는 모델을 편리하게 지원)
        - ResourceLoader (파일, 클래스패스, 외부 등 리소스를 편리하게 조회하도록 지원)

## 같이 볼 자료

[Component Scan](https://www.notion.so/Component-Scan-d29c2c512618457481fc77f4c8741cc2) 

[Autowired](https://www.notion.so/Autowired-89fe7334ea08459584bd56dd0b315ed4) 

[Bean Lifes Cycle](https://www.notion.so/Bean-Lifes-Cycle-75459ef98bc64a9aa2cb845c209a48db) 

[Beans Scope](https://www.notion.so/Beans-Scope-59bff6f4494b4d5aa783157a45b381e7) 

[AOP](https://www.notion.so/AOP-3dfa02b4e90d44dd821b494e9bff1c71) 

## 참고

[[Spring DI/IoC] IoC? DI? 그게 뭔데?](https://velog.io/@ohzzi/Spring-DIIoC-IoC-DI-%EA%B7%B8%EA%B2%8C-%EB%AD%94%EB%8D%B0)

[[Spring] 스프링 컨테이너 개념 및 동작 원리(DI,IoC,ApplicationContext 개념)](https://chobopark.tistory.com/200)
