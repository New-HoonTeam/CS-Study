## Dispatcher Servlet   

HTTP 프로토콜로 들어오는 모든 요청을 가장 먼저 받아 적합한 컨트롤러에 위임해주는 프론트 컨트롤러(Front Controller)  
- 정적 리소스의 경우에는) 요청을 처리할 컨트롤러를 먼저 찾고, 요청에 대한 컨트롤러를 찾을 수 없는 경우에,   
  2차적으로 설정된 자원(Resource) 경로를 탐색하여 자원을 탐색   

<br/>

### 동작과정  

앞 과정 참고)   
HttpServlet에 구현된 service()에서 `Servlet 관련` Request/Response 객체를 `Http 관련` Request/Response로 캐스팅해줌    

[Dispatcher code 뜯어보기](https://mangkyu.tistory.com/216)

```java
public abstract class HttpServlet extends GenericServlet {

    @Override
    public void service(ServletRequest req, ServletResponse res)
        throws ServletException, IOException {

        HttpServletRequest  request;
        HttpServletResponse response;

        try {
            request = (HttpServletRequest) req;
            response = (HttpServletResponse) res;
        } catch (ClassCastException e) {
```

<br/>  

![제목 없음](https://user-images.githubusercontent.com/103614357/207763896-82637d62-ca50-47ba-9561-9969f6692130.png)    

**1. Cleint의 요청을 Dispatcher Servlet이 받음**   

<br/>

**2. 요청 정보를 통해 요청을 위임할 Controller 찾음**   
- Dispatcher Servlet이 `HandlerMapping`에게 요청정보 보냄  
- HandlerMapping은 request uri에 알맞은 `Handler객체`를 가져옴     
  (실제 Handler의 mapping은 DispatcherServlet에 주입된 HandlerMapping 구현체(RequestMappingHandlerMapping)에 의해 됨)

<br/>

**3. 요청을 Controller로 위임할 Handler Adapter를 찾아서 전달함**   
- Adapter interface를 통해 Controller를 호출하는 이유는 `Controller의 구현 방식`이 다양하기 떄문
  - 최근에는 @Controller에 @RequestMapping 관련 annotation을 사용해 Controller 클래스를 주로 작성하지만,    
    - @RequestMapping은 `RequestMappingHandlerAdapter`를 어노테이션화 한 것 
  - Controller interface를 구현하여 Controller 클래스를 작성할 수도 있음  

==> Spring은 HandlerAdapter라는 Adapter interface를 통해 `Adapter pattern`을 적용함으로써 Controller의 구현 방식에 상관없이 요청을 위임할 수 있는 것!     

<br/>

**4. Handler Adapter가 Controller로 요청을 위임함**     

![image](https://user-images.githubusercontent.com/103614357/207768496-38f4a558-eac5-492f-a7de-5489c655e4ad.png)   

- 이 때, 공통적인 전,후처리 과정 필요
  - 적용할 Interceptor들  
  - @RequestParam, @RequestBody 등을 처리하기 위한 `ArgumentResolver`  
    - parameter 값을 확인하여 정보를 전달받으면,    
      Argument Resolver가 `MessageConverter`를 사용해서   
      해당 parameter를 Controller가 필요로 하는 데이터로 변환
  - 응답 시에 ResponseEntity의 Body를 Json으로 직렬화하는 등의 처리를 하는 `ReturnValueHandler`
    - MessageConverter를 사용하여 응답 결과를 만들어 반환

<br/>

**5. 비지니스 로직을 처리함**   

<br/>

**6. `Controller`가 반환값을 반환함**   
- View 이름 반환하거나
- ResponseEntity 반환

<br/>

**7. Controller로부터 받은 응답을 `HandlerAdapter`가 반환값을 처리함**   
- Controller로부터 받은 응답을 응답 처리기인 ReturnValueHandler가 후처리   
  (Controller에서 String으로 View의 이름을 반환하든, @ResponseBody를 사용하여 객체를 반환하든 상관없이 동작하는 이유가 바로 이 ReturnValueHandler 덕분)   
  - Controller가 `View 이름`을 반환하면 `ViewResolver`를 통해 rendering된 View를 반환
  - Controller가 `ResponseEntity`를 반환하면 `MessageConverter` 사용해 응답 결과를 Json으로 직렬화해서 반환         

<br/>
   
**8. 서버의 응답을 Client로 반환함**      

<br/><br/>

Reference    
https://docs.spring.io/spring-framework/docs/current/reference/html/web.html#mvc    
https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81-mvc-1#curriculum   
https://mangkyu.tistory.com/18     
https://mangkyu.tistory.com/216   
https://maenco.tistory.com/entry/Spring-MVC-Argument-Resolver%EC%99%80-ReturnValue-Handler   
https://ttl-blog.tistory.com/262  

<br/>   
