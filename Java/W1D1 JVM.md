# JVM의 구조와 자바 실행방식

# JVM?

JVM은 Java Virtual Machine의 약자로서 자바 가상 머신이라고도 불림

JVM은 자바 프로그램와 OS 사이에서 중개자 역할을 수행하며, 자바가 OS에 종속되지 않고 실행될 수 있도록 해줌

자바 코드를 컴파일 해서 .class 바이트 코드를 생성하면 이 코드는 JVM 환경에서 실행 됨.

JVM은 JRE(Java Runtime Environment)에 포함되어 있음.

![Untitled](https://user-images.githubusercontent.com/39071638/207387435-4926ae90-5fba-4ede-bf5b-bd67ed4d4caf.png)
![Untitled 1](https://user-images.githubusercontent.com/39071638/207387452-23f1824b-f334-4f2e-a1bc-5df7ac8cf5b7.png)
왜 JVM이 필요할까요?

EX) test.c를 윈도우 환경에서 컴파일할 경우 → `test.exe` 라는 실행파일이 생성됨. (윈도우에서만 실행 가능한 파일)

test.c를 리눅스 환경에서 컴파일 할 경우

test.c —(전처리 ccp)→ test.i(전처리된 파일) —(컴파일 cc1)→ test.s(어셈블리어 파일) —(어셈블as)→ test.o(오브젝트 파일) —(링크 ld)→ `a.out`(실행 파일) 파일 생성

`.out` 포맷은 리눅스 환경에서만 실행 가능한 파일

각 OS에서의 실행파일이 OS에 종속적인 경우 해당 OS에서만 실행파일을 실행할 수 있음

자바 → os에 상관 없이 .class파일 실행이 가능함!

주의

Java는 플랫폼에 독립적이지만 JVM은 플랫폼에 종속적이다!

### JVM 특징

- **스택 기반의 가상 머신**: 대표적인 컴퓨터 아키텍처인 인텔 x86아키텍처, ARM 아키텍처와 같은 하드웨어가 레지스터 기반으로 동작하는 데 비해 JVM은 스택 기반으로 동작한다.
- **심볼릭 레퍼런스**: 기본 자료형(primity data type)을 제외한 모든 타입(클래스와 인터페이스)을 명시적인 메모리 주소 기반의 레퍼런스가 아니라 심볼릭 레퍼런스를 통해 참조한다.
    
    
    클래스 파일은 JVM이 프로그램을 실행할 때 필요한 API를 Link할 수 있도록 **심볼릭 레퍼런스**를 가진다. 
    
    심볼릭 레퍼런스를 런타임 시점에  메모리 상에서 실제로 존재하는 **물리적인 주소로 대체하는 Linking 작업**이 일어난다. 
    
    심볼릭 레퍼런스는 참조하는 대상의 이름을 지칭하고, 클래스 파일이 JVM에 올라가게 되면 심볼릭 레펀런스는 **실제 메모리 주소가 아닌 이름에 맞는 객체의 주소**를 찾아서 연결하는 작업을 수행한다.
    
- **가비지 컬렉션(garbage collection)**: 클래스 인스턴스는 사용자 코드에 의해 명시적으로 생성되고 가비지 컬렉션에 의해 자동으로 파괴된다.
- **기본 자료형을 명확하게 정의하여 플랫폼 독립성 보장**: C/C++등의 전통적인 언어는 플랫폼에 따라 int형의 크기가 변한다. JVM은 기본 자료형을 명확하게 정의하여 호환성을 유지하고 플랫폼 독립성을 보장
    
    long 형의 경우 32-bit → 4byte/64-bit → 8byte
    
- **네트워크 바이트 오더(network byte order)**: 자바 클래스 파일은 네트워크 바이트 오더를 사용한다. 인텔 x86아키텍처가 사용하는 리틀 엔디안이나, RISC 계열 아키텍처가 주로 사용하는 빅 엔디안 사이에서 플랫폼 독립성을 유지하려면 고정된 바이트 오더를 유지해야 하므로 네트워크 전송 시에 사용하는 바이트 오더인 네트워크 바이트 오더를 사용한다. 네트워크 바이트 오더는 **빅 엔디안**이다.
    
    Big-Endian : 큰 단위가 먼저 오게 바이트를 정렬
    
    Little-Endian :  작은 단위가 먼저 어게 바이트를 정렬하는 방식
    
![Untitled 2](https://user-images.githubusercontent.com/39071638/207387483-c99e88c9-8318-477d-9921-27dc7656005b.png)    

## JVM의 메모리 구조

크게 4가지 파트로 이루어짐

- `Garbage Collector`
- `Execution Engine`
- `Class Loader`
- `Runtime Data Area`

![Untitled 3](https://user-images.githubusercontent.com/39071638/207387509-e32f6417-9a52-4a7f-b857-f670cd138c3e.png)
JVM 동작 방식

1. 자바로 개발된 프로그램을 실행하면 JVM은 OS로부터 메모리를 할당합니다.
2. 자바 컴파일러(javac)가 자바 소스코드(`.java`)를 자바 바이트코드(`.class`)로 컴파일합니다.
3. `Class Loader`를 통해 JVM `Runtime Data Area`로 로딩합니다.
4. `Runtime Data Area`에 로딩 된 .class들은 `Execution Engine`을 통해 해석합니다.
5. 해석된 바이트 코드는 `Runtime Data Area`의 각 영역에 배치되어 수행하며 이 과정에서 `Execution Engine`에 의해 `GC`의 작동과 스레드 동기화가 이루어집니다.

---

JVM 세부 구조

### Class Loader

![Untitled 4](https://user-images.githubusercontent.com/39071638/207387606-0bcbdee5-7a5c-4b1a-9557-e1d85ff2e1b2.png)
자바는 클래스를 동적으로 읽어오므로, **런타임** 중에서 모든 코드가 JVM과 연결됨

여기서 동적으로 클래스를 로딩해주는 역할을 수행하는게 바로 `Class Loader`

자바를 이용해서 `.java` 파일을 생성하면,

컴파일일러는 `.java` 파일을 `.class` 파일로 변환함

이때 `Class Loader`는 `.class` 파일을 묶어서 JVM이 OS로 부터 할당 받은 영역인 `Runtime Data Area`로 적재함

### Execution Engine(실행 엔진)

`.class` 파일들은 `Runtime Data Area`의 메소드 영역으로 이동함

배치된 이후에 JVM은 메소드 영역의 바이트 코드를 실행 엔진(Execution Engine)에 제공하여, 정의된 내용대로 바이트 코드를 실행시킴

여기서 바이트 코드를 실행시키는 모듈이 `Runtime Engine` 

`Runtime Engine` 은 명령어 단위로 바이트 코드를 실행함

### Garbage Collector

![Untitled 5](https://user-images.githubusercontent.com/39071638/207387639-f255e6d8-17d1-4f72-b557-c5839e25360c.png)
C, C++ 와 같은 언어와는 다르게 Java는 사용자가 사용하지 않는 객체에 대해서 직접 할당 해제를 할 필요가 없음!

해당 역할은 `GC`가 대신 수행해줌

Heap 영역에 생성되어서 적재된 객체들 중에서 더 이상 참조되지 않는 객체를 탐색 후 제거하는 역할을 수행 함

### Runtime Data Area

![Untitled 6](https://user-images.githubusercontent.com/39071638/207387727-58aa373f-e903-42ac-a95d-a663c9ae12a9.png)
`런타임 데이터 영역`은 JVM의 메모리 영역으로 자바 App을 실행시킬 때, 데이터를 적재하는 영역

공용 공간(`GC`의 대상)

- Method 영역
    - 클래스 멤버 변수의 이름, 데이터 타입, 접근 제어자와 같은 각종 필드 정보
    - 메서드, Data Type 정보, Constant Pool, static 변수, final class 등이 생성되는 영역
- Heap
    - new 연산자로 생성한 객체들이 있는 공간
    - `GC`가 주기적으로 청소함

![Untitled 7](https://user-images.githubusercontent.com/39071638/207387758-628af40f-be90-4f14-bb5a-dc77dd7e6ad5.png)
Thread 마다 생성되는 영역

- Stack
    - 지역변수, 파라미터, 리턴 값, 연산에 사용되는 임시 값 등이 생성되는 영역
- PC Register
    - Thread가 생성될 때마다 생성되는 영역으로 프로그램 카운터, 즉 현재 스레드가 실행되는 부분의 주소와 명령을 저장하고 있는 영역
- Native Method Stack
    - 자바 이외의 언어(C, C++, 어셈블리 등)로 작성된 네이티브 코드를 실행할 때 사용되는 메모리 영역으로 일반적인 C 스택을 사용함
    - 보통 C/C++ 등의 코드를 수행하기 위한 스택을 말하며 (JNI) 자바 컴파일러에 의해 변환된 자바 바이트 코드를 읽고 해석하는 역할을 하는 것이 자바 인터프리터(interpreter)
    - 즉, 자바 이외의 언어로 작성된 네이티브 코드를 위한 영역

## JVM의 컴파일 과정

- `.java` 파일이 컴파일러를 통해 전달 된 다음 소스코드를 Bytecode로 인코딩한다.

소스파일에 포함 된 각 클래스의 내용은 별도의 `.class` 파일에 저장된다. 소스코드를 바이트 코드로 변환하는 동안 컴파일러는 다음 단계를 따른다.

- **Parse**: `*.java` 소스파일을 읽은 뒤 결과 토큰을 AST(Abastract Syntax Tree) 노드에 매핑한다.
- **Enter**:  정의된 심볼들을 심볼테이블(Symbol table) 에 저장한다.
- **Process annotations**: 요청된 경우 지정된 컴파일 위치에서 찾은 애너테이션을 처리합니다.
- **Attribute**: 구문 트리에 속성을 부여하며, 이름 확인, 유형 검사 및 상수 정의가 포함된다.
- **Flow**: 이전 단계의 트리에 대한 데이터 흐름을 분석합니다. 여기에 할당 및 도달 가능성에 대한 검사도 포함됩니다.
- **Desugar**: AST를 다시 작성하고 몇몇 syntactic sugar 들을 번역합니다.
- **Generate**: `*.class` 파일을 생성합니다.

![Untitled 8](https://user-images.githubusercontent.com/39071638/207387806-52bd3403-4c14-4698-96a4-81671ec19a6d.png)
![Untitled 9](https://user-images.githubusercontent.com/39071638/207387830-ad6d17e8-345b-4e46-bd9c-fcf68e154cc6.png)

[[Java] 자바 JVM 내부 구조와 메모리 구조에 대하여](https://coding-factory.tistory.com/828)

[[Java] 자바 가상머신 JVM(Java Virtual Machine) 총정리](https://coding-factory.tistory.com/827)

[자바 가상머신 JVM](https://lkhlkh23.tistory.com/100)

[[1주차] JVM은 무엇이며 자바 코드는 어떻게 실행하는 것인가.](https://catsbi.oopy.io/df0df290-9188-45c1-b056-b8fe032d88ca)
