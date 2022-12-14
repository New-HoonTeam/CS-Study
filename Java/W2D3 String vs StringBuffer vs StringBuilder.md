Java에서 문자열을 다루는 기본적인 자료형은 String 이다.
**Hello, World**나 **Kim Changgyu** 같은 문자열들은 자바에서 다음과 같이 표현해서 사용할 수 있다.

```java
String greeting = "Hello, World";
String name = "Kim Changgyu";
```

또 `new` 키워드를 사용한 다음과 같이 문자열을 생성, 표현할 수 있다.

```java
String greeting = new String("Hello, World");
String name = new String("Kim Changgyu");
```

<aside>
💡 보통 문자열을 표현할 때에는 가급적으로 **첫 번째 방법(리터럴 표기)을 권장**한다.

첫 번째 방법(리터럴 표기)은 **String Constant Pool** 영역에서 **불변**하게 관리하여 동일한 문자열은 같은 주소를 참조하지만 두 번째 방법(new)은 항상 새로운 String 객체를 만들어낸다.

</aside>

![[https://ifuwanna.tistory.com/221](https://ifuwanna.tistory.com/221)](https://p1dgey.notion.site/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2F2f45ebcb-2523-47d6-9969-68c42ff9db2b%2FUntitled.png?id=fb12ade2-5e46-4fcb-8a3f-abfef7d84c8c&table=block&spaceId=b76551b9-9f24-4a91-9bcd-340caa404f60&width=940&userId=&cache=v2)

이러한 **String**의 **불변성(Immutable)**으로 인해 **변하지 않는 문자열을 자주 읽어들이는 경우**에는 성능에 영향을 주지 않지만 실제 프로그래밍에서 문자열이 **자주 수정되는 경우** 매번 새로운 메모리를 사용하고 사용하지 않는 영역으로 인해 GC 제거 대상이 되어 전체적인 리소스 오버헤드가 발생하게 된다.

이를 해결하기 위해 Java에서는 가변성(Mutable)을 갖는 **StringBuffer**와 **StringBuilder** 클래스를 도입했다.

## StringBuffer / StringBuilder

**문자열의 추가, 수정, 삭제가 빈번하게 발생**하는 경우에 다양한 API(메서드)를 활용해서 **동일 객채 내 문자열 변경이 가능**하다.

```java
StringBuffer sbf = new StringBuffer("Hello");
sbf.append(", World");

StringBuilder sbd = new StringBuilder("Kim");
sbd.append(" Changgyu");

**// 필요 시점에 String 타입으로 변환하여 사용 가능**
**System.out.println(sbd.toString());
System.out.println(sbd.toString());** 
```

### 그런데 동일 객체 내 문자열 변경이 가능한 클래스가 왜 두 개로 나누어질까?

코드를 확인해보면 답을 알 수 있다.

*StringBuffer.java*

![Untitled](https://p1dgey.notion.site/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2Ff696c931-c20a-476d-987f-77c367bfcc8c%2FUntitled.png?id=b75ec915-88e5-4bda-a383-cf8147181155&table=block&spaceId=b76551b9-9f24-4a91-9bcd-340caa404f60&width=1220&userId=&cache=v2)

StringBuilder.java

![Untitled](https://p1dgey.notion.site/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2F4fff90f0-196a-4743-99cf-f8d724139843%2FUntitled.png?id=e1141f67-4030-49d1-9e66-2692614fad47&table=block&spaceId=b76551b9-9f24-4a91-9bcd-340caa404f60&width=1160&userId=&cache=v2)

→ **동기화의 유무**가 가장 큰 차이점이다.

즉, **synchronized** 키워드를 사용하기 때문에 **StringBuffer는 Thread-Safe** 하다는 점이다.
물론 동기화로 인해 처리 속도에서 StringBuilder 보다 뒤쳐질 수 밖에 없기도 하다.
그래서 어느정도 보완하게 위해 **Cache** 기법을 활용한다.

다음의 표를 참고하여 세 개의 문자열 관련 클래스를 비교할 수 있다.

![[https://ifuwanna.tistory.com/221](https://ifuwanna.tistory.com/221)](https://p1dgey.notion.site/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2F324c523f-27f1-4ce5-8540-d9cae2b14b25%2FUntitled.png?id=2a75d8d6-436b-4de3-a2ef-606482cd8aae&table=block&spaceId=b76551b9-9f24-4a91-9bcd-340caa404f60&width=1010&userId=&cache=v2)

[https://ifuwanna.tistory.com/221](https://ifuwanna.tistory.com/221)

- **실제 성능 비교**
    
    ```java
    /**
     * 시간 측정, 결과 출력 클래스
     * @author madplay
     */
    class MadClock {
        private long startTime;
        private long endTime;
    
        public void startClock() {
            startTime = System.nanoTime();
        }
    
        public void stopClock() {
            endTime = System.nanoTime();
        }
    
        public void printResult(String clockName) {
            System.out.printf("%s" + ": %.3f seconds %n",
                    clockName, (endTime - startTime) / (double) 1_000_000_000);
        }
    }
    ```
    
    ```java
    /**
     * 문자열 연산 비교 테스트 클래스
     * @author madplay
     */
    public class StringTest {
        private static final int MAX_LOOP_COUNT = 50_000;
    
        public static void main(String[] args) {
    
            // StringBuilder
            StringBuilder builder = new StringBuilder();
            MadClock builderClock = new MadClock();
            builderClock.startClock();
            for (int loop = 1; loop <= MAX_LOOP_COUNT; loop++) {
                builder.append("mad").append(loop).append("play");
            }
            builderClock.stopClock();
            builderClock.printResult("StringBuilder");
    
            // StringBuffer
            StringBuffer buffer = new StringBuffer();
            MadClock bufferClock = new MadClock();
            bufferClock.startClock();
            for (int loop = 1; loop <= MAX_LOOP_COUNT; loop++) {
                buffer.append("mad").append(loop).append("play");
            }
            bufferClock.stopClock();
            bufferClock.printResult("StringBuffer");
    
            // String
            String str = "";
            MadClock stringClock = new MadClock();
            stringClock.startClock();
            for (int loop = 1; loop <= MAX_LOOP_COUNT; loop++) {
                str += "mad" + loop + "play";
            }
            stringClock.stopClock();
            stringClock.printResult("String");
        }
    }
    ```
    
    
    
    ![https://madplay.github.io/post/difference-between-string-stringbuilder-and-stringbuffer-in-java](https://p1dgey.notion.site/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2F77b66518-538c-4e36-9175-55ede9f728db%2FUntitled.png?id=7149b43b-b898-4353-8ef2-1eae22b9097f&table=block&spaceId=b76551b9-9f24-4a91-9bcd-340caa404f60&width=2000&userId=&cache=v2)
    
    [https://madplay.github.io/post/difference-between-string-stringbuilder-and-stringbuffer-in-java](https://madplay.github.io/post/difference-between-string-stringbuilder-and-stringbuffer-in-java)
    

### String

String의 내장 메서드와 포맷팅 방법은 [점프 투 자바](https://wikidocs.net/205#primitive)를 참고하거나 검색하면 다양한 자료를 얻을 수 있다.

### Multi Line String (Text Blocks)

자바 13? 15 부터 지원하는 Syntax (`””” <문자열> “””`)

```java
public String getBlockOfHtml() {
    return """
            <html>
                <body>
                    <span>example text</span>
                </body>
            </html>""";
}
```

### 참고

[점프 투 파이썬](https://wikidocs.net/205#primitive)

[[Java] String, StringBuffer, StringBuilder 차이 및 장단점](https://ifuwanna.tistory.com/221)

[자바 String, StringBuilder 그리고 StringBuffer 성능 차이 비교](https://madplay.github.io/post/difference-between-string-stringbuilder-and-stringbuffer-in-java)

[](https://www.baeldung.com/java-text-blocks)
