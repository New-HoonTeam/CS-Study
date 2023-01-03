# Exception Handling in Servlet Filter and Spring Interceptor

# 예외 처리가 왜 문제가 되는 거죠??

![Untitled](https://user-images.githubusercontent.com/39071638/210305046-182d7380-4963-4b2e-8f8d-637eb2bacf0d.png)

주로 Error Handling이 이루어지는 곳? → Controller 

그런데 Filter와 Interceptor는 Controller로 가기 밖에 위치한 부분

그러면 Filter와 Interceptor에서 발생한 예외 처리는 어떻게 해야할까요??

그냥 예외를 던지면?? →

500코드를 던지면서 에러

## Filter

일반적으로는 `@ControllerAdvice` 로 해결하려고 시도하지만 Filter는 Controller의 범위 밖에 존재하기 때문에 별도의 처리를 해 줘야 한다.

![Untitled 1](https://user-images.githubusercontent.com/39071638/210305055-b96e7dd7-cb05-46fb-9df3-ab32800bfa74.png)

- 예외가 발생할 것 같은 필터의 상위에서 예외를 처리해주는 필터를 추가해 주면 된다!
- 그리고 해당 예외 처리 필터를 Security Config에 추가하면 끝!

```java
public class ExceptionHandlerFilter{
    @Override
    protected void doFilterInternal(
            HttpServletRequest request,
            HttpServletResponse response,
            FilterChain filterChain
    ) throws ServletException, IOException {

        try{
            filterChain.doFilter(request, response);
        }catch (ExpiredJwtException e){
            //토큰의 유효기간 만료
            setErrorResponse(response, ErrorCode.TOKEN_EXPIRED);
        }catch (JwtException | IllegalArgumentException e){
            //유효하지 않은 토큰
            setErrorResponse(response, ErrorCode.INVALID_TOKEN);
        }
    }
    private void setErrorResponse(
            HttpServletResponse response,
            ErrorCode errorCode
    ){
        ObjectMapper objectMapper = new ObjectMapper();
        response.setStatus(errorCode.getHttpStatus().value());
        response.setContentType(MediaType.APPLICATION_JSON_VALUE);
        ErrorResponse errorResponse = new ErrorResponse(errorCode.getCode(), errorCode.getMessage());
        try{
            response.getWriter().write(objectMapper.writeValueAsString(errorResponse));
        }catch (IOException e){
            e.printStackTrace();
        }
    }

    @Data
    public static class ErrorResponse{
        private final Integer code;
        private final String message;
    }
}
```

```java
@Configuration
@EnableWebSecurity
public class SpringSecurityConfig extends WebSecurityConfigurerAdapter {

    @Override
    protected void configure(HttpSecurity http) throws Exception {
		...
        
        http.addFilterBefore(
            new ExceptionHandlerFilter(),
            UsernamePasswordAuthenticationFilter.class
        );
        
        ...
    }
}
```

## Interceptor

Interceptor는 Controller 수행 전에 넘어온 Request를 처리하는 객체
![Untitled 2](https://user-images.githubusercontent.com/39071638/210305073-1b077f7a-869b-45d0-8ad6-54ac8e65fdb1.png)

- HandlerInterceptor, DispatcherServlet, ExceptionResolver의 동작 순서는 아래와 같다.`DispatcherServlet` → `preHandler(HandlerInterceprot)` → `Controller` → `postHandler(HandlerInterceprot)`→ `DispatcherServlet` → `afterCompletion` → `응답`
- 즉, `afterCompletion` 전에 발생한 예외는 모두 `DispatcherServlet`으로 반환된다.`afterCompletion`은 모든 처리가 끝난 후 예외 정보, 요청, 응답 정보를 모두 받는 위치에 있기 때문이다.
- 그럼 결국 핸들링이 되어야 한다. 그런데 왜 안된걸까?

### Handler의 존재 유무에 따라서 처리 과정이 다르다

### 핸들러가 존재할 때와 존재하지 않을 때로 구분한다.

`HandlerExceptionResolver`는 핸들러에 연관된 예외를 핸들링 하기 위한 것이다.

- 핸들러가 존재할 때
    1. `preHandle`에서 예외 발생
    2. 핸들러가 존재하는지 확인(`@Controller, @RestController`)
    3. 존재한다면, `ExceptionResolver`가 예외 해결 시도
- 핸들러가 존재하지 않을 때
    1. `preHandler`에서 예외 발생
    2. 핸들러가 존재하는지 확인
    3. 존재하지 않으면, 그대로 WAS까지 예외 전달
    4. 이후 `/error`로 재요청하며, 500 페이지 제공

결국 핸들러가 존재하지 않았기 때문에 핸들링이 되지 않는 범위였던 것

### 해결 방법

`HandlerExceptionResolver`가 에러를 핸들링 할 수 있게 하려면 어떻게 해야 하나요???

- forward
- redirect

```java
public abstract class SessionLogin implements HandlerInterceptor {
		...
		private boolean isAccessToken(HttpServletRequest request, HttpServletResponse response, String accessToken) throws ServletException, IOException {
				if (accessToken != null) {
				    return true;
				}
				
				request.setAttribute("message", "로그인이 필요합니다.");
				request.setAttribute("exception", "AuthenticationException");
				request.getRequestDispatcher("/api/error").forward(request, response);
				System.out.println("request = " + request);
				return false;
				}
		}
		...
}
```

```java
@RestController
public class ErrorController {
    @GetMapping("/api/error")
    public void error(HttpServletRequest request) throws AuthenticationException {
        String message = (String) request.getAttribute("message");
        String exception = (String) request.getAttribute("exception");

        if ("AuthenticationException".equals(exception)) {
            throw new AuthenticationException(message);
        }
    }
}
```

이렇게 되면 컨트롤러로 예외가 넘어오기 때문에 에러가 핸들링이 가능해짐

아니면 preHandler 메소드에서 forwarding해서 해결하는 방법도 존재

```java
@Override
	public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler)
			throws AuthenticationException {
		if (request.getMethod().equals("OPTIONS")) {
			return true;
		}
		String jwt = request.getHeader("Authorization");
		if (jwt == null) {
			LOG.error("[JwtInterceptor] JWT is null");
            
			request.setAttribute("message", "JWT is required");
            		request.setAttribute("exception", "AuthenticationException");
            		request.getRequestDispatcher("/error").forward(request, response);
            
		}
		return true;
	}
```

[[Spring Security] Filter 에서 발생한 예외 핸들링하기](https://jhkimmm.tistory.com/29)

[[Spring] DispatcherServlet의 예외처리 전략(HandlerExceptionResolver)](https://velog.io/@gillog/Java-HandlerInterceptor%EB%8B%A8-Exception-Handling%ED%95%98%EA%B8%B0HandlerExceptionResolver-ExceptionHandler)

[Spring Interceptor에서 예외를 응답 해주는 방법](https://velog.io/@monkeydugi/Spring-Interceptor%EC%97%90%EC%84%9C-%EC%98%88%EC%99%B8%EB%A5%BC-%EC%9D%91%EB%8B%B5-%ED%95%B4%EC%A3%BC%EB%8A%94-%EB%B0%A9%EB%B2%95)
