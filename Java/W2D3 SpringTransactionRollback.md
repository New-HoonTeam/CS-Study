## Spring Transaction Roll back  

### Transaction이란?  
여러 쿼리를 논리적으로 하나의 작업으로 묶어주는 것    

<br/>  

### Spring Transaction abstraction    

- JDBC, JPA, Hibernate 트랜잭션의 `실제 하는 일` 동일     
  => 추상화 가능! 
  
  - 트랜잭션을 가져온다 + 생성한다
  - commit 한다
  - rollback 한다 

<br/>  

- 가져오는 트랙잭션의 객체만 다름  
  - JDBC(Connection), JPA(EntityManager), Hibernate(Session)   

<br/>

==> Spring   

트랜잭션 기술의 공통점을 담은 `PlatformTransactionManager` interface 통해 각 구현체들이 `트랜잭션을 가져오는 방식`을 추상화       
  
=> 데이터 접근기술에 관계없이 `Transaction Manager`에서 트랜잭션 가져옴 = 특정 기술에 종속되지 않음     

<br/>   
  
![제목 없음](https://user-images.githubusercontent.com/103614357/207217555-e4c17f52-3dcc-48a3-ab98-311251d6c012.png)   

<br/>
  
```
package org.springframework.transaction;

public interface PlatformTransactionManager extends TransactionManager {
    
      TransactionStatus getTransaction(@Nullable TransactionDefinition definition)throws TransactionException;  
      void commit(TransactionStatus status) throws TransactionException;
      void rollback(TransactionStatus status) throws TransactionException;
}
```

<br/><br/>   

### 스프링이 추상화한 트랜잭션에서의 롤백대상, 모든 예외 및 에러를 롤백하는가?  
No!  
- `Runtime Exception` 또는 `Error`가 발생한 경우 롤백     
- Checked Exception에 대해서는 롤백되지 않음   
  - 스프링은 왜 Checked Exception을 Default로 설정하지 않았을까?  
    - Checked Excpetion은 개발자가 직접 처리할 수 있기 때문   
    - 개발자가 컴파일 시 예측/처리 가능한 예외의 처리를 개발자에게 강제하기 위해   
    - 개발자가 예외 처리를 제대로 하지 않아서 발생한 문제로 간주  

<br/>

- 참고) 아래 두개는 동일한 코드  

```
@Transactional

@Transactional(rollbackFor = {RuntimeException.class, Error.class})
```
- 모든 예외에 대해서 전부 트랜잭션을 롤백하고 싶다  

```
@Transactional(rollbackFor = {Exception.class})
```

<br/><br/>

Reference  

https://www.youtube.com/watch?v=_WkMhytqoCc&ab_channel=%EB%B0%B1%EA%B8%B0%EC%84%A0   
https://www.youtube.com/watch?v=cc4M-GS9DoY&ab_channel=%EC%9A%B0%EC%95%84%ED%95%9CTech    
https://www.youtube.com/watch?v=e9PC0sroCzc&ab_channel=%EC%9A%B0%EC%95%84%ED%95%9CTech   
https://pjh3749.tistory.com/269  
https://developer-talk.tistory.com/418    
https://jangjjolkit.tistory.com/m/3    
https://cotak.tistory.com/256  

<br/>
