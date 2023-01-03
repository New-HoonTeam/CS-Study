# Servlet Filter vs Spring Interceptor

# Servlet?

> 서블릿은 자바 클래스로 웹 애플리케이션을 작성한 뒤 이후 웹 서버 안에 있는 웹 컨테이너에서 이것을 실행하고, 웹 컨테이너에서는 서블릿 인스턴스를 생성 후 서버에서 실행되다가 웹 브라우저에서 서버에 요청(Request)을 하면 요청에 맞는 동작을 수행하고 웹 브라우저에 HTTP형식으로 응답(Response)합니다.
> 

## 주요 특징

- 클라이언트의 Request에 대해 동적으로 작동하는 웹 애플리케이션 컴포넌트
- HTML을 사용하여 Response 한다.
- JAVA의 스레드를 이용하여 동작한다.
- MVC 패턴에서의 컨트롤러로 이용된다.
- HTTP 프로토콜 서비스를 지원하는 javax.servlet.http.HttpServlet 클래스를 상속받는다.
- UDP보다 속도가 느리다.
- HTML 변경 시 Servlet을 재 컴파일해야 하는 단점이 있다.

![Untitled](https://user-images.githubusercontent.com/39071638/210304751-21ded379-3a6f-4b64-a1b9-4d5dfc9be203.png)

# Servlet Filter

Server와 Client 사이에서 서블릿으로 향하는 Request/Response를 중간에 가로채서 필터링을 위한 객체와 메서더를 정의해 둔  인터페이스

클라이언트와 서블릿 사이에 존재하면서 Request/Response를 가로채서 특정한 작업(필터링)을 수행한다

모든 Response/Request에 적용되야 하는 작업을 수행한다.

![Untitled 1](https://user-images.githubusercontent.com/39071638/210304764-716ab977-956f-43e8-a4b2-7fd41e1dfa6b.png)

### 3가지구현해야 하는 메소드

- init()
- doFilter()
- destroy()

```java
package javax.servlet;

import java.io.IOException;

public interface Filter {
    default void init(FilterConfig filterConfig) throws ServletException {
    }

    void doFilter(ServletRequest var1, ServletResponse var2, FilterChain var3) throws IOException, ServletException;

    default void destroy() {
    }
}
```

### **init**

init 메소드는 필터 객체를 초기화하고 서비스에 추가하기 위한 메소드이다. 웹 컨테이너가 1회 init 메소드를 호출하여 필터 객체를 초기화하면 이후의 요청들은 doFilter를 통해 처리된다.

### **doFilter**

doFilter 메소드는 url-pattern에 맞는 모든 HTTP 요청이 디스패처 서블릿으로 전달되기 전에 웹 컨테이너에 의해 실행되는 메소드이다. doFilter의 파라미터로는 FilterChain이 있는데, FilterChain의 doFilter 통해 다음 대상으로 요청을 전달하게 된다. chain.doFilter() 전/후에 우리가 필요한 처리 과정을 넣어줌으로써 원하는 처리를 진행할 수 있다.

### **destroy**

destroy 메소드는 필터 객체를 서비스에서 제거하고 사용하는 자원을 반환하기 위한 메소드이다. 이는 웹 컨테이너에 의해 1번 호출되며 이후에는 이제 doFilter에 의해 처리되지 않는다.

# Spring Interceptor

Client에서 Server로 들어온 Request 객체를 Controller의 Handler에 들어가기 전에 가로채는 역할을 수행함

Interceptor란 컨트롤러에 들어오는 요청 **HttpRequest**와 컨트롤러가 응답하는 **HttpResponse**를 가로채는 역할을 수행함

인터셉터는 관리자만 접근할 수 있는 관리자 페이지에 접근하기 전에 관리자 인증을 하는 용도로 활용될 수 있음

즉 Controller에 도착하기 전에 요청을 가로채서 원하는 추가 작업이나 로직을 수행한 후 Controller로 보내주는 역할을 수행하는 모듈이다
![Untitled 2](https://user-images.githubusercontent.com/39071638/210304776-29989310-ffa7-4f64-ac62-da453a224389.png)

사용 예제 : Client에서 요청이 들어왔는데, 로그인을 하지 않아서 세션이 생성되지 않았다면 Interceptro가 확인을 하고 로그인 페이지로 다시 보냄

인터셉터(Interceptor)는 J2EE 표준 스펙인 필터(Filter)와 달리 Spring이 제공하는 기술로써, 디스패처 서블릿(Dispatcher Servlet)이 컨트롤러를 호출하기 전과 후에 요청과 응답을 참조하거나 가공할 수 있는 기능을 제공한다. 즉, 웹 컨테이너에서 동작하는 필터와 달리 인터셉터는 스프링 컨텍스트에서 동작을 하는 것이다.

디스패처 서블릿은 핸들러 매핑을 통해 적절한 컨트롤러를 찾도록 요청하는데, 그 결과로 실행 체인(HandlerExecutionChain)을 돌려준다. 그래서 이 실행 체인은 1개 이상의 인터셉터가 등록되어 있다면 순차적으로 인터셉터들을 거쳐 컨트롤러가 실행되도록 하고, 인터셉터가 없다면 바로 컨트롤러를 실행한다.

인터셉터는 스프링 컨테이너 내에서 동작하므로 필터를 거쳐 프론트 컨트롤러인 디스패처 서블릿이 요청을 받은 이후에 동작하게 되는데, 이러한 호출 순서를 그림으로 표현하면 다음과 같다
![Untitled 3](https://user-images.githubusercontent.com/39071638/210304790-b58ffbb9-3d76-47c5-b16b-c9c44823a341.png)

```java
public interface HandlerInterceptor {

	default boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler)
			throws Exception {

		return true;
	}
	default void postHandle(HttpServletRequest request, HttpServletResponse response, Object handler,
			@Nullable ModelAndView modelAndView) throws Exception {
	}
	default void afterCompletion(HttpServletRequest request, HttpServletResponse response, Object handler,
			@Nullable Exception ex) throws Exception {
	}

}
```

### **preHandle 메소드**

preHandle 메소드는 컨트롤러가 호출되기 전에 실행된다. 그렇기 때문에 컨트롤러 이전에 처리해야 하는 전처리 작업이나 요청 정보를 가공하거나 추가하는 경우에 사용할 수 있다.

preHandle의 3번째 파라미터인 handler 파라미터는 핸들러 매핑이 찾아준 컨트롤러 빈에 매핑되는 HandlerMethod라는 새로운 타입의 객체로써, @RequestMapping이 붙은 메소드의 정보를 추상화한 객체이다.

또한 preHandle의 반환 타입은 boolean인데 반환값이 true이면 다음 단계로 진행이 되지만, false라면 작업을 중단하여 이후의 작업(다음 인터셉터 또는 컨트롤러)은 진행되지 않는다.

### **postHandle 메소드**

postHandle 메소드는 컨트롤러를 호출된 후에 실행된다. 그렇기 때문에 컨트롤러 이후에 처리해야 하는 후처리 작업이 있을 때 사용할 수 있다. 이 메소드에는 컨트롤러가 반환하는 ModelAndView 타입의 정보가 제공되는데, 최근에는 Json 형태로 데이터를 제공하는 RestAPI 기반의 컨트롤러(@RestController)를 만들면서 자주 사용되지는 않는다.

또한 컨트롤러 하위 계층에서 작업을 진행하다가 중간에 예외가 발생하면 postHandle은 호출되지 않는다.

### **afterCompletion 메소드**

afterCompletion 메소드는 이름 그대로 모든 뷰에서 최종 결과를 생성하는 일을 포함해 모든 작업이 완료된 후에 실행된다. 요청 처리 중에 사용한 리소스를 반환할 때 사용하기에 적합하다. postHandler과 달리 컨트롤러 하위 계층에서 작업을 진행하다가 중간에 예외가 발생하더라도 afterCompletion은 반드시 호출된다.

## Interceptor vs AOP

인터셉터 대신에 컨트롤러들에 적용할 부가기능을 어드바이스로 만들어 AOP(Aspect Oriented Programming, 관점 지향 프로그래밍)를 적용할 수도 있다. 하지만 다음과 같은 이유들로 컨트롤러의 호출 과정에 적용되는 부가기능들은 인터셉터를 사용하는 편이 낫다.

1. 컨트롤러는 타입과 실행 메소드가 모두 제각각이라 포인트컷(적용할 메소드 선별)의 작성이 어렵다.
2. 컨트롤러는 파라미터나 리턴 값이 일정하지 않다.
3. AOP에서는 HttpServletRequest/Response를 객체를 얻기 어렵지만 인터셉터에서는 파라미터로 넘어온다.

# Servlet Filter vs Spring Interceptor

우선 비교 하기 전에

위의 2가지를 사용하는 이유???

- 바로 공통의 작업은 한 번에 처리하기 위해

![Untitled 4](https://user-images.githubusercontent.com/39071638/210304806-d3dfa6b1-e9c7-4cb1-8821-d311e86193a6.png)

![Untitled 5](https://user-images.githubusercontent.com/39071638/210304814-1ac15277-b1fb-44ad-ba3d-88a91fddbc82.png)

> 위의 표에 적힌 내용들 중에서 각각이 사용되는 용도에 대해서는 자세히 살펴보도록 하자. 일부에서 필터(Filter)가 스프링 빈으로 등록되지 못하며, 빈을 주입 받을 수도 없다고 하는데, 이는 잘못된 설명이다. 이는 매우 옛날의 이야기이며, 필터는 현재 스프링 빈으로 등록이 가능하며, 다른 곳에 주입되거나 다른 빈을 주입받을 수도 있다.
> 

### Requset/Response 객체 조작

Filter는 메소드 chaining으로 request, respose를 인자로 넘겨줘서 추가적인 작업을 할 수 있는데, Interceptor는 그러한 작업을 할 수 없음

대신 Interceptor는 해당 객체의 내부 값을 조작할 수는 있음

# 그럼 언제 뭐를 사용해야하나요??

## Filter

- 공통된 보안 및 인증/인가 관련 작업
- 모든 요청에 대한 로깅 또는 감사
- 이미지/데이터 압축 및 문자열 인코딩
- Spring과 분리되어야 하는 기능

기본적으로 스프링과 무관하게 수행되어야 할 기능을 처리

대표적으로는 공통 검사가 있음

보안검사, 이미지나 데이터의 압축이나 문자열 인코딩과 같이 웹 애플리케이션에 전반적으로 사용되는 기능을 구현하기에 적당함

객체를 조작할 수 있다는 점에서 Interceptor보다 더 강력한 기술!

## Interceptor

- 세부적인 보안 및 인증/인가 공통 작업
- API 호출에 대한 로깅 또는 감사
- Controller로 넘겨주는 정보(데이터)의 가공

클라이언트의 요청과 관련되어 전역적으로 처리해야 하는 작업들을 처리할 수 있다.

대표적으로 필터(Filter)를 인증과 인가에 사용하는 도구로는 SpringSecurity가 있다. SpringSecurity의 특징 중 하나는 Spring MVC에 종속적이지 않다는 것인데, 이러한 이유로는 필터 기반으로 인증/인가 처리를 하기 때문이다

[[Web] 서블릿(Servlet)이란 무엇인가? 서블릿 총정리](https://coding-factory.tistory.com/742)

[서블릿 필터(filter)에 대해서](https://sgcomputer.tistory.com/238)

[[Spring] 필터(Filter) vs 인터셉터(Interceptor) 차이 및 용도 - (1)](https://mangkyu.tistory.com/173)

[javax.servlet 패키지](https://sgcomputer.tistory.com/232)

[[Spring] Interceptor란? 구현 예제와 함께 (HandlerInterceptor, HandlerInterceptorAdapter)](https://velog.io/@gillog/Spring-InterceptorHandlerInterceptor-HandlerInterceptorAdapter)

[[spring] Spring Interceptor 란?(HandlerInterceptor, HandlerInterceptorAdapter)](https://kimvampa.tistory.com/127)

[[Spring] Filter, Interceptor, AOP 차이 및 정리](https://goddaehee.tistory.com/154)
