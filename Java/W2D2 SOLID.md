## SOLID  
5가지의 객체 지향 설계 원칙       

<br/>
  
### SRP(Single Responsibility Principle)  
단일 책임의 원칙    
- 해당 모듈이 여러 대상 또는 액터들에 대해 책임을 가져서는 안되고, 오직 하나의 액터에 대해서만 책임을 져야 한다     
  - SRP 에서 이야기하는 책임이란, `기능` 정도로 생각     
  - 만약 한 클래스가 수행할 수 있는 기능(책임)이 여러 개라면,     
    클래스 내부의 함수끼리 강한 결합을 발생할 가능성   
    
    ```
    A 메소드 결과 기반으로 B 메소드 호출, B메소드 결과 기반으로 C 메소드 호출?    
    -> A 하나 수정하면 B, C까지 다 수정되어야  
    ```  
    
- 변경이 필요할 때 수정할 대상이 명확해짐   
- 적절하게 책임과 관심이 다른 코드를 분리하고, 서로 영향을 주지 않도록 추상화하자   

<br/><br/>

### OCP(Open-Closed Principle  
개방 폐쇄 원칙   
- 확장에는 열려있고, 수정에는 닫혀있어야 한다  
  - 확장에 열려있다 : 새로운 동작을 추가하여 애플리케이션의 기능을 확장할 수 있다  
  - 수정에 닫혀있다 : 기존의 코드를 `수정하지 않고` 애플리케이션의 동작을 추가하거나 변경할 수 있다  

<br/>

- 개방 폐쇄 원칙을 지키기 위해서는 추상화에 의존해야 함  

- 메모리에 저장하는 방식을 사용하다가 DB에 저장하는 방식으로 변경 요청이 온다면?    
  아래의 코드라면 코드수정 불가피  
```
public class UserService {
  private final UserMemoryRepository;
```

```
public class UserService {
  private final UserJdbcRepository;
```
  
- `추상화`
  - 핵심적인 부분만 남기고, 불필요한 부분은 제거함    
    = `변하지 않는 부분은 고정`하고 변하는 부분을 생략    
    = 복잡한 것을 간단히 하는 것     
  - 변경이 필요한 경우에 생략된 부분을 수정하여 개방-폐쇄의 원칙을 지킬 수 있음  

```
public class UserService {
  private final UserRepository;
```

<br/><br/>

### ISP(Interface Segregation Principle)
인터페이스 분리 원칙   
- 클라이언트의 목적과 용도에 적합한 인터페이스만을 제공하는 것       
  = 한 클래스는 자신이 사용하지 않는 인터페이스는 구현하지 않아야 함  
  = 인터페이스는 해당 인터페이스를 사용하는 클라이언트를 기준으로 잘게 분리되어야 함     

<br/><br/>

### LSP(Liskov Substitution Principle)  
리스코프 치환 원칙  
- 하위 타입은 상위 타입을 대체할 수 있어야 한다  
  - 정사각형은 도형이다, 직사각형은 도형이다    
  - 상속관계에서는 꼭 일반화 관계 (IS-A) 가 성립해야 한다는 의미  
    - 결국은, 리스코프 치환 원칙을 지키지 않으면 개방 폐쇄 원칙을 위반하게 되는 것   
    - 상속관계가 아닌 클래스들을 상속관계로 설정하면, 이 원칙이 위배됨(재사용 목적으로 사용하는 경우)  
    - 상속 관계를 잘 정의하여 LSP 원칙이 위배되지 않도록 설계해야  
    - [정사각형은 직사각형이다로 했을 때 올바르지 않은 상속관계의 코드](https://velog.io/@harinnnnn/OOP-%EA%B0%9D%EC%B2%B4%EC%A7%80%ED%96%A5-5%EB%8C%80-%EC%9B%90%EC%B9%99SOLID-%EB%A6%AC%EC%8A%A4%EC%BD%94%ED%94%84-%EC%B9%98%ED%99%98-%EC%9B%90%EC%B9%99-LSP)

<br/><br/>

### DIP(Dependency Inversion Principle)  
의존 역전 원칙  
- 저수준 모듈이 고수준 모듈에서 정의한 추상 타입에 의존해야 한다   
  = 추상화에 의존하며 구체화에는 의존하지 않는 설계 원칙 
  - `고수준 모듈` : 변경이 없는 추상화된 클래스(또는 인터페이스)
  - `저수준 모듈` : 변하기 쉬운 구체 클래스  

- ex) `UserService`는 구체적인 `UserMemoryRepository` or `UserJdbcRepository`에 의존하는게 아니라  
  추상화된 `UserRepository`에 의존하므로 저장 정책이 변경되어도 다른곳들로 변경이 전파되지 않음, 유연한 애플리케이션   
  
- 의존 역전 원칙은 개방 폐쇄 원칙과 밀접한 관련   
  의존 역전 원칙이 위배되면 개방 폐쇄 원칙 역시 위배될 가능성이 높다    
  
<br/>

==> 결론  
- SRP, ISP : 객체가 커지는 것 막아줌  
- DIP, LSP : OCP 서포트(자주 변화되는 부분을 추상화하고 다형성을 이용 -> 기능 확장에는 용이하되 기존 코드의 변화에는 보수적)        
- 핵심은 `추상화`    
- 구체 클래스에 의존하지 않고 추상 클래스(또는 인터페이스)에 의존함으로써 유연하고 확장가능한 애플리케이션을 만들 수 있다!  

<br/><br/>

Reference  
데브코스 홍구님 객체지향 라이브세션  
https://mangkyu.tistory.com/194     
https://velog.io/@haero_kim/SOLID-%EC%9B%90%EC%B9%99-%EC%96%B4%EB%A0%B5%EC%A7%80-%EC%95%8A%EB%8B%A4   
https://velog.io/@harinnnnn/OOP-%EA%B0%9D%EC%B2%B4%EC%A7%80%ED%96%A5-5%EB%8C%80-%EC%9B%90%EC%B9%99SOLID-%EB%A6%AC%EC%8A%A4%EC%BD%94%ED%94%84-%EC%B9%98%ED%99%98-%EC%9B%90%EC%B9%99-LSP     

<br/> 
