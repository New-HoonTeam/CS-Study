# 피보나치를 코드로 구현하는 방법

# 피보나치 수열

> 수학 에서 **피보나치 수** (영어: Fibonacci numbers)는 첫째 및 둘째 항이 1이며 그 뒤의 모든 항은 바로 앞 두 항의 합인 수열이다. 처음 여섯 항은 각각 1, 1, 2, 3, 5, 8이다. 편의상 0번째 항을 0으로 두기도 한다.
> 
<img width="429" alt="image" src="https://user-images.githubusercontent.com/39071638/210305570-68285c3a-bb93-4331-a19e-71cc3588d6a4.png">
<img width="462" alt="image" src="https://user-images.githubusercontent.com/39071638/210305588-90772da1-ab69-4541-8242-95ece0deee5e.png">


### 그렇다면… 어떻게 짜야 할까요?

- 의사코드(Pseudo-Code)로 먼저 시도해보죠!!

> **의사코드**
(슈도코드, pseudocode[[1]](https://ko.wikipedia.org/wiki/%EC%9D%98%EC%82%AC%EC%BD%94%EB%93%9C#cite_note-1)
)는 프로그램을 작성할 때 각 [모듈](https://ko.wikipedia.org/wiki/%EB%AA%A8%EB%93%88_(%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%98%EB%B0%8D))
이 작동하는 논리를 표현하기 위한 언어이다. 특정 [프로그래밍 언어](https://ko.wikipedia.org/wiki/%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%98%EB%B0%8D_%EC%96%B8%EC%96%B4)
의 문법에 따라 쓰인 것이 아니라, 일반적인 언어로 코드를 흉내 내어 [알고리즘](https://ko.wikipedia.org/wiki/%EC%95%8C%EA%B3%A0%EB%A6%AC%EC%A6%98)
을 써놓은 코드를 말한다. 의사(疑似)코드는 말 그대로 흉내만 내는 코드이기 때문에, 실제적인 프로그래밍 언어로 작성된 코드처럼 [컴퓨터](https://ko.wikipedia.org/wiki/%EC%BB%B4%ED%93%A8%ED%84%B0)
에서 실행할 수 없으며, 특정 언어로 프로그램을 작성하기 전에 알고리즘의 모델을 대략적으로 모델링하는 데에 쓰인다.
> 

```purescript
F(n) = F(n - 1) + F(n - 2)
```

## How?

2가지 방식으로 구현이 가능함

- 재귀함수(Recursive)
- 동적 계획법(Dynamic Programming)

### 재귀 (Recursive)

> [컴퓨터 과학](https://ko.wikipedia.org/wiki/%EC%BB%B4%ED%93%A8%ED%84%B0_%EA%B3%BC%ED%95%99)에 있어서 **재귀**
(再歸, Recursion)는 자신을 정의할 때 자기 자신을 재참조하는 방법을 뜻하며, 이를 프로그래밍에 적한 **재귀 호출**(Recursive call)의 형태로 많이 사용된다. 또 사진이나 그림 등에서 재귀의 형태를 사용하는 경우도 있다.
> 

자기 자신을 호출하는 재귀 함수를 활용하는 형태로 문제를 푸는 방법

![Untitled](https://user-images.githubusercontent.com/39071638/210305446-ab76001e-c260-4335-afdf-f463f740624a.png)
나 자신을 이용해서 풀기!

```python
def fibo(n: int):
	fibo(n - 1) + fibo(n - 2)....
```

### 동적 계획법 (Dynamic Programming)

> 수학과 컴퓨터 과학, 그리고 경제학에서 **동적 계획법**
(動的計劃法, dynamic programming)이란 복잡한 문제를 간단한 여러 개의 문제로 나누어 푸는 방법을 말한다. 이것은 부분 문제 반복과 최적 부분 구조를 가지고 있는 알고리즘을 일반적인 방법에 비해 더욱 적은 시간 내에 풀 때 사용한다.
> 

이전의 상황을 이용해서 풀기

```python
fibo[i] = fibo(i - 1) + fibo(n - 2)...
```

뭐가 더 좋을까요???

- 한 번 짜 볼까요??

fibo.py

```python
import sys
import time
from pprint import pprint

def fibo_recursive(a: int):
    if a > 2:
        return fibo_recursive(a - 1) + fibo_recursive(a - 2)
    elif a >= 0:
        return a

def fibo_dp(a: int, dp: list):
    for i in range(2, a + 2):
        dp[i] = dp[i - 1] + dp[i - 2]

def solution():
    n = int(sys.stdin.readline().rstrip())
    dp = [i for i in range(n + 2)]

    dp_start = time.time()
    fibo_dp(n, dp)
    dp_end = time.time()

    recursive_start = time.time()
    rec = fibo_recursive(n)
    recursive_end = time.time()

    pprint(dp)
    pprint(rec)

    print(f'dp time :\t\t\t{dp_end - dp_start:.6f}')
    print(f'recursive time :\t{recursive_end - recursive_start:.6f}')

if __name__ == '__main__':
    solution()
```

```python
10
[0, 1, 1, 2, 3, 5, 8, 13, 21, 34, 55, 89]
89
dp time :			0.000003
recursive time :	0.000013
```

```python
30
[0,
 1,
 1,
 2,
 3,
 5,
 8,
 13,
 21,
 34,
 55,
 89,
 144,
 233,
 377,
 610,
 987,
 1597,
 2584,
 4181,
 6765,
 10946,
 17711,
 28657,
 46368,
 75025,
 121393,
 196418,
 317811,
 514229,
 832040,
 1346269]
1346269
		dp time :			0.000016
recursive time :	0.155707
```

fibo.cpp

```cpp
#include <bits/stdc++.h>
#include <ctime>
using namespace std;
int n, dp[1001];
int fibo_recursive(int a){
    if(a > 2) return fibo_recursive(a - 1) + fibo_recursive(a - 2);
    else if(a >= 0) return a;
}
int fibo_dp(int a){
    for(int i = 2; i < a + 2; i++)
        dp[i] = dp[i - 1] + dp[i - 2];
}
int main(){
    cin >> n;
    clock_t dp_start, dp_end, recursive_start, recursive_end;
    dp[0] = 0; dp[1] = 1;

    dp_start = ::clock();
    fibo_dp(n);
    dp_end = ::clock();

    recursive_start = ::clock();
    int f = fibo_recursive(n);
    recursive_end = ::clock();

    for(int i = 0; i < n + 2; i++)
        cout << dp[i] << ";
    cout << "\n";
    cout << f << "\n";

    cout << "dp time \t\t: " << dp_end - dp_start << "ms\n";
    cout << "recursive time  : " << recursive_end - recursive_start << "ms\n";

    return 0;
}
```

```cpp
10
0 1 1 2 3 5 8 13 21 34 55 89 
89
dp time 		: 0.7ms
recursive time  : 0.2ms
```

```cpp
30
0 1 1 2 3 5 8 13 21 34 55 89 144 233 377 610 987 1597 2584 4181 6765 10946 17711 28657 46368 75025 121393 196418 317811 514229 832040 1346269 
1346269
dp time 		: 0.7ms
recursive time  : 684.9ms
```

### 왜 이렇게 시간 차이가 나는 건가요??

[1003번: 피보나치 함수](https://www.acmicpc.net/problem/1003)

- 
    
![Untitled 1](https://user-images.githubusercontent.com/39071638/210305459-6733cd97-65e8-4822-998e-4a40bb055a00.png)
재귀는 불필요한 함수 호출을 지속적으로 하기 때문에 함수를 호출해서 이동하는 시간 + 리턴한 값을 받아오는 시간 까지 해서 매우 불필요하게 작동을 해요 (참고로 피보나치를 재귀로 구현한 코드의 시간 복잡도는 $O(2^n)$ 이에요)

반면에 DP의 시간복잡도는 $O(n)$으로 훨씬 효율적이죠 


[피보나치 수 - 위키백과, 우리 모두의 백과사전](https://ko.wikipedia.org/wiki/%ED%94%BC%EB%B3%B4%EB%82%98%EC%B9%98_%EC%88%98)

[재귀 (컴퓨터 과학) - 위키백과, 우리 모두의 백과사전](https://ko.wikipedia.org/wiki/%EC%9E%AC%EA%B7%80_(%EC%BB%B4%ED%93%A8%ED%84%B0_%EA%B3%BC%ED%95%99))

[동적 계획법 - 위키백과, 우리 모두의 백과사전](https://ko.wikipedia.org/wiki/%EB%8F%99%EC%A0%81_%EA%B3%84%ED%9A%8D%EB%B2%95)

[의사코드 - 위키백과, 우리 모두의 백과사전](https://ko.wikipedia.org/wiki/%EC%9D%98%EC%82%AC%EC%BD%94%EB%93%9C)
