# 자바 Primitive vs Reference

# 자바내에서의 기본 타입의 종류

- 정수
    - byte
    - char
    - short
    - int
    - long
- 실수
    - float
    - double
- 논리
    - boolean

데이터의 실제 값을 저장함

![Untitled](https://user-images.githubusercontent.com/39071638/207390513-94c9dafc-36b3-4b1e-98ea-8380b02102cb.png)

그렇다면 참조타입은??

- 배열타입
- 열거 타입
- 클래스
- 인터페이스

![Untitled 1](https://user-images.githubusercontent.com/39071638/207390539-af3e438b-c4bc-454c-97c5-3a9528a8a060.png)
왜 참조 타입으로 불리는 걸까?? 

→ 객체의 주소를 저장하기 때문에!!

자바에서 실제 객체는 Heap 영역에 저장되며, 실제 객체들의 주소는 Stack영역에 저장함

객체를 사용할 때 마다 Stack 영역에 저장된 참조 변수에 저장된 주소를 불러와서 객체를 사용함

```java
int a = 10;
Integer b = 7;
```

![Untitled 2](https://user-images.githubusercontent.com/39071638/207390557-c5df02d7-17fe-4af7-b7d8-d76c4ed6b1f9.png)
|  | Primitive | Reference |
| --- | --- | --- |
| is Nullable? | X | O |
| 제네릭에 사용 가능? | X | O |
| 속도 | 매우 빠름 | 그냥 그저 |
|  | 모두 자바 내에서 정의되어 있음(pre-defined) | String만 빼고 자바내에서 정의되어있지 않음 |
|  | 변수명이 모두 소문자임 | 대문자로 시작함 |
|  |  |  |
|  | 타입에 따라 크기가 다름 | JVM은 기본적으로 8byte를 할당해줌 |
|  |  |  |

# String pool

String을 new 연산자로 사용하지 않고 리터럴로 생성한 경우에 String 객체가 들어가는 공간

String을 new 연산자로 생성할 경우??

```java
String s1 = new String("Cat");
String s2 = new String("Cat");
...
```

![Untitled 3](https://user-images.githubusercontent.com/39071638/207390650-d5e3bf0f-b423-44ce-bfc4-0d350d899676.png)


String을 리터럴로 생성하면??

```java
String s1 = "Cat";
String s2 = "Dog";
String s3 = "Cat";
```


![Untitled 4](https://user-images.githubusercontent.com/39071638/207390704-78966f80-f73c-43f5-bf07-3d93b72dd2f8.png)

```
