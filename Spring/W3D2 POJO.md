## POJO(Plain Old Java Object)     
> <토비의 스프링>        
`객체 지향`적인 원리에 충실하면서     
환경과 기술에 `종속되지 않고`    
필요에 따라 `재활용`될 수 있는 방식으로 설계된 오브젝트    

<br/>   

```java
public class MyPojo {
    private String name;
    private int age;
    
    public String getName() {
    	return name;
    }
    public String getAge() {
    	return age;
    }
    public void setName(String name) {
    	this.name = name;
    }
    public void setAge(int age) {
    	this.age = age;
    }
}
```    

- 다른 class나 interface를 extends/implements 받아 method가 추가된 클래스가 아닌 일반적으로 우리가 알고 있는 getter, setter 같이 기본적인 기능만 가진 자바 객체(VO)      

<br/>

- 참고) `POJO`와 `Bean`           
  - POJO 클래스와 Bean은 둘 다 `가독성과 재사용성`을 높이기 위해 Java 객체를 정의하는 데 사용    
  - 차이 : POJO에는 다른 제한 사항이 없지만 Bean은 몇 가지 `제한 사항`이 있는 특수 POJO(= 몇개의 컨벤션을 적용한 POJO)     
    - Bean - 인수가 없는 생성자가 있어야 함, 필드 접근제어자는 private이어야함 등의 제한 사항이 있음   

<br/><br/>

### POJO의 조건  

**1. 특정 규약에 종속되지 않는다.**   

- 자바언어와 꼭 필요한 API외에는 종속되지 말아야함  
- ex) EJB2와 같이 특정 클래스를 상속하도록 요구하는 등의 특정 규약을 따라 만들게 하는 경우처럼 되면 안됨       
  - 자바의 단일 상속 제한으로 더이상 객체지향적인 설계 적용 어려워짐    

 <br/>

**2. 특정 환경에 종속되지 않는다.**     

- ex) 비지니스 로직을 담고 있는 POJO 클래스는 `웹이라는 환경` 정보나 웹 기술을 담고 있는 클래스나 인터페이스를 사용해서는 안됨   
  - 나중에는 웹 컨트롤러와 연결되서 사용될 것이 분명하더라도, 직접적으로 웹이라는 환경으로 제한해버리는 오브젝트나 API에 의존해서는 안됨     
  - 그렇게 되면 웹 외의 클라이언트가 사용하지 못하기 때문          
- 때문에 비지니스 로직을 담은 코드에 HTTPServletRequest, HttpSession, 캐시에 관련된 API가 등장한다면 진정한 POJO라고 할 수 없음   

<br/>

**3. 객체 지향적 원리에 충실해야한다.**   

- POJO는 객체지향적인 `자바언어의 기본`에 충실하게 만들어져야함 
  - 자바 언어 문법을 사용했다고해서, 자동적으로 객체지향 프로그래밍과 객체지향 설계가 적용되었다고 볼 수는 없음    
  - `SOLID 원칙` 참고  
- 책임과 역할이 각기 다른 코드를 한 클래스에 몰아넣어 덩치큰 만능 클래스를 만들고       
  상속과 다형성의 적용이 아닌 if/switch문으로 가득 설계된 오브젝트라면 POJO라고 부르기 힘듬     

<br/><br/>

### 왜 POJO를 지향해야 할까?  
- Spring Framework 이전에는 원하는 엔터프라이즈 기술이 있다면, 그 기술을 직접적으로 사용하는 객체 설계였음      
  - 이로 인한 문제점 : `특정 기술과 환경에 종속`되어 의존하게 된 자바 코드가 생김   
    - 가독성이 떨어져 `유지보수`에 어려움
    - 특정 기술의 클래스를 상속받거나, 직접 의존하게 되어 `확장성이 떨어짐`     
- 자바가 객체지향 설계의 장점들을 잃어버리게 된 것!   

=> 그래서 `본래 자바의 장점`을 살리는 `POJO`라는 개념 등장       

<br/>

특정 환경이나 기술에 종속적이지 않으니까~    
- 재사용 가능   
- 확장 가능한 유연한 코드 작성   
- 테스트 단순해짐
- 객체지향적인 설계를 제한없이 적용할 수 있음   

<br/><br/>

### POJO framework  
`POJO에` 애플리케이션의 `핵심로직과 기능을 담아` 설계하고 개발하는 방법이 POJO 프로그래밍     
POJO 프로그래밍이 가능하도록 기술적인 기반을 제공하는 프레임워크        
- 대표적인 POJO framework : `Spring Framework`와 `Hibernate`     
- Spring을 이용하면 POJO 프로그래밍의 장점을 그대로 살려서    
  엔터프라이즈 애플리케이션의 `핵심로직을 객체지향적인 POJO를 기반`으로 깔끔하게 구현하고,   
  동시에 엔터프라이즈 환경의 각종 서비스와 기술적인 필요를 POJO방식으로 만들어진 코드에 적용할 수 있음   

<br/><br/>  

## Spring Framework에서 POJO는 무엇이 될 수 있을까?     
- Spring은 POJO 방식을 기반으로 한 Web Framework    
- Spring에서는 `Domain과 Business Logic을 수행하는 대상`이 POJO 대상  
- ex) VO, DTO

<br/>
  
=> 개발자가 POJO 프레임워크를 사용한다고 해서 자동으로 객체지향적인 코드를 짜는 것은 아님   
객체지향적 코드가 가능한 기반에서 어떻게 효과적으로 객체지향적 설계를 잘 할지는 `개발자의 몫`!  

![제목 없음](https://user-images.githubusercontent.com/103614357/208030409-2cc7fd4b-1316-4307-811b-10240241d9a8.png)    
   
- POJO를 `Ioc/DI`, `AOP`, `PSA`를 통해 달성할 수 있음  
  - IoC와 DI를 통해 필요한 클래스를 직접 참조하지 않고 느슨한 결합을 할 수 있음  
  - AOP를 통해 부가기능의 코드를 분리함으로써 핵심로직만 담긴 POJO로서 객체를 유지할 수 있음   
  - PSA(Portable Service Abstraction) : 어떤 서비스를 이용하기 위한 접근 방식을 일관된 방식으로 유지         

<br/>

### 특정 기술에 종속적이면 POJO 아니라면서, Spring에서 Hibernate 기술 어떻게 사용?! (Spring의 PSA)       
- `POJO를 유지`하면서 `특정기술 사용`하는 경우 - Hibernate    
- Spring에서 정한 표준 인터페이스가 있기 때문에 가능 
  - Spring에서는 `ORM 기술`을 사용하기 위해 `JPA라는 표준 인터페이스`를 정해두었음!   
    (이러한 방법이 위에서 말한 Spring의 PSA)          
  - 여러 ORM framework들은 JPA 표준 인터페이스 아래 구현되어 실행됨   
- 만약, 자바 객체가 ORM 기술을 사용하기 위해서 Hibernate framework를 직접 의존하는 순간! POJO라고 할 수 없음!      

<br/><br/>

Reference     

토비의 스프링3(이일민 저)     
https://www.youtube.com/watch?v=oqPiEc2zNb0&ab_channel=CodingwithJohn      
https://www.geeksforgeeks.org/pojo-vs-java-beans/     
https://m.blog.naver.com/seek316/222117086127     
https://doing7.tistory.com/81       
https://siyoon210.tistory.com/120     
https://dev-coco.tistory.com/82   

<br/>
