### SOP (Same Origin Policy)

다른 출처의 리소스 사용을 제한하는 보안 메커니즘

- Protocol (http vs. https)
- Host (naver.com vs. google.com)
- Port (80 vs. 443)
    - 과거 Internet Explorer는 포트까지 검증하지 않음

### Q. **http://localhost** 와 동일 출처인 URL은 무엇일까요?

- [ ]  https://localhost
- [x]  http://localhost:80
- [ ]  http://127.0.0.1
- [x]  http://localhost/api/cors

→ 물리적인 출처보다 URL에 표기된 스펙을 따른다.

### Why SOP?

웹 사이트가 브라우저에서 JS를 실행하여 악의적으로 데이터를 읽거나 사용하는 것을 방지

→ 다른 출처의 리소스가 필요하다면…?

## CORS (Cross-Origin Resource Sharing)

> *Mozilla 설명 중 일부 발췌…*
> 
> 
> 추가 HTTP 헤더를 사용하여 한 출처에서 실행 중인 웹 애플리케이션이 다른 출처의 선택한 자원에 접근할 수 있는 권한을 부여하도록 브라우저에 알려주는 메커니즘이다.
> 

다른 출처에 대한 두 가지 요청의 방식 *(Preflight 외에는 공식적으로 정의된 단어는 아니니 주의)*

- 단순 요청 (Simple Request)
    - 반드시 아래의 항목 중 하나
    
    | 특징 | Preflight 요청 없이 바로 요청 |
    | --- | --- |
    | HTTP Method | GET / POST / HEAD |
    | Content-Type | Application/x-www-form-urlencoded<br>multipart/form-data<br>text/plain |
    | Header | Accept<br>Accept-Language<br>Content-Language<br>Content-Type |
- 예비 요청과 실제 요청 (Preflight & Actual Request)
    1. 예비 요청 (Preflight Request)
        - OPTIONS 메서드를 통해 다른 도메인의 리소스에 요청이 가능한 지 확인
            
            → Origin (요청 출처)
            
            ← Access-Control-Allow-Origin (서버 측 허가 출처)
            
            → Access-Control-Request-Method (실제 요청의 메서드)
            
            ← Access-Control-Allow-Methods (서버 측 허가 메서드)
            
            → Access-Control-Request-Headers (실제 요청의 추가 헤더)
            
            ← Access-Control-Allow-Headers (서버 측 허가 헤더)
            
            ← Access-Control-Max-Age (Preflight 응답 캐시 기간)
            
            - 매번 요청마다 Preflight를 보내는 건 비효율적
        - 응답의 Status Code는 200번대
        - 응답의 Body는 비어있는 것이 좋음
    2. 실제 요청 (Actual Request)
        - Preflight 를 통해 요청이 가능하다고 판별이 되면 실제 요청
- 인증정보 포함 요청 (Credentialed Request)
    - 인증 관련 헤더를 포함할 때 사용하는 요청
    - 클라이언트
        - credentials : include
            - 클라이언트 측에서 쿠키나 JWT를 자동으로 담아서 보내고 싶을 때 설정
    - 서버
        - Access-Control-Allow-Credentials : true
            - 클라이언트 측에서 보내는 인증 관련 헤더를 받을 수 있도록 허용
            - 단, 이때 Access-Control-Allow-Origin은 *이 아닌 정확한 출처

### 왜 굳이 Preflight가 필요할까?

- CORS를 모르는 서버를 위해서 ?
    - 클라이언트가 브라우저를 통해 Origin을 밝히고 서버에 요청
    - 서버에서는 들어온 요청에 대한 로직 실행 후에 응답
        - CORS 설정을 하지 않았기 때문에 관련 헤더 누락
    - 브라우저가 응답을 처리하는 과정에서 CORS 헤더가 없기에 클라이언트에 에러를 발생
    - 즉, 서버에서는 정상적으로 연산을 수행했는데 클라이언트만 원하는 결과를 받지 못 하는 문제

→ Preflight를 통해 CORS 관련 설정을 체크하고 문제가 있을 경우 실제 요청은 하지 않음

## Spring CORS

### addCorsMapping 오버라이딩 (전역 설정)

```java
@EnableWebMvc
@Configuration
@ComponentScan(...)
static class ServletConfig implements WebMvcConfigurer, ... {
		...
		
		**@Override
		public void addCorsMappings(CorsRegistry registry) {
		    registry.addMapping("/api/**")         // api 하위 전체 허용을 의미 
		            .allowedMethods("GET", "POST") // Default : ALL
		            .allowedOrigins("*");
		}**

		...
}
```

### Controller 단위 전역 설정

```java
@Controller
**@CrossOrigin(origins = "*")
// @CrossOrigin(origins = "*", methods = {RequestMethod.GET, ...} )**
public class CustomerController {
		...
}
```

### Controller 메서드 단위 설정

```java
@GetMapping("/api/v1/customers/{customerId}")
**@CrossOrigin(origins = "*")**
public ResponseEntity<Customer> findCustomer(...) {
    Optional<Customer> customer = customerService.getCustomer(customerId);
    return customer.map(v -> ResponseEntity.ok(v))
            .orElse(ResponseEntity.notFound().build());
}
```

### 그 외…

- Servlet Filter (Custom CORS Filter)

[CORS (Cross-Origin Resource Sharing) 문제 filter로 해결하기](https://oingdaddy.tistory.com/4)

- CorsConfigurationSource

[Spring Security CORS 설정하기](https://oingdaddy.tistory.com/243)

- 프론트 서버의 Proxy 설정 (운영 환경에는 적합하지 않음)
- 직접 헤더 설정

## 참고

[Same-origin policy - Web security | MDN](https://developer.mozilla.org/en-US/docs/Web/Security/Same-origin_policy)

[https://www.youtube.com/watch?v=-2TgkKYmJt4](https://www.youtube.com/watch?v=-2TgkKYmJt4)
