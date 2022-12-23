Spring Application 구동 시에 특정 메서드를 실행시키는 방법은 크게 세 가지로 분류할 수 있다.

## Bean 생성 생명주기 콜백

우선순위는 아래 표시한 순서대로 호출되며 자세한 사용법은 [**태희님이 작성한 자료**](https://github.com/New-HoonTeam/CS-Study/blob/main/Spring/W2D5%20Spring%20Bean%20Create.md)를 참고

### 1. @PostConstruct

### 2. afterPropertiesSet (InitializingBean 인터페이스의 메서드 구현)

### 3. @Bean(initMethod=”METHOD_NAME”)

```java
@Component
public class ExampleBean implements InitializingBean {

    private static final Logger LOG = LoggerFactory.getLogger(ExampleBean.class);

    public ExampleBean() {
        LOG.info("Constructor");
    }

    @Override
    public void afterPropertiesSet() throws Exception {
        LOG.info("InitializingBean");
    }

    @PostConstruct
    public void postConstruct() {
        LOG.info("PostConstruct");
    }

    public void init() {
        LOG.info("init-method");
    }
}
```

```
[main] INFO o.p.sample.ExampleBean - Constructor
[main] INFO o.p.sample.ExampleBean - PostConstruct
[main] INFO o.p.sample.ExampleBean - InitializingBean
[main] INFO o.p.sample.ExampleBean - init-method
```

## Listener

이 방법은 Bean 생명주기에서 실행시키는 것과 달리, 즉 **특정 Bean의 생명주기와 관계없이 Spring Context가 초기화 된 후 발생하는 이벤트를 구독하는 방식**

### 1. ApplicationListener 인터페이스 구현

```java
@Component
public class ListenerExample implements **ApplicationListener<ContextRefreshedEvent>** {

    private static final Logger LOG = LoggerFactory.getLogger(ListenerExample.class);

    **@Override public void onApplicationEvent**(ContextRefreshedEvent event) {
        LOG.info("------------------- 메서드 실행 -------------------");
    }
}
```

❗ 단, ContextRefreshedEvent는 Spring Context가 **초기화** 된 시점 외에 **Refresh가 발생**한 시점에도 발생되니 참고

➕ 필요에 따라 ContextRefreshEvent 대신 상황에 맞게 다른 이벤트를 넣어줄 수도 있다.

### 2. @EventListener

앞서 설명한 ApplicationListener를 구현하지 않고 메서드에 **@EventListener 어노테이션**을 붙여 사용할 수 있는 방식

```java
@Component
public class EventListenerExample {

    private static final Logger LOG = LoggerFactory.getLogger(EventListenerExample.class);

    **@EventListener**
    public void onApplicationEvent(ContextRefreshedEvent event) {
        LOG.info("------------------- 메서드 실행 -------------------");
    }
}
```

## Runner

### CommandLineRunner

Spring Boot가 **run() 이라는 콜백 메서드를 가진 CommandLineRunner 인터페이스 제공**
run() 메서드 안에 원하는 로직을 작성하면 Spring Application Context의 초기화가 완료된 후 실행

```java
@Component
public class CommandLineRunnerApp implements CommandLineRunner {
    private static final Logger LOG = LoggerFactory.getLogger(CommandLineRunnerApp.class);

    @Override
    public void run(String...args) throws Exception {
        LOG.info("------------------- 메서드 실행 -------------------");
    }
}
```

CommandLineRunner의 구현체인 Bean을 여러 개 정의할 수 있으며 **Ordered 인터페이스 또는 @Order 어노테이션으로 실행 순서 지정 가능**

### ApplicationRunner

CommandLineRunner 외 유사한 **run() 콜백 메서드를 가진 ApplicationRunner 인터페이스 제공**
ApplicationArguments은 getSourceArgs(), getOptionNames(), getOptionValues() 등의 메서드 보유

```java
@Component
public class RunnerApp implements ApplicationRunner {
    private static final Logger LOG = LoggerFactory.getLogger(RunnerApp.class);

    @Override
    public void run(ApplicationArguments args) throws Exception {
        LOG.info("------------------- 메서드 실행 -------------------");
    }
}
```

### [부록] SpringApplication.java

```java
public class SpringApplication {
	...
	private void callRunners(ApplicationContext context, ApplicationArguments args) {
		List<Object> runners = newArrayList<>();
		runners.addAll(context.getBeansOfType(ApplicationRunner.class).values());
		runners.addAll(context.getBeansOfType(CommandLineRunner.class).values());
		AnnotationAwareOrderComparator.sort(runners);
		for(Object runner : new LinkedHashSet<>(runners)) {
			if(runner instanceof ApplicationRunner) {
         callRunner((ApplicationRunner) runner, args);
      }
			if(runner instanceof CommandLineRunner) {
         callRunner((CommandLineRunner) runner, args);
			}
		}
	}
	...
}
```

## 참고

[Spring 애플리케이션 시작 시 실행되는 로직 작성하기](https://sgc109.github.io/2020/07/09/spring-running-startup-logic/)

[CS-Study/W2D5 Spring Bean Create.md at main · New-HoonTeam/CS-Study](https://github.com/New-HoonTeam/CS-Study/blob/main/Spring/W2D5%20Spring%20Bean%20Create.md)

[[Spring Boot] 애플리케이션 실행 후 특정 코드를 수행하는 방법 (Application Runner 사용법)](https://jinseongsoft.tistory.com/238)

[[Spring] Spring의 @EventListener](https://sunghs.tistory.com/139)
