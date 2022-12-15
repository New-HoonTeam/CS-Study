## Spring Bean의 생성과정  

### Spring Bean이란?  
- Spring IoC Container가 관리하는 자바 객체  
- Container에 의해 생명주기가 관리됨    
  = ApplicationContext가 알고 있는 객체, ApplicationContext가 생성하고 직접 관리해주는 객체   
  
<br/>   
  
- 우리가 new 연산자로 어떤 객체를 생성했을 때 그 객체는 bean이 아님
- `ApplicationContext.getBean()`으로 얻어질 수 있는 객체는 bean

<br/>

- 의존성 주입이 필요한 객체를 bean으로 등록  
=> Spring IoC Container가 `객체의 생성`과 `의존성 주입관리`       
=> 직접 객체를 생성해서 주입시켜주지 않아도됨 & 우리는 의존성을 사용하는 부분에만 집중할 수 있음   

<br/>

### Spring IoC Container에 bean 등록하는방법   
**bean 설정파일에 직접 bean 등록**   
- bean 설정파일
  - xml
  - Java 설정파일(`@Configuration`) + 그 안에 `Bean` 사용해 직접 bean 정의      


**Component scanning**   
- `@Component` : `@ComponentScan`이 붙어있는 클래스가 있는 패키지에서부터 모든 하위 패키지의 클래스에서 @Component 클래스를 찾음  

<br/><br/>  

### Bean객체의 생명주기(Singleton 기준)      

![제목 없음](https://user-images.githubusercontent.com/103614357/207495001-eeae94cd-cd37-4167-bbc4-534b99f2c1a8.png)

1. `Spring Container를 초기화`할 때, 가장 먼저 `bean 객체`를 메타데이터(설정 정보)에 맞추어 생성  

2. 먼저, 싱글톤 패턴을 사용하는게 아닌 `평범한 자바클래스`를 이용하여 객체 생성

3. Spring IoC Container는 `싱글톤 레지스트리` 라는 기능이 있음 - key, value 형태로 데이터 저장    
  => 싱글톤으로 관리위해 `빈이름을 key`로, `객체를 value로` 저장   
  => 의존성 필요 시 빈이름(key)를 이용하여 싱글톤 객체로 반환하게 됨  

4. 의존관계 설정  

5. `초기화 콜백` : bean에 필요한 데이터들을 필드에 초기화  
  
6. 이제 빈 사용 가능  

7. `소멸전 콜백`  
  
7. Spring Container 종료될 때, `bean 객체도 소멸`    

<br/>  

- 왜 `bean 생성`할 때 `초기화`를 같이 해주지 않고, bean의 등록과 의존 주입이 끝난 이후에 해줄까?   
  - 생성자
    - 필수 정보(파라미터)를 받고, 메모리를 할당해서 객체를 생성하는 책임을 짐   
  - 초기화
    - 객체 내부에서 값을 넣어주는 간단한 초기화 작업하는 경우도 있고,
    - 외부 커넥션을 연결하는 등 무거운 동작을 수행할 경우가 있다.   
    
  => 생성자 안에서 `무거운 초기화 작업`을 수행하는 것보다는      
  객체를 생성하는 부분과 초기화하는 부분을 명확하게 나누는 것이 좋음   
  또한! 객체의 생성과 객체의 초기화는 `다른 책임`     
  SRP 원칙에 따라 두 로직은 분리되어야 함  

<br/><br/>

### 참고) 사용자가 정의한 bean의 생명주기 콜백 메서드를 사용하고자 할 때 3가지 방법       
**1. interface(InitioalizingBean, DisposableBean)**   
- 인터페이스를 상속받아 초기화 메서드, 소멸전 메서드 구현가능    
  - `InitializingBean`은 `afterPropertiesSet()` 메서드로 초기화 지원  
  - `DisposableBean`은 `destroy()` 메서드로 소멸 지원  

- 스프링 전용 인터페이스 
- 요즘은 잘 사용하지 않는 방법

<br/>

**2. @Bean 등록시 옵션**    
- `@Bean(initMethod = "초기화 메서드명", destroyMethod = "종료 메서드명")`

```java  
public class Sample {

	public void init() {
		// 초기화에 필요한 로직 구현
	}
    
	public void close() {
		// 빈소멸전 필요한 로직 구현   
	}
}

@Configuration
static class LifeCycleConfig {

	@Bean(initMethod = "init", destroyMethod = "close")
	public Sample sample() {
 		return new Sample();
 	}
}
```

<br/>

**3. annotation(@PostConstruct, @PreDestroy)**  
- Java 9 이상에서는 deprecated 되어 별도의 dependency 를 추가하여 사용해야함  

```
<dependency>
    <groupId>javax.annotation</groupId>
    <artifactId>javax.annotation-api</artifactId>
</dependency>  
```

```java  
public class Sample {
    
	@PostConstruct
 	public void init() { 	 	
 	 	// 초기화에 필요한 로직 구현  
 	}
    
 	@PreDestroy
 	public void close() {
 	 	// 빈소멸전 필요한 로직 구현
 	}
}
```

<br/><br/>
  
### 참고) `Singleton`       
- Spring Container의 시작과 종료까지 `단 하나의 객체`만 사용하는 방식   
  - 생성자가 여러 차례 호출되더라도 `실제로 생성`되는 객체는 하나
  - 이후에 호출된 생성자는 `최초의 생성자가 생성한 객체`를 리턴   

<br/><br/>

Reference  
https://www.inflearn.com/course/spring_revised_edition    
https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81-%EC%9E%85%EB%AC%B8-%EC%8A%A4%ED%94%84%EB%A7%81%EB%B6%80%ED%8A%B8#curriculum   
https://www.youtube.com/watch?v=3gURJvJw_T4&ab_channel=%EC%9A%B0%EC%95%84%ED%95%9CTech    
https://atoz-develop.tistory.com/entry/Spring-%EC%8A%A4%ED%94%84%EB%A7%81-%EB%B9%88Bean%EC%9D%98-%EA%B0%9C%EB%85%90%EA%B3%BC-%EC%83%9D%EC%84%B1-%EC%9B%90%EB%A6%AC      
https://dongmin1994.tistory.com/15      
https://takoyummy.tistory.com/91    
https://chung-develop.tistory.com/55  
https://jddng.tistory.com/197     
https://alkhwa-113.tistory.com/entry/%EC%8A%A4%ED%94%84%EB%A7%81-%EB%B9%88%EC%9D%98-%EC%83%9D%EB%AA%85%EC%A3%BC%EA%B8%B0%EC%99%80-%EC%B4%88%EA%B8%B0%ED%99%94-%EB%B6%84%EB%A6%AC  
<br/>
