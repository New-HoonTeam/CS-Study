## 자바의 동시성이슈(공유자원 접근) 

### 용어정리      
- `공유자원` : 여러 스레드가 동시에 접근할 수 있는 자원     
- `임계영역` : 공유자원 중 여러 스레드가 동시에 접근했을 때 문제가 생길 수 있는 부분
- `경쟁상태` : 둘 이상의 스레드가 공유자원을 병행적으로 읽거나 쓰는 동작 할 때,    
  타이밍이나 접근 순서에 따라 실행 결과가 달라지는 상태   
  
<br/>

- `동시성` : 한순간에 여러가지 일이 일어나는게 아님!  
  짧은전환으로 여러가지 일이 `동시에 처리되는것처럼` 보임     
  
<br/>  
  
- 경쟁상태 방지하려면 임계영역에 대한 원자성, 가시성 보장해야함 = `동기화(synchronization)`해야함 

<br/><br/>    

### 동시성이슈란  
여러 스레드가 동시에 같은 공유자원에 접근할 때 발생하는 문제   

![제목 없음](https://user-images.githubusercontent.com/103614357/207265775-19f580ae-2585-4e0c-8b2f-856c9367f553.png)    

- CPU가 메인메모리에서 변수값 읽어옴  
  - 이 때, CPU와 메인메모리 거리 머니까 CPU cache에 담아둠
- ex)
  - Thread1과 Thread2에서 `변수에 +1` 해주는 로직 실행  
  - CPU cache1과 CPU cache2에 `변수값 a = 1`를 읽어서 담아둠  
  - Thread1에서 a를 수정해서 `CPU cache1의 a = 2`도 수정이 되었음  
  - `메인메모리의 a = 2`도 수정이 되었음  
  - `CPU cache2의 a = 1`는 여전히 그대로, +1 하면 2가됨   
    - CPU cache1과 CPU cache2에서 한번씩 +1 했으니 3이 되어야하는데!  
  
  => 문제발생!     
  - 이 때, 메인메모리에 있는 진짜값을 보지 못하고   
    CPU cache2에 담긴 값만 본다고 해서 `가시성 문제`라고 함    
     
  => 자바에서의 해결 : 공유변수(공유자원)인 경우 `volatile` keyword를 변수에 붙여주기    
    
  (`메인메모리에서만 값을 읽고 쓰겠다!`는 의미 = CPU cache를 사용하지 않겠다)   

<br/><br/>

### 동기화  
원자성, 가시성을 챙기는 것  

- `원자성` : 더이상 쪼갤 수 없는 하나의 연산인듯 동작하는 것    
 
    - 위에서의 예, `읽음 - 수정 - 반영`(하나의 연산이 아닌, 3가지의 연산)    
      -> 연산 사이의 텀이 있어 다른 스레드의 연산 개입 가능 => 경쟁상태 발생  

<br/><br/>

**블로킹방식**  
한 스레드가 작업 수행하는 동안 다른작업 진행안함 = 대기함  
자바에서는 `모니터` 메커니즘 제공   
  
![제목 없음](https://user-images.githubusercontent.com/103614357/207277918-9a706081-ca81-4cfe-86c9-19f43409dcdb.png)  

- `배타동기` : 하나의 스레드만 공유자원에 접근하게 함     
       
  - 임계영역에 한번에 한 스레드만 락 걸고 들어가도록 설계    
    = 연산결과가 메모리에 써질 때까지 다른 스레드는 대기, 임계영역에 들어올 수 없음   
  - 그 외의 접근하려고 하는 스레드는 배타동기큐에서 대기 
  - 배타동기 선언하는 키워드 : `synchronized` 
- synchronized 블락 진입 전,후에 메인 메모리와 CPU cache 메모리의 값을 동기화하기 때문에 문제 해소됨  

![1214-2](https://user-images.githubusercontent.com/103614357/207551236-50794c63-0a9d-40ae-babd-9300fbf838a0.png)  

경우에 따라 두 스레드가 교대로 번갈아가며 실행해야 할 경우  
- `wait` 함수로 Thread1을 block을 하고, 조건동기에서 대기
  - wait() : 스레드를 일시 정지 상태로 만듬 
- 배타동기에서 대기하던 Thread2가 진입

![1214-2](https://user-images.githubusercontent.com/103614357/207551363-741a369b-3776-42f9-b387-3d9e6d1a7404.png)

- Thread2가 `notify`로 Thread1을 깨우고, 임계영역에서 나감  
  - notify() : 일시 정지 상태에 있는 다른 스레드를 실행 대기 상태로 만듬 
  - wait(), notify() 둘 다 Thread 클래스가 아닌 Object 클래스에 선언된 메소드기 때문에 모든 공유 객체에서 호출이 가능     

![1214-2](https://user-images.githubusercontent.com/103614357/207551637-5c283570-f744-41ce-99e3-3adbfba4b94c.png)  

- Thread1이 block에서 풀리고, 다시 진입  

<br/>
  
```java
public class WaitNotifyExample {
    public static void main(String[] args) {
        WorkObject sharedObject = new WorkObject();
        
        ThreadA threadA = new ThreadA(sharedObject);
        ThreadB threadB = new ThreadB(sharedObject);
        
        threadA.start();
        threadB.start();
    }
}
```

```java
public class WorkObject {
    public synchronized void methodA() {
    
        notify(); // 일시정지 상태에 있는 ThreadB를 실행 대기상태로 만듬 
        
        try {
            wait(); // ThreadA를 일시 정지 상태로 만듬 
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }
    
    public synchronized void methodB() {
    
        notify(); // 일시정지 상태에 있는 ThreadA를 실행 대기상태로 만듬
        
        try {
            wait(); // ThreadB를 일시 정지 상태로 만듬
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }
}
```

```java
public class ThreadA extends Thread{
    private WorkObject workObject;

    ThreadA(WorkObject workObject) {
        this.workObject = workObject;
    }
    
    public void run() {
        for(int i=0; i<10; i++) {
            workObject.methodA(); // 공유객체의 methodA를 반복적으로 호출 
        }
    }
}
```

```java
public class ThreadB extends Thread{
    private WorkObject workObject;

    ThreadB(WorkObject workObject) {
        this.workObject = workObject;
    }
    
    public void run() {
        for(int i=0; i<10; i++) {
            workObject.methodB(); // 공유객체의 methodA를 반복적으로 호출 
        }
    }
}
```

<br/>
  
=> 단점   
- 한 스레드 외에는 다 기다려야함 -> 성능저하  
- 임계영역 들어갈 때 락 걸고 들어감 -> 데드락 가능성  

<br/><br/>
  
**논블로킹방식**    
다른 스레드의 작업여부와 상관없음  
- `Atomic` : 자바에서 사용하는 동시성 보장을 위한 래퍼클래스  
  - `CAS 알고리즘`(원시성 보장) + `volatile`(가시성 보장)  
    - volatile : JVM 메모리에서 바로 CPU의 스레드로 값 가져옴      
    => CPU cache에서 잘못된 값을 참조하는 가시성 문제가 해결됨   
    
    - 연산은 CAS 알고리즘 이용한 compareAndSet()으로 `메모리에 저장된 값`과 `스레드가 가지고 있는 값(스레드 내부 기대값)`을 비교해서 일치할 경우에만 연산결과 반영    
    => 메인메모리와 Thread 내부 값이 다를 경우의 문제도 해결이 됨  
       
     
```java  
public class AtomicInteger extends Number implements java.io.Serializable {
	
    private volatile int value;

    public final int incrementAndGet() {
        int current;
        int next;
        do {
            current = get();
            next = current + 1;
	} while (!compareAndSet(current, next)); 
	return next;
    }
	
    public final boolean compareAndSet(int expect, int update) {
        return unsafe.compareAndSwapInt(this, valueOffset, expect, update);
    }	
}
```

- 메모리에 저장되어진 값과 현재 thread에 저장된 expect 값을 비교하여 동일한 경우만 변경하려는 값 update로 쓰기를 수행  

<br/>

- 참고) CAS(Compare And Set) 알고리즘이란?    
  - `자원값 & 기대값` 비교   
    - 같은 경우 `변경할 값(새로운 값)`을 자원값에 반영하고 true 반환    
    - 다른 경우에는 변경값이 반영되지 않고 false 반환한 다음 재시도를 하는 방식으로 동작    
      (요구사항에 따라 다름, exception 발생하게 한다던지 등)   

<br/><br/>

**Java에서의 CAS 동작**    

![제목 없음](https://user-images.githubusercontent.com/103614357/207280426-6c852f41-a2a4-4f09-bba1-23ce3065bead.png)   

1. Thread1과 Thread2는 heap 내의 count 변수 읽어 CPU cache 메모리에 저장  
2. 번갈아가며 for문을 돌면서 count 값 1씩 증가
3. 변경한 count 값을 힙에 반영하기 위해 `변경 전의 count값`과 `heap에 저장된 count값` 비교   
   1) `같을 경우` true 반환, heap에 변경한 값 저장
   2) `다를 경우` false 반환, heap에 저장된 값을 읽어 2번으로
4. 힙에 변경한 값을 저장 후 Thread는 for문이 종료될 때까지 1번으로 가서 반복  

<br/><br/>

Reference   
https://www.youtube.com/watch?v=ktWcieiNzKs&ab_channel=%EC%9A%B0%EC%95%84%ED%95%9CTech     
https://steady-coding.tistory.com/554     
https://steady-coding.tistory.com/568       
https://javaplant.tistory.com/23      
https://n1tjrgns.tistory.com/244     
https://m.blog.naver.com/gngh0101/221174237333     
https://cornswrold.tistory.com/189  

<br/>
