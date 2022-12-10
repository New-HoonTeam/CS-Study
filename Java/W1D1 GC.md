
### GC(Garbage Collector)는 JVM의 heap 영역에 할당한 메모리 영역중 사용하지 않는 영역을 탐지하여 해제(제거)한다.

자바에서는 개발자가 메모리를 직접 해제할 수 없기 때문에 GC가 필요하게 된다.

**GC는 어떻게 작동할까??**

![](https://velog.velcdn.com/images/goseungwon/post/fe53f414-85a5-4d94-9e2b-9492c4201e81/image.png)

**먼저 JVM의 heap 영역의 구조부터 알아야 한다.**

- Young Generation : 새롭게 생성하는 객체들이 존재하는 곳
- Old Generation : Young Generation 영역에서 오래동안 살아남은 객체(가중치)들이 존재하는 곳
- Meta Space : 클래스의 메타데이터, 문자열 정보를 저장하는 영역 (자바 8부터 생김)

*YG와 OG로 나눈 이유는 어플리케이션에서의 객체의 수명은 대부분이 짧기 때문에 효율적으로 관리하기 위함이다.

### GC 알고리즘

**Reference Counting**

- Root Space에 접근할 수 있는 방법(참조하는 곳)이 없는 객체는 삭제 대상이 된다.
- 순환참조에 대해 문제가 발생한다.

**Mark-Sweep-Compact**

<img src= "https://velog.velcdn.com/images/goseungwon/post/242b81af-c345-4052-9d68-a9a5a04193f1/image.png" width="600px">


- JVM에서 사용하는 방식
- Root Space에서부터 순환을 하여 방문한 객체에 Mark를 한다. 
JVM에서 Root Space는 stack, method stack, method 영역을 뜻한다.
- Mark가 되지 않은 객체, 즉 방문하지 않은 객체는 삭제(Sweep) 대상이 된다.
- 남아있는 객체들은 정리(Compact)한다.
- 여기서 Compact는 필수가 아니다.
- 의도적으로 GC를 실행시켜야 되기 때문에 Stop The World가 발생한다.

*Stop The World : GC를 위해 어플리케이션을 멈추는 것

### Young Generation과 Old Generation에서의 GC는 다르게 나타난다.

**YG**

1. 새로운 객체는 Eden영역에 생성이 된다.
2. Eden영역이 가득차면 (Minor)GC를 발생시키고 살아남은 객체는 age(가중치)를 증가시키고 Survival 0 또는1 로 이동한다. 
3. Survival 0 또는 1 중 하나는 항상 비어있어야 한다. 앞으로 Survival은 S로 부르겠다.
4. 1~3을 반복하며 age가 threshold를 넘으면 Old Generation 영역으로 이동한다. (Promotion)
5. S 0 또는 1 이 꽉차게 되어도 GC 발생

**OG**

1. Old Generation 영역이 가득차면 (Major)GC가 발생한다.
2. Major GC는 여러가지 종류가 있음. 

### Major GC의 종류

**Serial GC**

<img src= "https://velog.velcdn.com/images/goseungwon/post/b7288d85-6b11-42b7-95c2-00d1c68dd954/image.png" width="250px">



- 하나의 스레드로 Mark Sweep Compact 알고리즘을 사용하여 OG 영역의 객체를 지운 뒤 정리한다.
- 적은 메모리와 CPU코어에 적합한 방법
- Stop The World 시간이 길다.

**Parallel GC**

<img src= "https://velog.velcdn.com/images/goseungwon/post/07254371-1051-4075-82c7-77c0d6756c97/image.png" width="300px">


- Serial GC와 같은 방식이지만, YG 영역에 대하여 멀티 스레드로 처리한다.
- Serial GC에 비해 Stop The World 시간이 짧다.

**Parallel Old GC**

- Parallel GC와 같지만 OG 영역까지 멀티 스레드로 처리한다.
- Java 8의 default 방식이다.

**CMS GC**

<img src= "https://velog.velcdn.com/images/goseungwon/post/f480ce68-2648-452f-9323-0a6d7e282ce9/image.png" width="400px">


- Stop The World를 최소화 하기 위해 고안한 방법으로 4 단계가 있다.
1. Initial Mark : Root Space가 직접 참조하는 객체에만 Marking 한다. (STW 발생)
2. ConCurrent Mark : 1 에서 마킹한 객체가 참조하는 다른 객체들을 따라가며 마킹한다. (STW 발생안함)
3. Remark : 2에서 변경된 과정을 한번 더 마킹하며 확정시킨다. (STW 발생)
4. Current Sweep : Mark 하지 않은 객체 제거 (STW 발생안함)
- Compact 를 지원하지 않는다
- G1 GC가 나온뒤 Deprecated 되었다.

**G1(Garbage First) GC**

<img src= "https://velog.velcdn.com/images/goseungwon/post/198aff33-f96f-43c2-ba3c-e3322ae10675/image.png" width="800px">



- Heap을 Region이라는 단위로 나눈 뒤, Eden, Survivor, OG Region으로 나눈다.
- Garbage만 남아있는 Region을 회수한다.
- Runtime에서 필요에 따라 YG, OG 영역의 비율을 튜닝한다.
- 자바 9 이후로 default GC이다(single core인 경우 여전히 serial gc를 사용한다.

장점

- Garbage영역만 회수하기 때문에 GC의 빈도 줄어든다.
- Compact 비용이 낮아진다.

단점

- 공간 부족 상태를 조심해야한다. (Minor/Major GC를 하고도 공간이 부족한 경우 Full GC가 발생한다.)
Full GC는 single thread로 동작한다.
- Heap 영역의 공간이 작으면 비효율적이다.



---
참고

[https://steady-coding.tistory.com/590](https://steady-coding.tistory.com/590)

[https://d2.naver.com/helloworld/1329](https://d2.naver.com/helloworld/1329)

[https://stackoverflow.com/questions/70664562/criteria-for-default-garbage-collector-hotspot-jvm-11-17](https://stackoverflow.com/questions/70664562/criteria-for-default-garbage-collector-hotspot-jvm-11-17)

우아한 테코톡

---

GC 튜닝이 알고싶다면

[https://d2.naver.com/helloworld/329631](https://d2.naver.com/helloworld/329631)
