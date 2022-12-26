
## Hashing

Hash Function을 사용해 고정된 크기의 값으로 변환하는 것.

## Hash Function

알고리즘을 이용하여 데이터의 고유한 새로운 숫자로 변환하는 것이다. (무결성, 암호화 등 사용처가 많다)

Division기법, Multiplication기법 등등 많은 알고리즘이 있다.

자바에서는 어떤 방법으로 해싱을 할까?

자바에서의 hashCode는 2^31에서 2^31사이의 정수를 뜻한다.
<img src="https://velog.velcdn.com/images/goseungwon/post/f5a4d555-b752-45ed-9cc2-b7d4f1fc46ec/image.png"  width="60%"/>


Object 클래스에 hashCode가 있기 때문에 오버라이드해서 각자의 방법을 통해 해싱할 수 있다.

오버라이드 하지 않는다면 어떤 방법으로 해싱할까? (자바 17버전)

```java
public static void main(String[] args) {
    Test test1 =new Test(1);
    Test test2 =new Test(2);

    System.out.println(test1);
    System.out.println(System.identityHashCode(test1));
    System.out.println(test2);
    System.out.println(test2.hashCode());
  }
}

class Test {
	int num;

	public Test(intnum) {
	this.num= num;
  }
```

위의 코드를 실행시켜보면 결과가 이렇게 나온다.
<img src="https://velog.velcdn.com/images/goseungwon/post/7d8cb8e6-2b49-4a3c-8ddc-b0050a5be7bf/image.png"  width="40%"/><img src="https://velog.velcdn.com/images/goseungwon/post/a92dc331-f85a-4a68-a38f-5ff4d076c120/image.png"  width="30%"/><img src="https://velog.velcdn.com/images/goseungwon/post/714d89ac-3d9a-4811-83e8-afd3772976b4/image.png"  width="30%"/>



별다른 알고리즘이 있지 않고 객체의 주소를 10진수로 변환해서 보여준다.

String에선 어떤 방법으로 해싱할까? 

![](https://velog.velcdn.com/images/goseungwon/post/9a31a2e6-dca2-4569-a400-ccb4437b098c/image.png)


먼저 isLatin1을 통해 문자열의 인코딩에 따라 다른 방법을 사용한다.

- Latin1 (0xff = 255)

  <img src="https://velog.velcdn.com/images/goseungwon/post/699592b7-b7cf-4ed8-bf88-ee00ee186d5d/image.png"  width="50%"/>

- UTF16

  <img src="https://velog.velcdn.com/images/goseungwon/post/17079597-e872-46ea-9676-ff82ff852477/image.png"  width="50%"/>


여기서 31을 사용하는 이유는 이펙티브 자바에선 이렇게 설명한다.

31은 소수면서 동시에 홀수이다. 그리고 시프트와 뺼셈의 조합을 통하면 더 빠른 성능을 낼 수 있다.(32-1)

짝수 곱셈에서는 overflow가 발생시 정보를 손실하게된다.

<br/>

## Hash Table

(key, value)로 저장하는 자료구조중 하나로 검색 속도가 매우 빠르다.

Hash Table은 key를 해싱하여 배열에 index를 생성하여 value를 저장/검색 한다. 
그렇기 때문에 Hash Table의 평균 시간 복잡도는 O(1)이다.

엄청난 검색 성능을 가진 Hash Table의 단점은 없을까?

다른 key값에 대해서 동일한 index를 반환하는 해시 충돌이 생긴다.

해시 충돌은 해시테이블의 공간 사용률이 70~80%정도 공간을 활용하게 되면 빈번하게 발생되어 성능이 저하된다.

### 해시 충돌 해결법

해시 충돌법은 크게 두가지로 나뉜다.

1. 분리 연결법
    ![](https://velog.velcdn.com/images/goseungwon/post/f869b265-3241-44c9-b9d6-02756fc7a585/image.png)

    
    같은 인덱스로 해싱되는 원소를 하나의 연결리스트로 관리한다.
    
    원소를 순차탐색한다.
    

1. 개방 주소법
    
    ![](https://velog.velcdn.com/images/goseungwon/post/5323f376-1200-404c-a181-a4dc46ba7d48/image.png)

    
    분리 연결법과 다르게 해시테이블 공간 안에서 해결한다는 특징이 있으며 세가지로 분류된다.
    
    - Linear Probing
        
        가장 간단한 충돌 해결 방법으로 충돌이 일어난 자리에서 일정 거리(일차함수)를 넘어에 저장한다.
        
        테이블의 맨 뒤를 넘어가면 맨 앞으로 온다.
        
    - Quadratic Probing
        
        충돌이 일어난 자리에서 일정거리(이차함수)를 넘어 저장한다.
        
        특정 영역에 데이터가 몰려도 빨리 빠져나온다.
        
    - Double Hashing
        
        해시 충돌이 나면 두번 째 해시함수를 사용해 저장한다.
        
        해시 충돌이 두번 일어나는 경우는 매우 드물기 때문에 무결성을 보장한다.
        
    

참고

---

이펙티브자바

[[10분 테코톡] 👩‍🏫코니의 #️⃣Hash Function](https://www.youtube.com/watch?v=Rpbj6jMYKag)

[[자료구조] 해시테이블(HashTable)이란?](https://mangkyu.tistory.com/102)

[[자료구조] 해시 테이블(Hash Table) 이란? , 해시 알고리즘 , 해시 함수](https://code-lab1.tistory.com/14)
