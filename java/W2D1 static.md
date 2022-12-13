# static의 의미

프로그래밍이 시작되는 시점에 클래스 로더가 클래스를 해석하여 
메서드 영역 혹은 힙 영역에 클래스 메타 데이터 및 정적 변수 등을 적재한다.

static이라는 의미는 ‘정적인, 고정된’이라는 뜻을 가지고 있다. 
메모리에서 고정되기 때문에 붙은 이름이지만, 
static을 사용한다는 의미는 모든 객체가 ‘공유’한다는 의미이다.

**static 변수 혹은 메서드는 인스턴스를 생성하지 않아도 사용할 수 있다.**

```java
public class StaticEx {
    public static String staticVariable = "it's class variable";
    public String instanceVariable = "it's instance variable";

    public static void staticMethod() {
        System.out.println("It's static method");
    }
}
```

```java
public class Main {
    public static void main(String[] args) {
        System.out.println(StaticEx.staticVariable);
        StaticEx.staticMethod();
    }
}
```

# 주의 사항

## **static method에서는 instance 변수, instance method를 사용할 수 없다.**

```java

public class StaticEx {
    public static String staticVariable = "it's class variable";
    public String instanceVariable = "it's instance variable";

    public void instanceMethod() {
        System.out.println("It's instance method");
    }
    
    public static void staticMethod() {
        System.out.println("It's static method");
        instanceMethod();
        System.out.println(instanceVariable);
    }
}
```


![](https://user-images.githubusercontent.com/86050295/207317720-b0c91cc3-03b1-4f1e-9240-6e6699b664ed.png)

![](https://user-images.githubusercontent.com/86050295/207317283-b934f858-f7f7-4171-8e6a-fea698e42484.png)




static이 붙은 멤버변수(클래스변수)는 
클래스가 메모리에 올라갈 때 이미 자동적으로 생성되는데,

반면에 인스턴스 변수는 인스턴스를 생성해야만 존재가 가능하다.
인스턴스가 생성이 항상 보장 되지 않음으로 static이 붙은 메소드에서 
인스턴스 변수 사용을 허용하지 않는다.

반대로, 인스턴스 변수나 인스턴스 함수에서는 static이 붙은 멤버들을 사용하는 것이 가능하다. 
인스턴스 변수가 존재한다는 것은 static이 붙은 변수가 이미 
메모리에 존재한다는 것을 의미하기 때문이다.

## static은 속도는 빠르지만, 프로그램이 끝날 때까지 메모리에 존재하기 때문에 주의 해야한다.


static을 클래스 변수라고 말하듯이 static이 붙은 변수는 클래스와 같은 영역에 생기고, 
클래스와 동일하게 메모리에서 항상 상주하게 된다. 
이 때문에 static으로 어떤 데이터나 객체를 연결해서 사용하게 되면 메모리 회수가 안된다. 실제로 시스템이 가동되면서 점점 느려진다.


## **동시성 문제**

여러 인스턴스가 공유함으로 여러 인스턴스가 한 번에 접근하여 잘못된 값 저장 될 수 있다.

```java
class StaticClass {
	private static int count = 0;

	public static void setCount {
		count++; -> count = count + 1;
	}
}
```


1. thread 1이 setCount를 호출해서 count값을 증가시키려고 한다. 
2. thread1의 (0 -> 1)변경사항이 count에 적용이 안된 상태에서 다른 thread 2도 setCount를 호출하여
3. thread2가 count를 (0 -> 1)1증가 시킨다. thread 1이 1을 저장한다.
4. 정상적으로 수행되었다면, 2가 되어야 하지만 동시성 문제가 발생하여 1이 저장된다.


## static method는 상속되지 않는다.

클래스 컴파일 되는 시점에 결정되지만, Override의 경우 런타임 시점에 사용될 메서드가 결정되기 때문이다.

# Static 언제 사용하면 좋을까?

## 모**든 인스턴스에 공통적으로 사용해야 변수에 static을 사용한다.**

인스턴스를 생성하게되면 각 인스턴스 변수들을 서로 독립적이기 때문에 
각각 다른 메모리 공간을 차지한다. 하지만 인스턴스들이 공통적으로 같은 값을 
유지해야 하는 경우 static을 붙이면 관리가 편리하고, 메모리 공간도 절약할 수 있다.

```java
public class StaticEx {
    public static final long CONSTANT = 10;
}
```

## **함수 내에서 인스턴스 변수를 사용하지 않는** 유틸성 클래스**, 를 만들 때,**

인스턴스 변수를 필요로 한다면, static method가 될 수 없다.

반대로 인스턴스 변수를 필요로 하지 않는다면, 가능하면 static을 붙이는 것이 좋다. 
함수 호출시간이 짧아지기 때문에 효율이 높아진다.


# Static의 사용예시

![Untitled3](https://user-images.githubusercontent.com/86050295/207319682-53edc02e-a705-4f3b-b737-5ce8f9f52bcb.png)

![Untitled4](https://user-images.githubusercontent.com/86050295/207319802-325a4ccb-4961-4070-91e9-adc5248b0784.png)




[[Java] static이란?](https://steady-coding.tistory.com/603)

[[Java] 자바 static의 의미와 사용법](https://coding-factory.tistory.com/524)

[[java/자바] Static 이란? Static 정리](https://mi-nya.tistory.com/251)

[[Java] static](https://devbox.tistory.com/entry/Java-static)

[static 개념 정리](https://ifcontinue.tistory.com/2)

[[JAVA] 자바 static 의 의미와 사용법](https://vivia2020.tistory.com/60)

[[Java] 자바 static의 의미와 사용법](https://kingofbackend.tistory.com/131)

[[java] 자바 static 란? 사용하는 이유?](https://taewooblog.tistory.com/162)

[[Java] 자바 : static의 개념은 무엇이고 왜 사용할까?](https://codinglevelup.tistory.com/135)
