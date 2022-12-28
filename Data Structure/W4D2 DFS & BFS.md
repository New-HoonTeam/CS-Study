## Graph (그래프)

**정점(Node)** 과 그 정점을 연결하는 **간선(Edge)** 으로 이루어진 자료구조
그래프 탐색이란 **하나의 정점에서 시작해 순차적으로 다른 정점을 한 번씩 방문**하는 것

### 그래프 구현 방법

1. 인접 행렬 (Adjust Matrix)
    - 정점(노드) 개수가 많아질수록 메모리 공간이 많이 필요 (N^2)
        - 방향성이 없는 경우에는 **불필요한 공간(메모리 효율성 저하)**이 생길 수 있음 (A→B == B→A)
    - **원하는 노드 간 코스트(Edge)를 구하는데 유리** (Index 기반 접근)
        - 그러나 모든 간선의 수를 알아내려면 O(V^2)의 시간복잡도
2. 인접 리스트 (Adjust List)
    - 인접 행렬에 비해 메모리 효율이 조금 낫지만 **조회** 속도에서 불리
        - 모든 간선의 수를 알아내려면 O(V+E)의 시간복잡도
    - 두 정점을 연결하는 간선을 조회하거나 정점의 차수를 알기 위해서 O(degree(v))의 복잡도
        - 해당 정점의 인접 리스트를 모두 탐색해야하므로…

|  | 인접 행렬 | 인접 리스트 |
| --- | --- | --- |
| 간선(u, v) 검색 | O(1) | O(degree(v)) |
| 정점(v)의 차수 계산 | O(n) | O(degree(v)) |
| 전체 노드 탐색 | O(n^2) | O(e) |
| 메모리 | N^2 | N+E |
| 구현 | EASY | DIFFICULT |

## DFS (Depth-First Search)

**깊이 우선 탐색**
루트 노드(혹은 다른 임의의 노드)에서 시작해 다음 분기로 넘어가기 전에 해당 분기를 완벽하게 탐색

![https://velog.velcdn.com/images%2Flucky-korma%2Fpost%2F30737a15-9adf-49a6-96a0-98c211cab1cc%2FR1280x0.gif](https://velog.velcdn.com/images%2Flucky-korma%2Fpost%2F30737a15-9adf-49a6-96a0-98c211cab1cc%2FR1280x0.gif)

### 구현 방식

DFS 알고리즘은 일반적으로 **재귀** 혹은 **스택**을 통해 구현

### 특징

- **방문한 노드는 기억**해두고 탐색 진행 중에 **막히면** 최근접 분기점으로 돌아와서 **다른 방향으로 탐색**
- 모든 노드를 방문하고자 할 때 사용
- 너비우선탐색(BFS)에 비해 비교적 **간단**하나 검색 속도 자체는 **느림**

![IMG1](https://p1dgey.notion.site/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2F69020591-b548-4147-85f1-0f97ff20def9%2FUntitled.png?id=bf7fd96a-0739-48ab-83f2-8c1d209476b3&table=block&spaceId=b76551b9-9f24-4a91-9bcd-340caa404f60&width=1800&userId=&cache=v2)

```java
// map : i -> j 로 이동 가능하면 1 아니면 0
// visit : i번 노드 방문 히스토리
public static void dfs(int i) {
	// 목표 지점인지 체크하는 로직을 넣을 수 있다.
	if (i == target) {
		System.out.println("Arrived!");
	}

	// 현재 노드 방문 히스토리를 남긴다.
	visit[i] = true;
	
	for(int j = 1; j < n+1; j++) {
		if (map[i][j] == 1 && visit[j] == false) {
			// 한 번 진입한 쪽으로 계속 탐색
			dfs(j);
		}
	}
}
```

## BFS (Breadth-First Search)

**너비 우선 탐색**
그래프 시작점에서 가까운 점들부터 우선적으로 탐색

![https://blog.kakaocdn.net/dn/oqv0X/btrr3mQXzrt/KvzkeJEbwkXI1c9PkmXLfK/img.gif](https://blog.kakaocdn.net/dn/oqv0X/btrr3mQXzrt/KvzkeJEbwkXI1c9PkmXLfK/img.gif)

### 구현 방식

일반적으로 **큐** 자료구조를 활용해 구현

### 특징

- DFS와 마찬가지로 **방문한 노드를 기억해두고 한 번 방문한 노드는 접근 X**
- 먼저 방문한 노드를 꺼낼 수 있는 큐 자료구조 사용
- 두 노드 사이의 **최단 경로**를 찾고 싶을 때 사용
    - 단, **간선의 길이(비용)이 같다**는 가정
    - 간선이 비용이 다른 경우 **[Minimum Spanning Tree](https://gmlwjd9405.github.io/2018/08/28/algorithm-mst.html)** 참고

![IMG2](https://p1dgey.notion.site/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2F9c9b3f9b-9238-4f11-9d8c-5d524093392f%2FUntitled.png?id=f21b46c6-68b9-40ac-8bbc-ca4fbe4fa2e4&table=block&spaceId=b76551b9-9f24-4a91-9bcd-340caa404f60&width=1800&userId=&cache=v2)

```java
public static void bfs(int i) {
	Queue<Integer> q = new LinkedList<>();
	q.offer(i); // 시작 노드를 큐에 넣는다. (= add = addLast)

	// 시작 노드 방문 히스토리를 남긴다.
	visit[i] = true;
	
	while (!q.isEmpty()) {
		int cursor = q.poll(); // 가장 먼저 들어가있던 원소를 뽑아낸다.
		System.out.print(cursor + " ");
		for(int j = 1; j < n+1; j++) {
			if (map[cursor][j] == 1 && visit[j] == false) {
				q.offer(j);
				visit[j] = true;
			}
		}
	}
}
```

### 오른손 법칙

![IMG3](https://p1dgey.notion.site/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2F751f53ce-e932-4470-b15a-e9e398313ab1%2FUntitled.png?id=008839d0-7886-43fa-a28e-2de319cce4fa&table=block&spaceId=b76551b9-9f24-4a91-9bcd-340caa404f60&width=1600&userId=&cache=v2)

## 연습 문제

단순히 그래프에서 모든 정점을 탐색하는 용도(대부분의 문제)라면 DFS, BFS 중 편한 것을 사용

- 단, 가중치 없는 그래프의 최단 경로문제는 BFS로 해결

| 문제 이름 | 문제 링크 | 답안 코드 링크 |
| --- | --- | --- |
| DFS와 BFS | [링크](https://www.acmicpc.net/problem/1260) | [링크](https://github.com/rhs0266/FastCampus/tree/main/%EA%B0%95%EC%9D%98%20%EC%9E%90%EB%A3%8C/02-%EC%95%8C%EA%B3%A0%EB%A6%AC%EC%A6%98/09~11-%EA%B7%B8%EB%9E%98%ED%94%84%20%ED%83%90%EC%83%89/%EB%AC%B8%EC%A0%9C%EB%B3%84%20%EC%BD%94%EB%93%9C/1260-DFS%EC%99%80%20BFS) |
| 단지번호 붙이기 | [링크](https://www.acmicpc.net/problem/2667) | [링크](https://github.com/rhs0266/FastCampus/tree/main/%EA%B0%95%EC%9D%98%20%EC%9E%90%EB%A3%8C/02-%EC%95%8C%EA%B3%A0%EB%A6%AC%EC%A6%98/09~11-%EA%B7%B8%EB%9E%98%ED%94%84%20%ED%83%90%EC%83%89/%EB%AC%B8%EC%A0%9C%EB%B3%84%20%EC%BD%94%EB%93%9C/2667-%EB%8B%A8%EC%A7%80%EB%B2%88%ED%98%B8%20%EB%B6%99%EC%9D%B4%EA%B8%B0) |
| 물통 | [링크](https://www.acmicpc.net/problem/2251) | [링크](https://github.com/rhs0266/FastCampus/tree/main/%EA%B0%95%EC%9D%98%20%EC%9E%90%EB%A3%8C/02-%EC%95%8C%EA%B3%A0%EB%A6%AC%EC%A6%98/09~11-%EA%B7%B8%EB%9E%98%ED%94%84%20%ED%83%90%EC%83%89/%EB%AC%B8%EC%A0%9C%EB%B3%84%20%EC%BD%94%EB%93%9C/2251-%EB%AC%BC%ED%86%B5) |
| 연구소 | [링크](https://www.acmicpc.net/problem/14502) | [링크](https://github.com/rhs0266/FastCampus/tree/main/%EA%B0%95%EC%9D%98%20%EC%9E%90%EB%A3%8C/02-%EC%95%8C%EA%B3%A0%EB%A6%AC%EC%A6%98/09~11-%EA%B7%B8%EB%9E%98%ED%94%84%20%ED%83%90%EC%83%89/%EB%AC%B8%EC%A0%9C%EB%B3%84%20%EC%BD%94%EB%93%9C/14502-%EC%97%B0%EA%B5%AC%EC%86%8C) |
| 미로 탐색 | [링크](https://www.acmicpc.net/problem/2178) | [링크](https://github.com/rhs0266/FastCampus/tree/main/%EA%B0%95%EC%9D%98%20%EC%9E%90%EB%A3%8C/02-%EC%95%8C%EA%B3%A0%EB%A6%AC%EC%A6%98/09~11-%EA%B7%B8%EB%9E%98%ED%94%84%20%ED%83%90%EC%83%89/%EB%AC%B8%EC%A0%9C%EB%B3%84%20%EC%BD%94%EB%93%9C/2178-%EB%AF%B8%EB%A1%9C%20%ED%83%90%EC%83%89) |
| 숨바꼭질 | [링크](https://www.acmicpc.net/problem/1697) | [링크](https://github.com/rhs0266/FastCampus/tree/main/%EA%B0%95%EC%9D%98%20%EC%9E%90%EB%A3%8C/02-%EC%95%8C%EA%B3%A0%EB%A6%AC%EC%A6%98/09~11-%EA%B7%B8%EB%9E%98%ED%94%84%20%ED%83%90%EC%83%89/%EB%AC%B8%EC%A0%9C%EB%B3%84%20%EC%BD%94%EB%93%9C/1697-%EC%88%A8%EB%B0%94%EA%BC%AD%EC%A7%88) |
| 탈출 | [링크](https://www.acmicpc.net/problem/3055) | [링크](https://github.com/rhs0266/FastCampus/tree/main/%EA%B0%95%EC%9D%98%20%EC%9E%90%EB%A3%8C/02-%EC%95%8C%EA%B3%A0%EB%A6%AC%EC%A6%98/09~11-%EA%B7%B8%EB%9E%98%ED%94%84%20%ED%83%90%EC%83%89/%EB%AC%B8%EC%A0%9C%EB%B3%84%20%EC%BD%94%EB%93%9C/3055-%ED%83%88%EC%B6%9C) |
| 유기농 배추 | [링크](https://www.acmicpc.net/problem/1012) | [링크](https://github.com/rhs0266/FastCampus/tree/main/%EA%B0%95%EC%9D%98%20%EC%9E%90%EB%A3%8C/02-%EC%95%8C%EA%B3%A0%EB%A6%AC%EC%A6%98/09~11-%EA%B7%B8%EB%9E%98%ED%94%84%20%ED%83%90%EC%83%89/%EB%AC%B8%EC%A0%9C%EB%B3%84%20%EC%BD%94%EB%93%9C/1012-%EC%9C%A0%EA%B8%B0%EB%86%8D%20%EB%B0%B0%EC%B6%94) |
| 연결 요소의 개수 | [링크](https://www.acmicpc.net/problem/11724) | [링크](https://github.com/rhs0266/FastCampus/tree/main/%EA%B0%95%EC%9D%98%20%EC%9E%90%EB%A3%8C/02-%EC%95%8C%EA%B3%A0%EB%A6%AC%EC%A6%98/09~11-%EA%B7%B8%EB%9E%98%ED%94%84%20%ED%83%90%EC%83%89/%EB%AC%B8%EC%A0%9C%EB%B3%84%20%EC%BD%94%EB%93%9C/11724-%EC%97%B0%EA%B2%B0%20%EC%9A%94%EC%86%8C%EC%9D%98%20%EA%B0%9C%EC%88%98) |
| 섬의개수 | [링크](https://www.acmicpc.net/problem/4963) | [링크](https://github.com/rhs0266/FastCampus/tree/main/%EA%B0%95%EC%9D%98%20%EC%9E%90%EB%A3%8C/02-%EC%95%8C%EA%B3%A0%EB%A6%AC%EC%A6%98/09~11-%EA%B7%B8%EB%9E%98%ED%94%84%20%ED%83%90%EC%83%89/%EB%AC%B8%EC%A0%9C%EB%B3%84%20%EC%BD%94%EB%93%9C/4963-%EC%84%AC%EC%9D%98%20%EA%B0%9C%EC%88%98) |
| 양 | [링크](https://www.acmicpc.net/problem/3184) | [링크](https://github.com/rhs0266/FastCampus/tree/main/%EA%B0%95%EC%9D%98%20%EC%9E%90%EB%A3%8C/02-%EC%95%8C%EA%B3%A0%EB%A6%AC%EC%A6%98/09~11-%EA%B7%B8%EB%9E%98%ED%94%84%20%ED%83%90%EC%83%89/%EB%AC%B8%EC%A0%9C%EB%B3%84%20%EC%BD%94%EB%93%9C/3184-%EC%96%91) |
| 바이러스 | [링크](https://www.acmicpc.net/problem/2606) | [링크](https://github.com/rhs0266/FastCampus/tree/main/%EA%B0%95%EC%9D%98%20%EC%9E%90%EB%A3%8C/02-%EC%95%8C%EA%B3%A0%EB%A6%AC%EC%A6%98/09~11-%EA%B7%B8%EB%9E%98%ED%94%84%20%ED%83%90%EC%83%89/%EB%AC%B8%EC%A0%9C%EB%B3%84%20%EC%BD%94%EB%93%9C/2606-%EB%B0%94%EC%9D%B4%EB%9F%AC%EC%8A%A4) |
| 경로찾기 | [링크](https://www.acmicpc.net/problem/11403) | [링크](https://github.com/rhs0266/FastCampus/tree/main/%EA%B0%95%EC%9D%98%20%EC%9E%90%EB%A3%8C/02-%EC%95%8C%EA%B3%A0%EB%A6%AC%EC%A6%98/09~11-%EA%B7%B8%EB%9E%98%ED%94%84%20%ED%83%90%EC%83%89/%EB%AC%B8%EC%A0%9C%EB%B3%84%20%EC%BD%94%EB%93%9C/11403-%EA%B2%BD%EB%A1%9C%20%EC%B0%BE%EA%B8%B0) |
| 트리의 부모 찾기 | [링크](https://www.acmicpc.net/problem/11725) | [링크](https://github.com/rhs0266/FastCampus/tree/main/%EA%B0%95%EC%9D%98%20%EC%9E%90%EB%A3%8C/02-%EC%95%8C%EA%B3%A0%EB%A6%AC%EC%A6%98/09~11-%EA%B7%B8%EB%9E%98%ED%94%84%20%ED%83%90%EC%83%89/%EB%AC%B8%EC%A0%9C%EB%B3%84%20%EC%BD%94%EB%93%9C/11725-%ED%8A%B8%EB%A6%AC%EC%9D%98%20%EB%B6%80%EB%AA%A8%20%EC%B0%BE%EA%B8%B0) |
| 나이트의 이동 | [링크](https://www.acmicpc.net/problem/7562) | [링크](https://github.com/rhs0266/FastCampus/tree/main/%EA%B0%95%EC%9D%98%20%EC%9E%90%EB%A3%8C/02-%EC%95%8C%EA%B3%A0%EB%A6%AC%EC%A6%98/09~11-%EA%B7%B8%EB%9E%98%ED%94%84%20%ED%83%90%EC%83%89/%EB%AC%B8%EC%A0%9C%EB%B3%84%20%EC%BD%94%EB%93%9C/7562-%EB%82%98%EC%9D%B4%ED%8A%B8%EC%9D%98%20%EC%9D%B4%EB%8F%99) |
| 촌수계산 | [링크](https://www.acmicpc.net/problem/2644) | [링크](https://github.com/rhs0266/FastCampus/tree/main/%EA%B0%95%EC%9D%98%20%EC%9E%90%EB%A3%8C/02-%EC%95%8C%EA%B3%A0%EB%A6%AC%EC%A6%98/09~11-%EA%B7%B8%EB%9E%98%ED%94%84%20%ED%83%90%EC%83%89/%EB%AC%B8%EC%A0%9C%EB%B3%84%20%EC%BD%94%EB%93%9C/2644-%EC%B4%8C%EC%88%98%20%EA%B3%84%EC%82%B0) |
| 현명한 나이트 | [링크](https://www.acmicpc.net/problem/18404) | [링크](https://github.com/rhs0266/FastCampus/tree/main/%EA%B0%95%EC%9D%98%20%EC%9E%90%EB%A3%8C/02-%EC%95%8C%EA%B3%A0%EB%A6%AC%EC%A6%98/09~11-%EA%B7%B8%EB%9E%98%ED%94%84%20%ED%83%90%EC%83%89/%EB%AC%B8%EC%A0%9C%EB%B3%84%20%EC%BD%94%EB%93%9C/18404-%ED%98%84%EB%AA%85%ED%95%9C%20%EB%82%98%EC%9D%B4%ED%8A%B8) |
| 케빈 베이컨의 6단계 법칙 | [링크](https://www.acmicpc.net/problem/1389) | [링크](https://github.com/rhs0266/FastCampus/tree/main/%EA%B0%95%EC%9D%98%20%EC%9E%90%EB%A3%8C/02-%EC%95%8C%EA%B3%A0%EB%A6%AC%EC%A6%98/09~11-%EA%B7%B8%EB%9E%98%ED%94%84%20%ED%83%90%EC%83%89/%EB%AC%B8%EC%A0%9C%EB%B3%84%20%EC%BD%94%EB%93%9C/1389-%EC%BC%80%EB%B9%88%20%EB%B2%A0%EC%9D%B4%EC%BB%A8%EC%9D%98%206%EB%8B%A8%EA%B3%84%20%EB%B2%95%EC%B9%99) |
| 결혼식 | [링크](https://www.acmicpc.net/problem/5567) | [링크](https://github.com/rhs0266/FastCampus/tree/main/%EA%B0%95%EC%9D%98%20%EC%9E%90%EB%A3%8C/02-%EC%95%8C%EA%B3%A0%EB%A6%AC%EC%A6%98/09~11-%EA%B7%B8%EB%9E%98%ED%94%84%20%ED%83%90%EC%83%89/%EB%AC%B8%EC%A0%9C%EB%B3%84%20%EC%BD%94%EB%93%9C/5567-%EA%B2%B0%ED%98%BC%EC%8B%9D) |
| 토마토 | [링크](https://www.acmicpc.net/problem/7569) | [링크](https://github.com/rhs0266/FastCampus/tree/main/%EA%B0%95%EC%9D%98%20%EC%9E%90%EB%A3%8C/02-%EC%95%8C%EA%B3%A0%EB%A6%AC%EC%A6%98/09~11-%EA%B7%B8%EB%9E%98%ED%94%84%20%ED%83%90%EC%83%89/%EB%AC%B8%EC%A0%9C%EB%B3%84%20%EC%BD%94%EB%93%9C/7569-%ED%86%A0%EB%A7%88%ED%86%A0) |

## 참고

[[Java] DFS, BFS 정리](https://bbangson.tistory.com/42)

[DFS, BFS의 설명, 차이점](https://velog.io/@lucky-korma/DFS-BFS%EC%9D%98-%EC%84%A4%EB%AA%85-%EC%B0%A8%EC%9D%B4%EC%A0%90#bfs-dfs-%EB%91%90%EA%B0%80%EC%A7%80-%EB%AA%A8%EB%91%90-%EA%B7%B8%EB%9E%98%ED%94%84%EB%A5%BC-%ED%83%90%EC%83%89%ED%95%98%EB%8A%94-%EB%B0%A9%EB%B2%95%EC%9E%85%EB%8B%88%EB%8B%A4)

[미로 찾기 알고리즘 BFS, DFS (with Python3)](https://www.google.com/url?sa=i&url=https%3A%2F%2Fmemostack.tistory.com%2F36&psig=AOvVaw0k16d8lcAzKQ3T1eSf0fqh&ust=1672186979994000&source=images&cd=vfe&ved=0CAMQjB1qFwoTCKiOkY_EmPwCFQAAAAAdAAAAABAD)

[그래프 탐색 알고리즘(Graph Search Algorithm)](https://jin1ib.tistory.com/entry/BFS-DFS-1)

[[알고리즘] 최소 신장 트리(MST, Minimum Spanning Tree)란 - Heee's Development Blog](https://gmlwjd9405.github.io/2018/08/28/algorithm-mst.html)
