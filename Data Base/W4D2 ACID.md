## ACID
- `트랜잭션` : 여러 쿼리를 논리적으로 하나의 작업으로 묶어주는 것    
  - 하나의 트랜잭션은 커밋 혹은 롤백된다   
- 트랜잭션이 성공적으로 처리되어 데이터베이스의 무결성과 일관성을 보장하려면 ACID라는 4가지 특성을 만족해야 함   

<br/>     

![제목 없음](https://user-images.githubusercontent.com/103614357/209550858-b91dd034-bae2-4547-b47b-fe6a44d40e16.png)   

- `ACID` : 트랜잭션이 안전하게 수행된다는 것을 보장하기 위한 성질    
  - `Atomicity`
  - `Consistency`
  - `Isolation`
  - `Durability`  

<br/>

### 원자성 Atomicity 
- DB에서의 Atomicity : 트랙잭션은 `모두 반영`되거나, `전혀 반영되지 않아야` 한다(all-or-nothing 방식)     
= 완료되지 않은 트랜잭션의 중간상태가 DB에 반영되서는 안된다       
  - 중간단계까지 실행되고 실패하는 일이 없도록 보장해줌     


- ex) 포인트로 구매할 때   
  - `포인트 차감 + 구매 완료` 업데이트 과정 필요
    - 그런데, 포인트 차감 후 오류 발생하면?   
    - `포인트만 차감`되고 `구매 완료는 되지 않는` 상황 발생할 수 있음
  - 이런 경우가 생기지 않도록 보장하는 성질이 원자성    

<br/>

### 일관성 Consistency
- 트랜잭션 작업처리결과는 항상 일관성 있어야 한다    
  = 데이터베이스는 일관된 상태로 유지되어야 한다     
- DB에 트랜잭션 수행 전, 후 여러 `제약조건에 맞는 상태`를 보장해줌(제약조건을 위배한다면 트랜잭션은 바로 종료됨)

<br/>  
  
- 여기서 말하는 `일관성`이란, 크게 2가지 의미 가짐
  - 1)트랜잭션이 커밋되면 데이터 베이스에 적용한 `제약조건(PRIMARY KEY, UNIQUE, NOT NULL 등)을 위반하지 않는다`는 보장을 의미
    - ex) 사용자가 번호를 저장하려고 하는데, 이 번호에 UNIQUE 제약이 설정되어 있으면 중복되니 사용자 번호를 저장할 수 없어야 함
   
  - 2)트랜잭션의 작업이 애플리케이션에서 `의도하고자 한 작동이 정상적으로 일어난다`는 보장을 의미
    - ex) 재고가 떨어졌을 때, 더 이상 판매를 할 수 없어야함
    - ex) 돈이 없는데 송금 가능하면 안됨

<br/>

### 독립성, 격리성, 고립성 Isolation
- 각각의 트랜잭션은 (서로 간섭없이) `독립적`으로 이루어져야 한다     
  = 둘 이상의 트랜잭션이 동시 실행되고 있을 때, 어떤 트랜잭션도 다른 트랜잭션 연산에 `끼어들 수 없다`          
  = 각 트랜잭션이 유일한 트랜잭션인 것 처럼 `격리`시켜 버리는 것을 의미

![제목 없음](https://user-images.githubusercontent.com/103614357/209526087-b6bb29c8-3f84-4c82-a19f-808bcba07f96.png)   

- ex) 동시성 문제(경쟁 조건)
  - 계좌에 100만원이 있는데, 두개의 디바이스에서 같은 계좌에 동시에 출금하는 경우   
    - phone 1 : 100만원 있음 -> 출금 요청 -> 100만원 출금   
    - phone 2 : 100만원 있음 -> 출금요청 -> 100만원 출금  
    - 200만원 출금!?
- 이러한 상황을 방지해야 한다는 성질이 고립성
  - 먼저 요청한 phone 1에서 출금요청이 완료될때 까지 다른 요청이 관여할 수 없게 한다는 것    
  
<br/>


### 지속성 Durability
- 트랜잭션이 성공적으로 `완료`되었으면 결과는 `영구히 반영`되어야 한다   
- 보통 DBMS에서 이러한 역할은 `트랜잭션 로그`가 해줌
  - 트랜잭션의 결과를 DB에 반영하기 전, 트랜잭션 로그에 먼저 저장을 하는 형태로  
- ex) 한번 송금이 성공하면 은행 시스템에 문제가 생기더라도 송금이 성공한 상태로 `복구`할 수 있어야 함     

<br/><br/>   

### ACID 성질은 트랜잭션이 이론적으로 보장해야하는 성질    
- 실제로는 성능을 위해 손실보장이 완화되기도 함   
  - ACID 원칙을 strict 하게 지키려면 `동시성이 매우 떨어지기` 때문
    - 격리 수준이 높아질 수록 동시성은 떨어지고  
    - 반대로 격리수준이 낮아질 수록 동시성이 높아지는것이 일반적     

- ex) 독립성을 완벽하게 보장하려고 하면, 한 데이터에 여러 연결이 접근하려고 할 시 `순차적으로 진행`    
  (A 트랜잭션 끝날 때까지 기다렸다가 B 트랜잭션 진행, B 끝나면 C 진행...)      
  => `성능` 떨어짐(동시성이 떨어짐)      
- 그렇기 때문에 DB 엔진은 ACID 원칙을 희생하여 동시성을 얻을 수 있는 방법을 제공   
  - 바로 transaction의 `격리 수준(isolation level)`  
    - READ UNCOMMITTED, READ COMMITTED, REPEATABLE READ, SERIALIZABLE  
    - 여러 트랜잭션이 동시에 처리될 때 특정 트랜잭션이 `다른 트랜잭션에서 변경하거나 조회하는 데이터`를 볼 수 있게 허용할지 말지를 결정하는 것
    - ACID중 Isolation(독립성) 원칙을 덜 지키는 level을 사용할수록 문제가 발생할 가능성은 커지지만, 동시에 더 높은 동시성을 얻을 수 있음   

<br/><br/>    

- 최근까지 DB들은 대부분 ACID 보장하기 위해 `lock`에 의존했음   
  - lock 종류    
    - `공유락(Share lock = read lock)` : 데이터를 `읽을 때` 사용되는 락(select for update문), 공유락끼리(= 데이터 읽기 트랜잭션에서) 접근가능   
    - `베타 락(Exclusive lock = write lock)` : 데이터 `변경할 때` 사용, 해당 트랜잭션이 완료될 때까지 어떤 트랜잭션도 진행할 수 없음   
  
=> 많은 수의 락은 관리하게 되면 동시작업 수행이 어렵고 `성능저하` 초래  

<br/><br/>  

- 락의 대안으로, 수정되는 모든 데이터를 별도 복사본으로 관리하는 `MVCC(Multi-Version Concurrency Control)`      
  - `오래된 버전`과 `새로운 버전`의 데이터를 구별하여 저장 -> 접근시점에 따라 데이터 제공      
  - `snapshot`을 이용하여 locking 불필요 -> 일반적인 RDBMS보다 `빠른 동작` 가능     
  - ex) A가 트랜잭션 시작할 때 가지고 있던 복사본을 B에 제공하여 동시에 수행 가능

<br/><br/>   

## 참고) MySQL은 MVCC?     

- 일반적으로 MySQL은 MVCC라고 함(Consistent Non-Locking Reads)   
  - 하지만 조금 더 정확히하면 isolation level에 따라 다름
    -  `read commited`와 `repeatable read`는 MVCC
    - `read uncommitted`와 `serializable`은 MVCC가 아님
- MVCC의 목적(쉽게 설명하면) : read lock을 쓰지 않음으로써, 서로 다른 트랜잭션들이 같은 tuple에 대해서 read/write or write/read 하더라도 서로를 block 하지 않도록 하는 것 

<br/><br/>

### MySQL에서 insert, update, delete     

- MySQL에서 `insert, update, delete` 같이 DB를 변경(write)하는 쿼리 문을 실행하면       
  기본적으로 해당 tuple에 대해서 `write lock을 먼저 획득`한 뒤에 해당 쿼리 문을 수행  
  
<br/><br/>

### MySQL에서 select       

**Consistent Non-Locking Read**     
- `read commited` : `각 SELECT 문마다` `Snapshot 이 초기화`되기 때문에 다른 트랜잭션의 커밋에 의해 해당 데이터가 변경되면 같은 읽기 작업이라도 다른 결과를 반환할 수 있음
- `repeatable read` : `최초 SELECT 문이 수행`된 시점을 기준으로 `Snapshot 이 생성`되어 다른 트랜잭션에 의해 해당 데이터가 변경되어도 `Undo Log 에 저장된 내용을 기반`으로 기존 데이터를 재구성

=> read commited와 repeatable read는 MVCC라고 말할 수 있음    
  
<br/>   

- `기본적인 SELECT 문`을 통해 조회한 데이터는 기본적으로 `Non-Locking Read`           
  - 때문에 다른 트랜잭션에 의해 변경될 가능성이 높음
  - InnoDB는 이를 해결하기 위해 두 가지 `Locking Read` 제공  
  
<br/>

**Locking Read**   
- `SELECT … FOR SHARE` : select를 할 때 `read lock을 먼저 획득`하도록 하는 쿼리문 = 읽은 Row에 S-Lock 설정 
  - 다른 트랜잭션이 Row를 `읽을 순 있지만`, S-Lock 을 설정한 트랜잭션이 커밋되기 전까지 Row 를 `수정할 수 없음`
  - 아직 커밋되지 않은 다른 트랜잭션에 의해 해당 Row 가 변경될 경우, 트랜잭션이 종료될 때까지 기다린 뒤 가장 최신화된 값을 사용하여 쿼리를 수행
- `SELECT … FOR UPDATE` : `Row` 및 관련 `인덱스`에 `Lock 설정`
  - 다른 트랜잭션은 해당 Row 를 `UPDATE` 또는 `SELECT ... FOR SHARE` 수행하는 것이 `제한`됨
- FOR SHARE 혹은 FOR UPDATE 로 설정된 모든 Lock 은 `트랜잭션이 커밋 또는 롤백될 때 반환`     

<br/>
   
- MySQL이 `serializable 레벨`일 때는 평범한 `select문`을 사용해도 `SELECT .. FOR SHARE`로 암묵적으로 바꿔서 처리        
==> MySQL에서 serializable 레벨은 MVCC가 아닌 `lock으로 동작`     
  - MySQL에서 serializable 레벨은 평범한 select문에서도 `read lock`을 획득하게끔 만듬    
    => 결과적으로 `read/write 모두 lock 사용`       
    -> 이 레벨은 MVCC로 동작한다고 보기 어려움               

<br/><br/>  

Reference   
https://www.youtube.com/watch?v=e9PC0sroCzc&ab_channel=%EC%9A%B0%EC%95%84%ED%95%9CTech       
https://www.youtube.com/@ez.      
https://jaeseongdev.github.io/development/2021/06/16/%ED%8A%B8%EB%9E%9C%EC%9E%AD%EC%85%98%EC%9D%98-%ED%8A%B9%EC%A7%95-(ACID)/       
https://continuetochallenge.tistory.com/127        
https://dlgnlfus.tistory.com/261       
https://easy-code-yo.tistory.com/27      
https://tecoble.techcourse.co.kr/post/2022-11-07-mysql-isolation/     
   
<br/>
