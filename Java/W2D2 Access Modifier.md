### 접근 제어자 (Access Modifier)

자바는 접근 제어자를 사용하여 변수나 메서드의 사용 권한(범위)을 설정할 수 있다.

1. private
2. default
3. protected
4. public

접근 제어자는 private → default → protected → public 순으로 보다 많은 접근을 허용한다.

### private

접근 제어자가 private으로 설정되었다면 해당 변수, 메서드는 해당 클래스에서만 접근이 가능하다.

```java
class Sample {
	private String secret;
	private String getSecret() {
		return this.secret;
	}
}

class SampleTest {
	public static void main(String[] args) {
		Sample s = new Sample();
		**System.out.println(s.secret);      // 접근 불가
		System.out.println(s.getSecret()); // 접근 불가**
	}
}
```

위 예제의 secret 변수와 getSecret 메서드는 오직 Sample 클래스에서만 접근이 가능하고 다른 클래스에서는 접근이 불가능하다.

### default

**접근 제어자를 별도로 설정하지 않았다면** 접근 제어자가 없는 변수, 메서드는 default 접근 제어자가 되어 **해당 패키지 내에서만 접근**이 가능하다.

*house/HouseKim.java*

```java
**package house;  // 패키지가 동일하다.**

public class HouseKim {
    String lastname = "kim";  // lastname은 default 접근제어자로 설정된다.
}
```

*house/HousePark.java*

```java
**package house;  // 패키지가 동일하다.**

public class HousePark {
    String lastname = "park";

    public static void main(String[] args) {
        HouseKim kim = new HouseKim();
        System.out.println(kim.lastname);
				**// HouseKim 클래스의 lastname 변수를 사용할 수 있다.**
    }
}
```

```java
kim
```

### protected

접근 제어자가 protected로 설정되었다면 protected가 붙은 변수, 메서드는 **동일 패키지의 클래스** 또는 **해당 클래스를 상속받은 다른 패키지의 클래스**에서만 접근이 가능하다.

*house/HouseKim.java*

```java
**package house;  // 패키지가 서로 다르다.**

public class HouseKim {
	protected String lastname = "kim";
}
```

*house/person/ChangGyuKim.java*

```java
**package house.person;  // 패키지가 서로 다르다.**

import house.HouseKim;

public class *ChangGyuKim* extends HouseKim {  // HouseKim을 상속했다.
	public static void main(String[] args) {
        *ChangGyuKim* kcg = new *ChangGyuKim*();
        System.out.println(kcg.lastname);
				**// 상속한 클래스의 protected 변수는 접근이 가능하다.**
    }
}
```

```
kim
```

*HouseKim* 클래스를 상속한 *ChangGyuKim* 클래스의 패키지는 `house.person`으로 *HouseKim*의 패키지인 `house`와 다르지만 *HouseKim*의 lastname 변수가 protected이기 때문에 `kcg.lastname`과 같은 접근이 가능하다. 만약 lastname의 접근제어자가 protected 가 아닌 default 접근제어자였다면 kcg.lastname 문장은 컴파일 오류가 발생할 것이다.

### public

접근 제어자가 public으로 설정되었다면 public 접근 제어가자 붙은 변수, 메서드는 어떤 클래스에서라도 접근이 가능하다.

```java
package house;

public class HouseKim {
	protected String lastname = "kim";
	public String info = "this is public message.";
}
```

위 예제의 HouseKim의 info 변수는 public 접근 제어자가 붙어 있으므로 어떤 클래스라도 접근이 가능하다.

```java
import house.HouseKim;

public class Sample {
    public static void main(String[] args) {
        HousePark houseKim = new HouseKim();
        System.out.println(houseKim.info);
    }
}
```

```
this is public message.
```

### 클래스, 메서드의 접근 제어자

위 예제는 변수만 대상으로 설명했지만 클래스나 메서드도 마찬가지의 접근 제어자 규칙을 따른다.
접근 제어자를 모두 public으로 설정해도 프로그램을 잘 동작할 것이다.
**하지만 접근 제어자를 이용하면 권한이 없는 임의의 제어를 방지할 수 있다.**

### Item 15. 클래스와 멤버의 접근 권한을 최소화하라.

어설프게 설계된 컴포넌트와 잘 설계된 컴포넌트의 가장 큰 차이는 바로 클래스 내부 데이터와 내부 구현 정보를 외부 컴포넌트로부터 얼마나 잘 숨겼느냐다. 잘 설계된 컴포넌트는 모든 내부 구현을 완벽히 숨겨, 구현과 API를 깔끔히 분리한다. 오직 API를 통해서만 다른 컴포넌트와 소통하며 서로의 내부 동작 방식에는 전혀 개의치 않는다.
정보 은닉, 혹은 캡슐화라고 하는 이 개념은 소프트웨어 설계의 근간이 되는 원리다.

**정보 은닉의 장점**

- 시스템 관리 비용 절감
    - 각 컴포넌트를 더 빨리 파악하여 디버깅할 수 있고, 다른 컴포넌트로 교체하는 부담이 적기 때문이다.
- 성능 최적화에 도움 (성능을 높여준다는 의미는 아니다.)
    - 다른 컴포넌트에 영향을 주지 않고 특정 컴포넌트를 최적화할 수 있다.
- 소프트웨어 재사용성 증대
    - 외부에 거의 의존하지 않고 독자적으로 동작할 수 있는 컴포넌트라면 다른 시스템이나 환경에서 유용하게 쓰일 가능성이 높다.
- 큰 규모의 시스템 개발 난이도를 낮춰준다.
    - 시스템 개발 중에도 언제든지 개별 컴포넌트의 동작을 검증할 수 있기 때문이다.

**정보 은닉의 핵심**

**모든 클래스와 멤버의 접근성을 가능한 한 좁혀야 한다.**
(중략)
한 클래스에서만 사용하는 톱레벨(가장 바깥) 클래스나 인터페이스는 이를 사용하는 클래스 안에 private static으로 중첩시켜보자. 톱레벨로 두면 같은 패키지의 모든 클래스가 접근할 수 있지만, private static으로 중첩시키면 바깥 클래스 하나에서만 접근할 수 있다. (중략) public일 필요가 없는 클래스의 접근 수준을 줄이는 것이 핵심이다.
(중략)
default → protected → public으로 변경할수록 접근할 수 있는 대상의 범위가 엄청나게 넓어진다.
처음 설계시에 적절하게 접근 범위를 설정하고 **반드시 필요한 경우가 아니면 private이 적절**하다. 다른 클래스에서 접근해야할 경우가 생긴다면 **조금씩 범위를 넓혀**주면 되기 때문이다. 그러나, 너무 **자주 권한을 풀어**주는 일이 발생한다면 **해당 컴포넌트를 분리**할 필요가 있는지 고민할 필요가 있다.
(중략)
접근성을 좁하지 못 하는 **제약**이 하나 있긴 하다. 상위 클래스의 메서드를 재정의할 때는 그 접근 수준을 상위 클래스에서보다 좁게 설정할 수 없다는 것이다. 이 제약은 상위 클래스의 인스턴스는 하위 클래스의 인스턴스로 대체해 사용할 수 있어야 한다는 **리스코프 치환 원칙**을 지키기 위해 필요하다.
(중략)
**public 클래스의 인스턴스 필드는 되도록 public이 아니어야 한다.** final이 아닌 인스턴스 필드를 public으로 선언하면 그 필드에 담을 수 있는 값을 제한할 힘을 잃게 된다. **public 가변 필드를 갖는 클래스를 일반적으로 Thread-Safe 하지 않다.**
(중략)

```java
// 보안에 허점이 숨어있는 코드
public static final Thing[] VALUES = { ... };

// 방어적인 코드 (1)
private static final Thing[] PRIVATE_VALUES = { ... };
public static final **List<Thing>** VALUES = 
		Collections.unmodifiableList(Arrays.asList(PRIVATE_VALUES));
}
// 방어적인 코드 (2)
private static final Thing[] PRIVATE_VALUES = { ... };
public static final **Thing[]** values() {
		return PRIVATE_VALUES.clone();
}
```

<aside>
💡 **핵심 정리**

1. 프로그램 요소의 접근성은 가능한 한 최소한으로 하라
2. 꼭 필요한 것만 골라 최소한의 public API를 설계하자
3. 그 외에는 클래스, 인터페이스, 멤버가 의도치 않게 API로 공개되는 일이 없도록 해야 한다.
4. public 클래스는 상수용 public static final 필드 외에는 어떠한 public 필드도 가져서는 안 된다.
5. public static final 필드가 참조하는 객체가 불변인지 확인하라
</aside>

### 알아봐도 좋은 것

- module-info : 서로 다른 프로젝트 A, B에 대해 A에서 선언한 클래스를 B로 불러와 사용할 수 있다.
    - `module`, `exports`, `requires`

```java
module common.widget {
    exports com.logicbig;      // 외부에 노출하고싶은 패키지
}
```

```java
module data.widget {
    requires common.widget;    // 사용할 패키지
    requires java.sql;
}
```

### 참고

- [점프 투 파이썬](https://wikidocs.net/232)

- 이펙티브 자바 (3/E)
