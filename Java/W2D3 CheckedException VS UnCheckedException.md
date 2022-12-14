### 자바는 문제를 알리는 throwable 타입으로 세가지를 제공한다.

![](https://velog.velcdn.com/images/goseungwon/post/8e91ffa2-f42c-46dc-a71d-525c46058ddc/image.png)


1. CheckedException
2. RuntimeException
3. Error

Throwable 클래스를 열어보자

![](https://velog.velcdn.com/images/goseungwon/post/dca1bba0-7a49-45dd-baf8-3e5a54ce6a3b/image.png)


요약하자면 throwable 클래스는 자바의 모든 오류와 예외의 슈퍼클래스이다.

JVM 또는 하위클래스의 throw에 의해 던져질 수 있다.

마찬가지로 catch절의 인자는 이 클래스 또는 하위클래스만 될 수 있다.

에러와 RuntimeException의 하위 클래스를 제외한 Exception은 CheckedException으로 간주한다.

**정리하면**

**UnChekcedEception → RuntimeException + Error**

**CheckedException → 나머지**

**CheckedException**

- Exception 하위 클래스중 RuntimeException을 제외한 모든 Exception
- 컴파일러가 예외처리를 확인하기 때문에 반드시 에러를 처리해야 한다 (try/catch, throw)
- 예외 발생시 트랜잭션을 rollback하지 않는다.

**UnCheckedException**

- RumtimeException의 하위 클래스
- 에러 처리를 강제하지 않는다.
- 예외 발생시 트랜잭션을 rollback한다.

### 예외를 적절히 사용하는법

호출하는 쪽에서 복구를 할 것이라 여겨지면 **CheckedException**을 사용하라.

- 이것이 **CheckedException**, **UnCheckedException** 를 구분하는 기본 규칙이다.
- **CheckedException**을 throw 하면 호출자가 catch로 처리하거나 호출자의 호출자에게 throw하도록 강제하기 때문에, **CheckedException**을 회복하라고 던져주는 것과 같다.
- 예외를 잡고 별다른 조취를 취하지 않는 것은 좋지 않은 생각이다.

**UnCheckedException**은 보통 잡을 필요가 없거나, 보통은 잡지 말아야 한다.

- 프로그램이 UnCheckedException을 던진 것은 복구가 불가능하거나 더 실행해서 좋을것이 없는 경우이다.
- 프로그래밍 오류를 나타낼때 사용한다.
    - 크기가 4인 배열이 있다면 index는 0~3이어야 하는데, ArrayIndexOutOfBoundsException이 발생했을 경우는 프로그램 입장에서 복구가 가능한지 구분할 수 없다.
    - 따라서 복구 가능성은 개발자의 판단에 달려있다. 복구가 가능하다 생각하면 checkedException을, 그렇지 않다고 생각하면 RuntimeException을 던져주자.

### 추가적으로 필요없는 **CheckedException**의 남발을 조심하자.

- 예외처리를 상세하게 하면 안정성이 높아진다.
- 예외를 과하게 사용하면 쓰기 불편한 프로그램이 된다.
- 따라서 catch 또는 상위 메서드로 throw를 통해 문제를 전파하게 한다.
- try/catch가 부담스러운 상황에는 optional 또는 if else를 적절히 사용하자.



<br/>

### 참고
---
이펙티브 자바

[자바 예외 구분: Checked Exception, Unchecked Exception](https://madplay.github.io/post/java-checked-unchecked-exceptions)
