# 동일성(Identity)

- 동일성은 비교 대상의 두 객체의 메모리 주소가 같음을 의미한다.
- `==` 연산자를 사용한다.

primitive 타입은 객체가 아니라 주소를 저장하고 있지 않기 때문에
`==` 연산자를 사용하였을 때, 값이 같으면 동일하다고 말한다.

![Untitled1](https://user-images.githubusercontent.com/86050295/207330313-defe7f84-0f2b-4a2b-aa9f-2617ae3c7661.png)

# 동등성(Equality)

- 두 객체가 논리적으로 동일한 값을 가지고 있음을 의미한다.
- `Object.equals()` 메서드를 `override`해서 논리적 동일함을 정의해서 사용한다.
- `equals()`와 함께 `hashCode()`를 `override` 해야 한다.

(`equals()`와 `hashCode()`를 override해주지 않으면 주소값을 이용한다)

## `Object.equals()`

![Untitled2](https://user-images.githubusercontent.com/86050295/207330745-c6ee7f0a-d804-4e24-86af-ddc2639343d5.png)



```java
public class Phone {
    private String ap;
    private String os;
    private String color;

    Phone(String ap, String os, String color) {
        this.ap = ap;
        this.os = os;
        this.color = color;
    }

    @Override
    public boolean equals(Object obj) {
        if (this == obj) {
            return true;
        }
        if (obj == null || obj.getClass() != getClass()) {
            return false;
        }
        Phone target = (Phone) obj;

        if (ap.equals(target.getAp()) && os.equals(target.getOs())) {
            return true;
        }
        return false;
    }

    public String getAp() {
        return ap;
    }

    public String getOs() {
        return os;
    }
}
```

```java
public class Main {
    public static void main(String[] args) {
        Phone iPhone1 = new Phone("a15", "ios16", "blue");
        Phone iPhone2 = new Phone("a15", "ios16", "blue");

        System.out.println(iPhone1 == iPhone2);
        System.out.println(iPhone1.equals(iPhone2));
    }
}
```
![Untitled3](https://user-images.githubusercontent.com/86050295/207331302-8a34cf89-e317-4da2-88d2-45d2893e8de0.png)


## `Object.hashCode()`

![Untitled4](https://user-images.githubusercontent.com/86050295/207331914-aa940b51-76c7-4d6b-8fbf-ab1c0d14720f.png)
![Untitled5](https://user-images.githubusercontent.com/86050295/207332130-fac56a43-9ba8-42a5-9b3e-b6b6c0a8f37b.png)




동등한 객체라면 같은 정수값이 반환 되도록 override 해야 한다.

왜 동등한 객체 hashCode를 override 해주어야 하는가?

`HashTable`, `HashSet`, `HashMap` 등 `hashCode()` 값을 이용해 객체의 동등성을 인식하는
Collection들의 경우 다른 `hashCode()` 값이 반환될 경우 `equals()` 가 `true`여도 
동등하지 않은 객체로 인식한다.

<br>

### `hashCode()`를 override 해주지 않을때

```java
import java.util.HashSet;
import java.util.Set;

public class Main {
    public static void main(String[] args) {
        Phone iPhone1 = new Phone("a15", "ios16", "blue");
        Phone iPhone2 = new Phone("a15", "ios16", "blue");
  
        Set<Phone> phoneSet = new HashSet<>();

        phoneSet.add(iPhone1);
        phoneSet.add(iPhone2);

        System.out.println(phoneSet.size());
    }
}
```

![Untitled6](https://user-images.githubusercontent.com/86050295/207332257-ba636900-24c1-4ca9-87ac-c231dfd278a0.png)

<br>


### `hashCode()`를 override 해주었때

```java
package com.example.account;

import java.util.Objects;

public class Phone {
   .
   .
   .
    @Override
    public boolean equals(Object obj) {}

    @Override
    public int hashCode() {
        return Objects.hash(ap + os);
    }
   .
   .
   .
}
```

```java
public class Main {
    public static void main(String[] args) {
        Phone iPhone1 = new Phone("a15", "ios16", "blue");
        Phone iPhone2 = new Phone("a15", "ios16", "blue");

        Set<Phone> phoneSet = new HashSet<>();

        phoneSet.add(iPhone1);
        phoneSet.add(iPhone2);

        System.out.println(phoneSet.size());
    }
}
```

![Untitled7](https://user-images.githubusercontent.com/86050295/207332377-e50d4ec0-1c26-4c88-9b2c-4c759732ef46.png)


# 참고

[동일성(Identity) vs 동등성(Equality) - feat. equals() hashCode()](https://creampuffy.tistory.com/140)

[동일성(Identity)과 동등성(Equality)](https://hudi.blog/identity-vs-equality/)

[[Java] 동일성(Identity)와 동등성(Equality), 그리고 hashCode와 equals](https://limdevbasic.tistory.com/24)

[Log4Jae](https://jhyonhyon.tistory.com/37)

[](https://www.baeldung.com/java-hashcode)

[Use Objects.hash() or own hashCode() implementation?](https://stackoverflow.com/questions/45832458/use-objects-hash-or-own-hashcode-implementation)
