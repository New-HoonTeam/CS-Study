### JDBC Flow

![JDBC Flow](https://p1dgey.notion.site/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2Fa90d5062-ac4a-4e28-9a12-6cf699e1f959%2FUntitled.png?id=5d3c09f2-3a0b-463b-873b-0fa7dbb96542&table=block&spaceId=b76551b9-9f24-4a91-9bcd-340caa404f60&width=790&userId=&cache=v2)

- DriverManager를 통해서 Connection 객체를 받아온다.
- Connection을 통해서 Statement를 가져온다. (Statement, PreparedStatement, …)
- Statement를 통해서 쿼리를 실행해서 ResultSet을 가져오거나 update를 실핸한다.
- 데이터베이스 커넥션을 종료한다. (statement.close(), connection.close())

### Java 7 이전의 try-catch-finally

사용 후 반납해주어야 하는 자원들은 **Closeable** 인터페이스를 구현하고 있다.
**리소스 사용 후에 close 메서드를 직접 호출**해주어야 했다.
(JdbcTemplate을 사용하지 않는) JDBC 실습을 진행할 때 finally에서 Statement, Connection을 close() 해주거나 catch에서 rollback(), close() 해주어야 했던 것과 같다.

### try-catch-finally 를 사용했을 때의 문제점

- 리소스 반납을 하는 코드가 매번 들어가서 복잡하고 번거로움
- 실수 또는 에러로 리소스 반납을 하지 못 하면 예기치 못한 문제 발생

### try-with-resources

Java 7에서 위와 같은 문제점을 보완하고자 새로 도입한 문법이다.
이 문법은 **AutoCloseable 인터페이스를 구현하고 있는 자원에 대해 적용**이 가능하며,
**매번 자원을 직접 해제**하거나 예기치 못한 종료로 **리소스 반납이 누락되는 것을 방지**해준다.

- **AutoCloseable: void를 반환하는 close 메서드만 정의한 인터페이스**

try-with-resources 사용법은 다음과 같다.

```java
*try*
**(
		AutoCloseable 인터페이스를 구현하고 있는 자원 1;
		AutoCloseable 인터페이스를 구현하고 있는 자원 2;
		...
)**
*{
		// 본문 코드
}
catch
{
		// 예외 처리 코드
}
finally
{
		// 마지막으로 실행할 코드
}*
```

```java
public List<String> findAllName() {
    List<String> names = new ArrayList<>();
    try (
            var connection = DriverManager.getConnection(...);
            var statement = connection.prepareStatement(...);
    ) {
        try (var resultSet = statement.executeQuery()) {
            while (resultSet.next()) {
                var customerName = resultSet.getString(...);
                var customerId = resultSet.getLong(...);
                var createdAt = resultSet.getTimestamp(...).toLocalDateTime();
                names.add(customerName);
            }
        }
    } catch (SQLException e) {
				logger.error("Got error while closing connection", e);
    }
    return names;
}
```

### Q. Closeable이 먼저? AutoCloseable이 먼저?

분명히 Closeable이 먼저 나왔고 위 같은 문제를 해결해고자 AutoCloseable 인터페이스를 도입했을텐데, 구조를 보면 상위 인터페이스가 AutoCloseable이고, 이를 상속한 하위 인터페이스가 Closeable이다.

> Java 개발자들은 먼저 만들어진 Closeable 인터페이스에 부모 인터페이스인 AutoCloseable을 추가함으로써 **하위 호환성**을 가져갈 수 있었다. 그리고 동시에 변경 작업에 대한 수고를 덜었다.
만약 Closeable를 부모로 만들었다면 기존에 만들어 준 클래스들이 모두 Closeable이 아닌 AutoCloseable를 구현하도록 수정이 필요했을 것이다.
> 

### Item 9. try-finally 보다는 try-with-resources를 사용하라

- 가독성
- 안전성
    - 정확하게 리소스를 반납할 뿐 아니라 **문제가 발생했을 때 진단하기에도 용이**
- try-catch-finally로 자원을 반납하는 경우 Error Stack Trace가 누락될 수 있다.
    
    ```java
    public class ChangGyu implements AutoCloseable {
    
        @Override
        public void close() throws Exception {
            System.out.println("close()");
            throw new IllegalStateException();
        }
    
        public void hello() {
            System.out.println("hello()");
            throw new IllegalStateException();
        }
    }
    ```
    
    ```java
    public class Main {
        public static void main(String[] args) throws Exception {
            **ChangGyu kim = null;
    
            try {
                kim = new ChangGyu();
                kim.hello();
            } finally {
                if (kim != null) {
                    kim.close();
                }
            }**
        }
    }
    ```
    
    ```java
    hello()
    close()
    
    Exception in thread "main" java.lang.IllegalStateException
    	at org.example.ChangGyu.close(ChangGyu.java:8)
    	at org.example.Main.main(Main.java:12)
    ```
    
    **hello에서 발생한 에러 트레이스가 제대로 찍히지 않았다.**
    운영 중인 서비스에서 발생한 문제였다면 원인을 파악하는데 상당한 시간이 들었을 수도 있다.
    
    ```java
    public class Main {
        public static void main(String[] args) throws Exception {
            **try (Changgyu kim = new Changgyu()) {
                kim.hello();
            }**
        }
    }
    ```
    
    ```java
    hello()
    Exception in thread "main" close()
    java.lang.IllegalStateException
    	at org.example.ChangGyu.hello(ChangGyu.java:13)
    	at org.example.Main.main(Main.java:6)
    	Suppressed: java.lang.IllegalStateException
    		at org.example.ChangGyu.close(ChangGyu.java:8)
    		at org.example.Main.main(Main.java:5)
    ```
    

### 참고

[[Java] try-with-resources란? try-with-resources 사용법 예시와 try-with-resources를 사용해야 하는 이유](https://mangkyu.tistory.com/217)

- 이펙티브 자바 (3/E)
