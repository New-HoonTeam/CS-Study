# 강한 결합, 약한 결합

# 강한 결합이란…!

어떤 객채가 다른 객체에게 강하게 의존하고 있음

# 약한 결합!

위와 같이 클래스(구현체)가 아닌 인터페이스(추상체)에 의존을 하면 객체간의 결합이 약해지고, 다른 객체가 필요하더라도 코드의 수정이 **적어짐**(없이질 수도 있음)

약한 결합을 하면 추상체를 멤버 변수로 가지므로 **런타임**시 의존 관계가 결정됩니다!

---

평소의 지갑 상태(강한 결합 상태)

```java
@Getter
public class Wallet {
    private Won won;
    private List<CreditCard> creditCardList;

		public Won consume(Won won){
			if(this.won >= won){
				this.won - won;
			return this.won;
		}
}
```

WWDC가 열려서 미국에 가고 싶어요!!

![Untitled](https://user-images.githubusercontent.com/39071638/208289571-5702f6a6-f47f-4213-b215-857a797a7aa4.png)
```java
public class Wallet {
    private Dollar dollar;
    private List<CreditCard> creditCardList;
}
```

조금 더 유연한 지갑이 필요해…

```java
public interface Money {
	Money consume(Money money);
}

public class Dollar implements Money {
	@Ovrride
	...
}

public class Won implements Money {
	@Override
	...
}
```

```java
@Getter
public class Wallet {
    private final Money money;
    private List<CreditCard> creditCardList;

    public Wallet(Money money) {
        this.money = money;
    }
		...
}
```

이제 지갑을 바꿀 필요 없이(코드의 수정 없이) 원하는 화폐 단위를 사용하도록 설정할 수 있어!! 

위의 강한 결합의 지갑 클래스의 경우 `Wallet` 클래스가 `Won` 클래스에 의존하고 있음

이럴 경우 Won클래스가 필요하지 않거나 대체될 때, 대대적인 코드의 수정이 불가피함

해결방법 : 약한 결합 (인터페이스를 통한 의존)

즉

- 강한 결합을 할 경우 한 객체는 다른 객체에 대해 강한 의존성을 가진다
- 이럴 경우 멤버 변수에 대한 변경이 발생하면 코드의 수정이 잦을 수 있음
    - 유지 보수성이 나빠짐!!
- 위에 대한 해결책이 약한 결합
- 강한 결합과는 다르게 추상체에 의존하므로 유지 보수에 용이함

---

### 그런데… 어디서 많이 본 것 같지 않나요??

- 어디서 봤드라…
    
    SOLID 원칙의 [`OCP`](https://github.com/New-HoonTeam/CS-Study/blob/main/Java/W2D2%20SOLID.md) !!
    
    Spring의 `DI` 방법 중 `생성자주입`!!!
    
    `OCP` : 확장에는 열려있고, 수정에는 닫혀 있어야 한다
    
    기존의 코드를 수정하지 않게 추상체를 이용한다
    
    `생성자주입` : 스프링에서 의존성을 주입하는 방법 중 하나  
    

[[Java] 강한 결합과 약한 결합](https://velog.io/@damiano1027/Java-%EA%B0%95%ED%95%9C-%EA%B2%B0%ED%95%A9%EA%B3%BC-%EC%95%BD%ED%95%9C-%EA%B2%B0%ED%95%A9)
