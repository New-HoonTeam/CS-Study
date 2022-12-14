
## 의존성

OOP에서 객체간 협력은 필수적이며, 객체가 협력한다는 것은 의존성이 존재한다는 것이다. 여기서 의존성이란 파라미터나 리턴값 또는 지역변수 등으로 다른 객체를 **참조**하는 것을 의미한다.

예들 들어 Service가 Repository를 사용하고 있을 때 Service객체가 Repository객체에 의존성이 있다고 표현한다.

여기서 두 객체간의 관계를 맺어주는 것을 의존성 주입이라고 한다.

### 의존성 종류

컴파일 의존성 (구체 클래스에 의존)

- 코드를 컴파일하는 시점에 결정되는 의존성
- 클래스 사이의 의존성
- 결합도가 높으며 변경에 유연하지 못함

런타임 의존성 (추상 클래스나 인터페이스에 의존)

- 코드(애플리케이션)를 실행하는 시점에 결정되는 의존성
- 객체 사이의 의존성
- 결합도가 낮으며 변경에 유연함

## 의존성 주입

### 장점

- 객체의 불변성 확보.
- 결합도를 낮춰주며 유연한 코드를 만들어 준다.
- 테스트 용이. (원하는 의존성 주입)

1. 생성자 주입 (스프링이 권장하는 방법)

```
@Controller
public class TestController {
    private final TestServicetestService;

    public TestController(TestService testService) {
        this.testService= testService;
    }
}

@Service
class  TestService{}
```

- final 키워드로 불변성을 갖는다.
- 스프링에 비침투적인 코드 작성
- 순환 참조 방지 (빈이 생긴 뒤 참조하기 때문에 에러발생)

1. 필드 주입

```
@Controller
public class TestController {
    @Autowired
    private TestServicetestService;

}

@Service
class  TestService{}
```

- 가장 간편한 방식.
- null의 가능성
- 스프링 컨테이너 안에서만 작동(순수 자바로 Test 불가능)

1. Setter 주입

```
@Controller
public class TestController {
    private TestServicetestService;

    public void getTestService(TestService testService) {
        this.testService= testService;
    }

}

@Service
class  TestService{}
```

- setter 메서드가 public 이기 때문에 언제 어디서든 변경이 가능하다. (OCP 어김)
- 필수 관계가 아닌 경우 null 가능

### 자원을 직접 명시하지 말고 의존 객체 주입을 사용하라(생성자 주입을 사용해야하는 이유)

사용하는 자원에 따라 동작이 달라지는 클래스는 정적 유틸리티 클래스나 싱글톤은 적합하지 않다. → 인스턴스를 **생성할 때** 생성자에 필요한 자원을 넘겨준다.

- 불변을 보장한다.
- 같은 자원을 사용하는 의존 객체를 안심하고 공유하게 해준다.
- 유연성, 재사용성, 테스트 용이성 개선
- 또는 생성자에 팩토리 메서드나 빌더를 넣어줘도 좋다.

참조

---

이펙티브 자바

[[OOP] 의존성(Dependency)이란? 컴파일타임 의존성과 런타임 의존성의 차이 및 비교](https://mangkyu.tistory.com/226)

[[Spring] 의존성 주입 4가지 방법](https://velog.io/@skyepodium/Spring-%EC%9D%98%EC%A1%B4%EC%84%B1-%EC%A3%BC%EC%9E%85-4%EA%B0%80%EC%A7%80-%EB%B0%A9%EB%B2%95)
