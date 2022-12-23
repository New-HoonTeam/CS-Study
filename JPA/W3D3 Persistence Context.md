## JPA의 Persistence Context
- `영속 엔티티/객체` : 영속성 컨텍스트에 보관중인, DB data에 매핑되는 메모리상의 객체   
- `EntityManager` : Entity 객체 관리  
- `영속성 컨텍스트` : 일종의 메모리 저장소, Entity 저장소  
  - EntityManager를 생성할 때 만들어짐  
  - EntityManager를 통해 접근하고 관리  
  - 영속 컨텍스트를 통해 관리된다 =  `변경을 추적한다`  
  - 삭제할 때도 변경추적됨 : 삭제되었다는 사실을 변경추적 
  - 따라서, 영속 컨텍스트에 있는 `객체를 변경`하게 되면 Transaction commit 시점에 `변경된 내역이 DB에 반영`  

![제목 없음](https://user-images.githubusercontent.com/103614357/208809341-d30cf3a1-fe3a-4463-9aa9-938138c437d2.png)   

<br/><br/>   
 
### 1차 캐시

![제목 없음](https://user-images.githubusercontent.com/103614357/208812080-f35399b2-6f65-46ea-91d4-600ca059e5cc.png)   

- 1차 캐시는 Collection인 `Map 형태`의 자료구조로 `Key와 value 값`을 가지고 있는 영역
  - `Key`는 `식별자 값`으로 @Id이고, `value 값`으로는 `Entity값`이 들어감
- 1차 캐시가 되는 것은 오직 식별자로 쿼리날릴 때만 가능    

<br/><br/> 

### Persistence Context의 이점 5가지     
**1. 1차캐시**    

- 영속성 컨텍스트에서 관리되는 Entity는 `1차캐시에서` 조회 가능(단, id로 조회할 경우만 가능)    

![제목 없음](https://user-images.githubusercontent.com/103614357/208809228-8c53f3d7-9de1-47e7-b90e-2f05265851a3.png)    

- 첫번째 객체를 find 해서 DB에서 가져옴  
- 두번째 객체를 find 해서 DB에서 가져오는데 이 때, 첫번째 객체와 식별자 동일

=> `식별자가 동일`하다면 `동일 객체`로 판단     
&nbsp; SELECT Query가 또 실행되지 않고, `영속성 컨텍스트에 보관된 객체` 조회        
= Repeatable Read 효과   

<br/>

- 참고) DB에는 저장되어 있는데, 1차캐시에 데이터가 없다면? (= 위에서 첫번째 객체 find 할 때)       
  - DB에서 조회해서 가져온 후
  - 1차캐시에 저장 후
  - Entity 반환    

<br/><br/>

**2. 동일성(Identity) 보장**    
- 영속성 컨텍스트(1차 캐시)에서 관리되는 Entity를 가져왔다 = 동일성 보장   
  - Java Collection에서 똑같은 객체를 꺼내는 것과 동일   
  - Java Collection에서 값을 가져 올때 동일한 주소 값을 가져오듯이, `같은 reference`를 불러오면 동일성을 보장    
- 1차 캐시로 `반복가능한 읽기 등급`의 트랜잭션 격리 수준을 DB가 아닌 application 차원에서 제공   

<br/>

- 참고) 트랜잭션 격리 수준에서 반복 읽기(ISOLATION LEVEL : Repeatable Read)   
  - `SELECT`시 명시적인 트랜잭션을 시작하여 공유잠금이 설정되면   
    해당 데이터에 대해 다른 세션에서는 `UPDATE나 DELETE`시 일어나는 `배타적 잠금이 대기 상태`   
    => 트랜잭션 종료시 까지 해당 데이터를 `반복적으로 SELECT시 동일한 데이터` 보장     
    
    = 한 세션의 트랜잭션이 진행 중일 때는 외부에서 데이터의 변경을 차단하고    
    동일한 세션에서는 동일한 데이터를 읽을수 있는 구조가 필요할때 활용 가능한 격리 수준
  - 하지만 외부로부터의 UPDATE에 대한 트랜잭션으로부터 잠금을 획득하는 것,
    데이터의 INSERT시까지 차단되지는 않음  

<br/><br/>

**3. Transaction Write-behind 트랜잭션을 지원하는 쓰기 지연**   
- `쓰기지연`
  - 한 트랜잭션안에서 이루어지는 Update나 Save의 Query를 영속성 컨텍스트 내의 `쓰기 지연 SQL 저장소`에 가지고 있다가   
  - Transaction이 `commit되는 순간` 쓰기 지연 SQL 저장소에 보관된 Query 보냄   

<br/>
  
```java
transaction.begin();

Station staion1 = new Station("사당역");
Station staion1 = new Station("종각역");

em.persist(station1);
em.persist(station2);

//커밋 전까지 Insert Query를 DB에 보내지 않음
//commit() 하는 순간 DB에 Insert Query 보냄

transaction.commit();
```

- `em.persist(station1);` 
  - 쓰기 지연 SQL 저장소에 Insert Query 저장
  - 1차 캐시엔 Entity 저장  
  
![제목 없음](https://user-images.githubusercontent.com/103614357/208831490-01819d53-d9fa-4a3d-823c-d31d69398e6e.png)   

<br/>

- `em.persist(station2);` : 위와 같은 작업  

![제목 없음](https://user-images.githubusercontent.com/103614357/208813311-5e6d7c80-179c-4885-a9d7-e0ce87479540.png)  

<br/>

- `transaction.commit();`
  - 쌓아둔 SQL을 DB에 보내서 결과 반영 

![제목 없음](https://user-images.githubusercontent.com/103614357/208813358-374af7e8-a9f1-4ee6-a1d5-edba4fd116bd.png)  

<br/><br/>

**4. Dirty Checking 변경감지**    
- 위에서 말한 것! 영속 컨텍스트를 통해 관리된다 =  `변경을 추적한다`  
  - 영속성 컨텍스트에서 관리하는 Entity에 변경이 일어났을 경우 이를 감지      
  - 영속성 컨텍스트에 있는 `객체를 변경`하게 되면 Transaction commit 시점에 `변경된 내역이 DB에 반영`  

<br/>

```java
Station station = em.find(Station.class, "사당역");

//4호선 -> 2호선으로 변경
station. setLine("2호선")

transaction.commit();
```   

![제목 없음](https://user-images.githubusercontent.com/103614357/208814284-ab10055e-9577-4108-8bca-43d7eb456f6c.png)   

- update 관련 코드가 필요없음
- Entity 변경되었다면~
  - 데이터가 변경되면 즉시 1차캐시에 반영  
  - 영속성 컨텍스트에서 `스냅샷`과 `1차캐시의 Entity` 비교
  - 다르면 `Update Query 생성`하여 쓰기 지연 저장소에 저장
  - 쌓여있는 쓰기 지연 SQL 저장소의 Query를 DB에 반영     

<br/>    

- 참고) flush가 발생해야 영속성 컨텍스트의 변경내용을 DB에 반영함
  - flush는 단순히 변경내용을 DB에 동기화해줌
    - 영속성 컨텍스트를 비우는건 아님 
  - flush()를 의도적으로 호출한다거나 commit() 하는 순간 flush() 호출됨  

<br/><br/>   

**5. 지연로딩 가능**
- @ManyToOne의 경우 FK쪽의 엔티티를 가져올 때 PK쪽의 엔티티(연관관계에 있는 데이터)도 같이 가져오게 됨
- 그런데, Entity를 조회할 때 연관된 Entity들이 항상 사용되는 것은 아님!  
- 이 때, 연관된 Entity를 자주 사용하지 않는다면, `지연로딩(Lazy Loading)` 사용 가능
- `지연로딩(Lazy Loading)` : `연관관계에 있는 나머지 데이터`는 조회 미룸  
  - Proxy 객체를 사용하여 연관관계에 있는 객체의 초기화를 가능한 지연시킴   
  - Entity가 `연관관계에 있는 해당 Entity의 값을 사용을 할 때` SQL을 날려 해당 데이터 가져옴  
- `즉시로딩(Eager Loading)` : 특정 Entity를 조회할 때 연관된 모든 Entity를 같이 로딩  
  - 단점 : Entity간의 관계가 복잡해질수록 조인으로 인한 성능 저하, N+1 발생이 무분별하게 생길 수 있음  
  - ex) 연관된 Entity가 컬렉션이라면,, 
    - 로딩할 때 비용 많이 듬
    - 잘못하면 너무 많은 데이터 로딩

<br/>

- Lazy Loading의 장점   
  - 불필요한 데이터 참조를 줄일 수 있음 
  - -> 메모리 소비량 감소   
  - -> 초기 로딩시간이 적음   
  
  => 성능 이슈가 발생할 가능성을 줄여줌!  

<br/><br/> 

Reference   
https://www.inflearn.com/course/ORM-JPA-Basic   
https://www.youtube.com/watch?v=tUwg78VkWJ0&ab_channel=%EC%B5%9C%EB%B2%94%EA%B7%A0    
https://www.youtube.com/watch?v=tXyDmqoMmKE&ab_channel=%EB%A9%94%ED%83%80%EC%BD%94%EB%94%A9    
https://www.youtube.com/watch?v=kJexMyaeHDs&ab_channel=%EC%9A%B0%EC%95%84%ED%95%9CTech       
https://jackjeong.tistory.com/116     
https://zzang9ha.tistory.com/347      
https://programmer-chocho.tistory.com/81   

<br/>
