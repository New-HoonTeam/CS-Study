

빈의 라이프 사이클에 따르면 11번과 14번 단계는 BeanPostProcessor와 연관되어 있다.

그 전에 10번까지 이미 모든 빈들이 생성되고, 의존관계가 설정되어 있기 때문에, 
BeanPostProcessor타입의 빈들은 @Autowired로 의존관계를 주입하는 로직을 적용한다.

![](https://velog.velcdn.com/images/goseungwon/post/d3ec601f-8da8-44bc-9674-333158a560e3/image.png)


![](https://velog.velcdn.com/images/goseungwon/post/690417a4-8c16-485e-bebe-afeb9afe6d87/image.png)

생성자, 메서드, 파라미터, 필드에 @Autowired를 사용해 스프링 컨테이너의 빈의 의존성을 주입한다.

스프링 4.3부터 생성자에 Autowired를 명시하지 않아도 자동으로 달아준다.

그렇기 때문에 주입하려는 대상이 빈으로 등록되었는지 확인해준다.

![](https://velog.velcdn.com/images/goseungwon/post/21d8450b-8c74-41f8-976a-92a7cecb2697/image.png)

autowired의 주석을 보면 실제 주입은 BeanPostProcessor를 통해 수행된다고 나와있다.

![](https://velog.velcdn.com/images/goseungwon/post/67facac0-0358-4af2-a8ff-045cf22d181c/image.png)


BeanPostProcessor는 인터페이스기 때문에 구현체가 필요하다.

![](https://velog.velcdn.com/images/goseungwon/post/2da2c946-0d77-4023-baf6-b1d103714ef8/image.png)


BeanPostProcessor를 구현하여 AutoWired의 핵심이 되는 클래스가 바로 AutowiredAnnotationBeanPostProcessor이다.

![](https://velog.velcdn.com/images/goseungwon/post/3301184e-572d-4d5a-b194-a840c980b73d/image.png)


AutowiredAnnotationBeanPostProcessor 클래스를 살펴보자

여러 메서드가 있지만 핵심이 되는 processInjection 메서드다.

![](https://velog.velcdn.com/images/goseungwon/post/62bd2253-4cac-4a73-a457-8c144fc50506/image.png)


1. 메타데이터를 받아온다.
2. 인젝션을 한다.
3. 예외 처리를 한다.

메타데이터를 가져오는 메서드를 알아보자.

![](https://velog.velcdn.com/images/goseungwon/post/04b432f5-dd5f-4411-b184-e208b00a32f4/image.png)


다음으로 인젝션을 수행하는 메서드를 알아보자

InjectionMetadata 클래스의 inject메서드인데, null 여부를 확인한 뒤 다른 injection 메서드를 호출한다.

![](https://velog.velcdn.com/images/goseungwon/post/85de1cca-7dbf-45e1-9061-bc2c5ab8d0fa/image.png)

![](https://velog.velcdn.com/images/goseungwon/post/62c8e77b-4303-4aa8-9698-372cf6ab1e5f/image.png)


메서드 내부에서 reflectionUtils를 통해 주입을 하는데, reflection은 스프링 내부 구현 대부분을 이루고 있을만큼 큰 역할을 한다. 

reflection은 클래스, 인터페이스, 필드, 메서드의 런타임 특성을 검사/수정할 수 있는 것인데 자세한 내용은 [여기](https://www.baeldung.com/java-reflection)를 확인해보자

참조

---

[ReflectionUtils (Spring Framework 6.0.3 API)](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/util/ReflectionUtils.html)

[@Autowired의 동작원리](https://beststar-1.tistory.com/40#AbstractAutowireCapableBeanFactory)
