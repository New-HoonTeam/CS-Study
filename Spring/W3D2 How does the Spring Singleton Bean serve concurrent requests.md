# WAS Web 요청을 처리하는 방법
![Untitled](https://user-images.githubusercontent.com/86050295/208967924-ed25a2ef-071f-49ba-9bb8-1c4f25ecd70c.png)

1. WAS는 다중 요청을 처리하기 위해서 쓰레드의 컬렉션인 `Thread Pool`을 생성한다.
2. 유저의 요청이 들어오면 요청을 Task Queue에 담고, 
가용한 `Thread Pool`이 있으면 Task를 가용한 Thread에 할당 한다.
3. Thread는 스프링부트에서 작성한 Dispatcher Servlet을 거쳐 유저 요청을 처리한다.
4. 작업을 마친 Thread는 `Thread Pool`로 반환된다.

# JVM Runtime Memory

```java
class Person {
    int id;
    String name;

    public Person(int id, String name) {
        this.id = id;
        this.name = name;
    }
}

public class PersonBuilder {
    private static Person buildPerson(int id, String name) {
        return new Person(id, name);
    }

    public static void main(String[] args) {
        int id = 23;
        String name = "John";
        Person person = null;
        person = buildPerson(id, name);
    }
}
```

![Untitled](https://www.baeldung.com/wp-content/uploads/2018/07/java-heap-stack-diagram.png)
[](https://www.baeldung.com/java-stack-heap#heap-space-in-java)

## Stack Memory

- 선입 후출 구조로 메서드 호출 시 생성되는 지역변수, 매개변수, 연산 중 발생하는 임시 데이터 저장하는 메모리

## Heap Memory

- 생성된 객체들이 저장되는 메모리이다. → 스프링이 Controller를 Singleton Bean으로 등록하면 heap memory에 저장된다.
- lock을 걸지 않는 이상 전역에서 참조 가능하다.

<aside>
💡 단일 Thread 자바 프로그램의 Stack Memory의 여러 참조 변수들이 
Heap Memory에 있는 하나의 `Person` 객체를 참조 하고 있는 것을 확인할 수 있다.

</aside>

# 정리

- Thread는 각자 독립적인 PC Register와 Stack Area를 가진 프로그램 실행 흐름의 단위다.
- Lock이 되어 있지 않으면, Thread에서 동시에 Heap Area에 있는 객체를 참조할 수 있다.
- 따라서 WAS가 받은 다수의 같은 요청을 처리하는 여러 Thread는 Heap에 저장된 하나의 Controller만 참조해서, 각자의 요청을 동시에 처리할 수 있다.
⇒ 따라서 singleton인 Controller를 stateless하게 설계하는 것이 매우 중요하다.

# 예시

게시판 글Post가 3개가 저장되어 있고, `PostController`에는 `postId`에 해당하는 `Post`를 가져오는 `getPost`가 있다. (`PostController`는 SpringBean으로 등록되어 있다)

```java

@Slf4j
@RestController
@RequiredArgsConstructor
@RequestMapping("/api/v1")
public class PostController {
    private final PostFacade postFacade;
    private final PostService postService;

    @PostMapping("/posts/{postId}")
    public ApiResponse<Long> updatePost(
            @PathVariable Long postId,
            @RequestBody @Validated UpdatePostRequest updatePostRequest
    ) {
				log.info(Thread.currentThread().getName() + "------" + postId);
        log.info(String.valueOf(this.getClass()));
        return new ApiResponse<>(postService.updatePost(postId, updatePostRequest));
    }
}
```

```
curl http://localhost:8080/api/v1/posts/1 & curl http://localhost:8080/api/v1/posts/2
```

Post 1과 Post2를 동시에 가져오는 요청을 수행한다.

```
2022-12-22T02:14:56.363+09:00  INFO 42819 --- [nio-8080-exec-2] c.p.s.controller.PostController          : http-nio-8080-exec-2------2
2022-12-22T02:14:56.363+09:00  INFO 42819 --- [nio-8080-exec-2] c.p.s.controller.PostController          : class com.prgms.springbootboardjpa.controller.PostController
2022-12-22T02:14:56.363+09:00  INFO 42819 --- [nio-8080-exec-1] c.p.s.controller.PostController          : http-nio-8080-exec-1------1
2022-12-22T02:14:56.364+09:00  INFO 42819 --- [nio-8080-exec-1] c.p.s.controller.PostController          : class com.prgms.springbootboardjpa.controller.PostController
```

<aside>
💡 thread1, thread2의 서로 다른 쓰레드에서 동일한 PostController를 참조해서, 
각각 다른 Post를 가져오는 것을 확인할 수 있다.

</aside>

[How does singleton bean serve multiple requests at the same time in Spring?](https://medium.com/@hasanli.vusala.73/how-does-singleton-bean-serve-multiple-requests-at-the-same-time-in-spring-f4c9d797dec9)

# 참고

[](https://www.baeldung.com/java-stack-heap#heap-space-in-java)

[](https://www.baeldung.com/spring-singleton-concurrent-requests)

[How does singleton bean serve multiple requests at the same time in Spring?](https://medium.com/@hasanli.vusala.73/how-does-singleton-bean-serve-multiple-requests-at-the-same-time-in-spring-f4c9d797dec9)
