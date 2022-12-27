# Tree

> 트리 하나의 뿌리에서 위로 뻗어 나가는 형상처럼 생겨서 
트리라는 명칭이 붙은 자료 구조
> 

- 선형적 자료구조에서 표현하기 어려운 계층적 데이터를 컴퓨터에서 표현한 구조
- 자기 참조 자료구조 - 트리는 자식도 트리고 또 그 자식도 트리
- 재귀적 특성 때문에 순회에서 트리에서는 재귀적 순회가 좀 더 자연스러운 편

## Tree 용어

![treeTerm](https://user-images.githubusercontent.com/86050295/209693199-3a0cbb72-45e7-459d-a0f6-74f5309e4561.png)

|  |  |
| --- | --- |
| 노드 | 트리를 구성하는 원소 |
| 간선 | 트리를 연결하는 선 |
| 루트 노드 | 가장 최상위 노드 |
| 리프 노드 | 가장 최하위 노드 |
| 높이 | 현재 위치에서부터 리프 노드 까지의 거리 |
| 깊이 | 루트노드에서 현재 노드까지의 거리 |
| 레벨 | 루트 노드(level=0)부터 노드까지 연결된 간선 수의 합 |
| 차수 | 자식 노드의 수 |
| 서브 트리 | 트리를 구성하는 내부 트리 |
| 형제 노드 | 같은 레벨에 부모가 같은 노드 |

## Graph vs Tree

- 순환 구조가 갖지 않는 그래프
- 부모에서 자식을 가리키는 단방향 그래프
- 루트 노드를 제외한 노드들은 단 하나의 부모 노드를 가진다.

![graph_tree](https://user-images.githubusercontent.com/86050295/209693336-0311bbb5-d800-48dc-8675-270b41d63742.png)


# Binary Tree - 이진 트리

m-ary 트리(다항 트리, 다진 트리)
각 노드가 m개 이하의 자식을 갖고 있으면, m-ary트리라고 한다.

$m=2$ 인 경우 특별히 이진 트리라고 한다. 즉, 자식 노드를 최대 두 개 가진 트리

이진 트리는 왼쪽, 오른쪽, 최대 2개의 자식을 갖는 매우 단순한 형태로 다진 트리에 비해 
훨씬 간결할 뿐만 아니라 여러 가지 알고리즘을 구현하는 일도 좀 더 간결하게 처리할 수 있어
대체로 트리라고 하면 특별한 경우가 아니고서는 대부분 이진트리를 일컫는다.

## 이진 트리 유형

![binaryTreeType](https://user-images.githubusercontent.com/86050295/209693430-cc248742-728e-47e1-88a5-c1f1706b4c1d.png)


### Full Binary Tree (정 이진 트리)

모든 노드가 0개 또는 2개의 자식을 갖는다.

### Complete Binary Tree (완전 이진 트리)

마지막 레벨을 제외하고 왼쪽 부터 모든 레벨이 완전히 채워진 이진 트리

### Perfect Binary Tree (포화 이진 트리)

모든 노드가 2개의 자식 노드를 갖고 있으며, 
모든 리프 노드가 동일한 깊이 또는 레벨을 갖는다.
문자 그대로, 가장 완벽한 유형의 트리

## 트리 순회

> 각 노드를 정확히 한 번 방문하는 과정


이진 트리에서 노드의 방문 순서에 따라 다음과 같이 3가지 방시으로 구분된다.

- 전위 순회(NLR) : 현재 노드를 먼저 순회한 다음 왼쪽과 오른쪽 서브트리를 순회
- 중위 순회(LNR) : 왼쪽 서브트리를 순회한 다음 현재 노드와 오른쯕 서브트리를 순회
- 후위 순회(LRN) : 왼쪽 서브트리와 오른쪽 서브트리 현재 노드 순서로 순회

![treeSearch](https://user-images.githubusercontent.com/86050295/209693580-beda9e84-d766-4f5d-8325-35f4e873265a.png)


전위 - F, B, A, D, C, E, G, I, H

중위 - A, B, C, D, E, F, G, H, I

후위 - A, C, E, D, B, H, I, G, F



# BST - Binary Search Tree (이진 탐색 트리)

노드의 왼쪽 서브트리에는 그 노드의 값보다 작은 값들을 지닌 노드들로 이뤄져 있고, 
노드의 오른쪽 서브 트리에는 그 노드의 값보다 큰 값들을 지닌 노드들로 이루어져 있는 트리

이진 트리의 시간복잡도

|  | 평균 | 최악의 경우 |
| --- | --- | --- |
| 검색 | $O(\log n)$ | $O(n)$ |
| 삽입 | $O(\log n)$ | $O(n)$ |
| 삭제 | $O(\log n)$ | $O(n)$ |

![BSTSearch](https://user-images.githubusercontent.com/86050295/209693805-ad3f4433-5c87-48c4-a033-3549d9e42011.png)

탐색 시 평균 시간 복잡도가 $O(\log n)$

![BSTSearchWorst](https://user-images.githubusercontent.com/86050295/209693864-2264d416-1f96-47de-a60c-e14240b517d7.png)

균형을 이루지 못한 경우 최악의 경우 $O(n)$
이 경우 연결 리스트와 다르지 않다

## 자가 균형 이진 탐색 트리

> 삽입, 삭제 시 자동으로 높이를 작게 유지하는 노드 기반의 이진 탐색 트리
> 

![balancedTree](https://user-images.githubusercontent.com/86050295/209693980-86c97165-40fd-4347-a91b-419c5810becf.png)


- 자가 균형 이진 탐색 트리는 최악의 경우에도 삽입/삭제시 이진 트리의 균형이 잘 맞도록 유지
- 모든 노드의 좌우 서브 트리 높이가 1이상 차이가 나지 않도록 하는 트리
- 높이를 가능한 한 낮게 유지하는 것이 중요

# AVL Tree 균형 트리

불균형한 이진 트리를 균형있게 유지하기 위해, 자료를 넣을 때마다, 균형을 잡아주는 트리

AVL 트리의 시간복잡도

|  | 평균 | 최악의 경우 |
| --- | --- | --- |
| 검색 | $O(\log n)$ | $O(\log n)$ |
| 삽입 | $O(\log n)$ | $O(\log n)$ |
| 삭제 | $O(\log n)$ | $O(\log n)$ |

## AVL 트리 - 리밸런싱

- BF(Balance Factor) = 왼쪽 서브 트리 높이 - 오른쪽 서브트리 높이
- BF를 [1, 0, -1]만 가지게 하여 균형을 유지
- BF > 1 이면, 왼쪽 서브 트리에 이상이 있음
- BF < 1 이면, 오른쪽 서브 트리에 이상이 있음

### 회전 연산

- 단순 회전 - LL, RR
- 이중 회전 - LR, RL

### LL (Left - Left)

오른쪽 방향으로 회전 1회

![LL](https://user-images.githubusercontent.com/86050295/209694108-7d6a6cca-75a8-4066-b318-4df40d45d159.png)


### RR (Right - Right)

왼쪽 방향으로 회전 1회

![RR](https://user-images.githubusercontent.com/86050295/209694130-f61db526-ca8f-47b7-86f8-ce5040608467.png)


### LR (Left - Right)

RR 회전 후 LL 회전 2회

![LR](https://user-images.githubusercontent.com/86050295/209694147-9548ab0c-94a3-400d-9822-5c609af6da76.png)

### RL (Right - Left)

LL 회전 후 RR 회전 2회

![RL](https://user-images.githubusercontent.com/86050295/209694166-ba8f4527-2eec-4d25-bc62-893c95abeea5.png)


# 참고 자료

파이썬 알고리즘 인터뷰

[[자료구조] AVL트리란? AVL트리 쉽게 이해하기, AVL트리 시뮬레이터](https://code-lab1.tistory.com/61)

[[Data Structure] AVL(Adelson-Velsky and Landis) 트리란?](https://fomaios.tistory.com/entry/Data-Structure-AVLAdelson-Velsky-and-Landis-%ED%8A%B8%EB%A6%AC%EB%9E%80)

[[알고리즘] AVL Tree(트리) : 필수기본정리 - Balanced Factor, 4가지 Rotation(회전), 삽입, 제거, Kotlin구현](https://underdog11.tistory.com/entry/%EC%95%8C%EA%B3%A0%EB%A6%AC%EC%A6%98-AVL-Tree%ED%8A%B8%EB%A6%AC-%ED%95%84%EC%88%98%EA%B8%B0%EB%B3%B8%EC%A0%95%EB%A6%AC-AVL%ED%8A%B8%EB%A6%AC-%EA%B0%9C%EB%85%90-Balanced-Factor-4%EA%B0%80%EC%A7%80-Rotation%ED%9A%8C%EC%A0%84-%EC%82%BD%EC%9E%85-%EC%A0%9C%EA%B1%B0-Kotlin%EA%B5%AC%ED%98%84)

[자료구조 - AVL Tree](https://walbatrossw.github.io/data-structure/2018/10/26/ds-avl-tree.html)
