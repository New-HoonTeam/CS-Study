# Bean Scope?

빈이 존재할 수 있는 범위

# 종류

- 싱글톤 : 스프링 컨테이너의 시작과 함께 종료 할 때까지 유지되는 가장 넓은 범위의 스코프
- 프로토 타입 : 스프링 컨테이너가 생성과 의존관계 주입까지만 관여하고 
그 이후는 관리하지 않는 매우 짧은 범위의 스코프
- 웹 관련 스코프
    - request : 웹 요청이 들어오고 나갈 때까지 유지되는 스코프
    - session : 웹 세션이 생성되고 종료될 때까지 유지되는 스코프
    - application : 웹의 서블릿 컨텍스트와 같은 범위로 유지되는 스코프
    

## 싱글톤 타입

![Untitled](https://user-images.githubusercontent.com/86050295/208333067-e28fc057-788a-4684-9762-d8b07b4991dd.png)

싱글톤 타입 빈은 생성과 함께 만들어지고, 
싱글톤 스코프의 빈을 조회하면, 매번 같은 객체를 반환한다.

```java
public class SingletonTest {
 
 @Test
 public void singletonBeanFind() {
 AnnotationConfigApplicationContext ac = new AnnotationConfigApplicationContext(SingletonBean.class);
		SingletonBean singletonBean1 = ac.getBean(SingletonBean.class);
		SingletonBean singletonBean2 = ac.getBean(SingletonBean.class);

		System.out.println("singletonBean1 = " + singletonBean1);
		System.out.println("singletonBean2 = " + singletonBean2);

		assertThat(singletonBean1).isSameAs(singletonBean2);
		ac.close(); //종료
 }

 @Scope("singleton")
 static class SingletonBean {

		@PostConstruct
		public void init() {
			System.out.println("SingletonBean.init");
		}

		@PreDestroy
		public void destroy() {
			System.out.println("SingletonBean.destroy");
		}
 }
}
```

```
SingletonBean.init
singletonBean1 = hello.core.scope.PrototypeTest$SingletonBean@54504ecd
singletonBean2 = hello.core.scope.PrototypeTest$SingletonBean@54504ecd
org.springframework.context.annotation.AnnotationConfigApplicationContext - 
Closing SingletonBean.destroy
```

- 빈 초기화 메서드가 실행
- 같은 인스턴스 빈을 조회
- 종료 메서드까지 정상 호출

## 프로토 타입

![Untitled1](https://user-images.githubusercontent.com/86050295/208333202-ac883d6e-8cb7-4c87-b2e0-04d635a7149f.png)


- 프로토타입 스코프 빈을 요청하면 이 시점에 
프로토 타입 빈을 새로운 프로토타입 빈을 생성해서 매번 다른 객체를 반환한다.
- 클라이언트에 빈을 반환하고, 스프링 컨테이너는 더이상 프로토 타입 빈을 관리하지 않는다.
- `@PreDestroy` 같은 종료 메서드가 호출되지 않는다.
- 반환된 이후 부터는 프로토 타입 빈을 관리할 책임은 프로토타입 빈을 받은 클라이언트에 있다.

```java
public class PrototypeTest {
    @Test
    public void prototypeBeanFind() {
        AnnotationConfigApplicationContext ac = new
                AnnotationConfigApplicationContext(PrototypeBean.class);
        System.out.println("find prototypeBean1");
        PrototypeBean prototypeBean1 = ac.getBean(PrototypeBean.class);
        System.out.println("find prototypeBean2");
        PrototypeBean prototypeBean2 = ac.getBean(PrototypeBean.class);

        System.out.println("prototypeBean1 = " + prototypeBean1);
        System.out.println("prototypeBean2 = " + prototypeBean2);

        assertThat(prototypeBean1).isNotSameAs(prototypeBean2);
        ac.close(); //종료
    }

    @Scope("prototype")
    static class PrototypeBean {
        @PostConstruct
        public void init() {
            System.out.println("PrototypeBean.init");
        }

        @PreDestroy
        public void destroy() {
            System.out.println("PrototypeBean.destroy");
        }
    }
}
```

```
find prototypeBean1
PrototypeBean.init
find prototypeBean2
PrototypeBean.init
prototypeBean1 = hello.core.scope.PrototypeTest$PrototypeBean@13d4992d
prototypeBean2 = hello.core.scope.PrototypeTest$PrototypeBean@302f7971
org.springframework.context.annotation.AnnotationConfigApplicationContext - Closing
```

- 싱글톤 빈은 스프링 컨테이너 생성 시점에 초기화가 실행 되지만, 
프로토타입 스코프의 빈은 스프링 컨테이너에서 빈을 조회할 때 생성되고, 
초기화 메서드도 실행된다.
- 프로토타입 빈을 2번 조회했으므로 완전히 다른 스프링 빈이 생성되고, 
초기화도 2번 실행된 것을 확인할 수 있다.
- 프로토타입 빈은 스프링 컨테이너가 생성과 의존관계 주입 
그리고 초기화 까지만 관여하고, 더는 관리하지 않는다. →
- 따라서 프로토타입 빈은 스프링 컨테이너가 종료될 때 
@PreDestroy 같은 종료 메서드가 실행되지 않는다.

# 웹 스코프의 특징

웹 스코프는 웹 환경에서만 동작한다.
웹 스코프는 프로토타입과 다르게 스프링이 해당 스코프의 종료시점까지 관리한다. 
따라서 종료 메서드가 호출된다.

## 웹 스코프 종류

- request : HTTP 요청 하나가 들어오고 나갈 때 까지 유지되는 스코프, 
각각의 HTTP 요청마다 별도의 빈 인스턴스가 생성되고, 관리된다.
- session : HTTP Session과 동일한 생명주기를 가지는 스코프
- application : 서블릿 컨텍스트(`ServletContext`)와 동일한 생명주기를 가지는 스코프
- websocket : 웹 소켓과 동일한 생명주기를 가지는 스코프

웹 스코프는 빈의 존재 범위만 다르고 동작 방식은 같다.

![web1](https://user-images.githubusercontent.com/86050295/208333300-b7e89a7f-dfaa-4898-8fb2-b06e91a7657c.png)

![web2](https://user-images.githubusercontent.com/86050295/208333314-988fc5cb-8e8d-4ab0-84ee-33f5d238a863.png)


김영한 인프런 강의 참고
