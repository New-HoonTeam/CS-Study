REQUIRED

없으면 새로운 트랜잭션, 있으면 트랜잭션 합류 → default 속성으로 생략 가능

```java
@Transactional(propagation = Propagation.REQUIRED)
public void method() {}
```

![require](https://user-images.githubusercontent.com/86050295/209550310-254af8c1-99ad-4461-be0c-843f459343c1.png)

두 트랜잭션 중에 어느 것 하나라도 롤백되면 모두 롤백

송금 : 출금 → 입금

REQURED_NEW

무조건 새로운 트랜잭션 생성

```java
@Transactional(propagation = Propagation.REQUIRED_NEW)
public void method() {}
```

![require_new](https://user-images.githubusercontent.com/86050295/209550308-11e92108-07c5-4d90-81ae-6b2dfdf87aa0.png)

다른 트랜잭션의 롤백이 서로에 영향을 미치지 않음

NESTED

트랜잭션 없으면, 트랜잭션 만들고, 트랜잭션이 있으면, 중첩 트랜잭션

```java
@Transactional(propagation = Propagation.NESTED)
public void method() {}
```

![nest](https://user-images.githubusercontent.com/86050295/209550312-dead4643-b15b-4cbf-9f2e-75850bbd5592.png)


부모 트랜잭션 롤백되면 모두 롤백

MANDATORY

트랜잭션이 없으면, 예외 발생

```java
@Transactional(propagation = Propagation.MANDATORY)
public void method() {}
```

SUPPORTS

트랜잭션이 있으면 합류하고, 없으면 트랜잭션 없이 실행

```java
@Transactional(propagation = Propagation.SUPPORTS)
public void method() {}
```

NOT_SUPPORTED

트랜잭션이 있으면 그 트랜잭션을 중단하고, 트랜잭션 없이 실행

```java
@Transactional(propagation = Propagation.NOT_SUPPORTED)
public void method() {}
```

NEVER

트랜잭션이 있으면 예외 발생

```java
@Transactional(propagation = Propagation.NEVER)
public void method() {}
```

정리

|  | 의미 | 기존 트랜잭션 없을 시 | 기존 트랜잭션 존재 할시 |
| --- | --- | --- | --- |
| REQUIRED | 트랜잭션 필요 | 새로운 트랜잭션 생성 | 기존 트랜잭션에 합류 |
| REQUIRED_NEW | 새로운 트랜잭션이 필요 | 새로운 트랜잭션 생성 | 새로운 트랜잭션 생성 |
| NESTED | 중첩된 트랜잭션 | 새로운 트랜잭션 생성 | 중첩된 트랜잭션 생성 |
| MANDATORY | 트랜잭션이 의무 | IllegalTransactionStateException
예외 발생 | 기존 트랜잭션 합류 |
| SUPPORTS | 트랜잭션이 있으면 지원 | 트랜잭션 없이 실행 | 트랜잭션에 합류 |
| NOT_SUPPORTED | 트랜잭션을 지원하지 않음 | 트랜잭션 없이 실행 | 트랜잭션 없이 실행 |
| NEVER | 트랜잭션 허용하지 않음 | 트랜잭션 없이 실행 | IllegalTransactionStateException
예외 발생 |

# 참고 자료

[](https://www.baeldung.com/spring-transactional-propagation-isolation)

[면접 셤? 단골문제 JPA Propagation과 Isolation 이해하기](https://devocean.sk.com/blog/techBoardDetail.do?ID=163799)

[[Spring] @Transactional - 1 전파 레벨(propagation)](https://n1tjrgns.tistory.com/266)

[[SPRING] JPA - Propagation](https://velog.io/@french_ruin/SPRING-JPA-Propagation)

[Spring Transaction Propagation을 예제를 통해 알아보자](https://oingdaddy.tistory.com/28)

[(Spring/Spring Boot) transactional annotation 속성 전파 propagation](https://lion-king.tistory.com/entry/SpringSpring-Boot-Transactional-%EC%98%B5%EC%85%98-propagation-isolation)

[Spring Transaction 옵션](https://taetaetae.github.io/2016/10/08/20161008/)
