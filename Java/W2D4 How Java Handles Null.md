# Java에서의 Null을 다루는 방법

# Null

어떤 변수가 특별한 값을 가지지 않는 상태

`public`, `private`과 같은 예약어 중 하나로 비어있는 값을 가짐을 의미함

그러면 초기화 안된 상태라 Null은 다른건가요??

—> 당연하죠!!

```java
String s1;
String s2 = null;
```

초기화가 되지 않은 변수는 메모리에 올라가지 않아서 사용 자체를 할 수없지만,

null로 초기화된 변수는 아무 값도 가지지 않은 빈 객체라서 사용은 할 수 있어요(하지 않는게 좋긴 한데…)

초기화는 했지만, 해당 객체에 의미있는 값을 집어넣지 않을 때 사용.

```java
String s1 = null;
```

참조 타입에만 사용할 수 있으며, 기본 타입에서는 사용할 수 없음

```java
int a1 = null; // 컴파일에러!
```

그렇기 때문에 auto-unboxing 과정에서 에러가 발생할 수도 있음

```java
Integer aa = null;
int bb = a1; // 에러!!
```

## NPE(Null Pointer Exception)

자바에서 null 값을 가진 객체를 참조하려고 할 때 생기는 예외

### 대부분의 NPE 발생 시점

- String 처리할 때
- File IO
- 서버 송수신 처리

### NPE의 문제점

- 너무 광범위한 에러라서 다양한 파싱 에러가 발생함
- 에러 발생 이후 디버깅이 매우 어렵다

# 예방법

# null을 넘기지 말자

```java
public String print(String hello){
	System.out.println(hello);
	return null; // null 리턴은 최대한 지양
}
```

## null인지 비교를 한 번 해보자

```java
String s1 = null;
if(s1 != null)
    System.out.println("s1 is not null");
else
    System.out.println("s1 is null");
```

![Untitled](https://user-images.githubusercontent.com/39071638/208289750-4785d1a8-2177-499f-9432-9f424ffeadcc.png)```java
private static final RowMapper<Customer> customerRowMapper = (resultSet, i) -> {
	var customerName = resultSet.getString("name");
	var customerEmail = resultSet.getString("email");
	var customerId = toUUID(resultSet.getBytes("customer_id"));
	var lastLoginAt = resultSet.getTimestamp("last_login_at") != null ?
	        resultSet.getTimestamp("last_login_at").toLocalDateTime() : null;
	var createdAt = resultSet.getTimestamp("created_at").toLocalDateTime();
	
	return new Customer(customerId, customerName, customerEmail, lastLoginAt, createdAt);
};
```

## String의 경우

### null이 생길 수 있는 객체를 뒤에 넣자

```java
String s1 = null;
```

```java
if(s1.equals("1"))
    System.out.println("same");
else
    System.out.println("not same");
```

![Untitled 1](https://user-images.githubusercontent.com/39071638/208289757-5b3534d5-4f68-4589-aa4f-352001889d27.png)```java
if("1".equals(s1))
    System.out.println("same");
else
    System.out.println("not same");
```

![Untitled 2](https://user-images.githubusercontent.com/39071638/208289768-de5d3b06-7cfb-4493-8a1d-4d985aa89552.png)
## try - catch로 NPE 잡기

```java
String s1 = null;
try{
    System.out.println(s1.toString());
}catch (NullPointerException e) {
    System.out.println("null pointer!");
}
```

![Untitled 3](https://user-images.githubusercontent.com/39071638/208289774-74485de8-b9bb-453e-9426-2a3570279640.png)
# 빈 객체 이용

```java
@Getter
@NoArgsConstructor
public class Student {
    public static Student EMPTY_STUDENT = new Student();

    private int age;
    private String name;
    private long id;
    private Major major;
    
    public static boolean isEmpty(Student student){
        if(EMPTY_STUDENT == student)
            return true;
        return false;
    }
}
```

## Optional 사용

```java
Optional<String> s1 = Optional.empty();
Optional<String> s2 = Optional.of("hello");

if(s1.isPresent())
    System.out.println("haha");
else // s1.isEmpty()
    System.out.println("papa");
System.out.println(s2.get());
```

단, Optional을 남발할 경우 메모리 부하가 커지기 때문에 꼭 NPE가 발새할 것 같은 부분에서만 사용할 것!

[NPE(NullPointerException) 원인과 예방](https://estrella-0707.tistory.com/5)

[[Java] NullPointException 원인, 예방, 해결하기](https://goddaehee.tistory.com/126)
