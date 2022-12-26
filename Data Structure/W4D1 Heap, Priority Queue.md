## Heap 

![제목 없음](https://user-images.githubusercontent.com/103614357/209045681-29a2cfd5-c6a1-442d-af3d-1f31a2fd3ef5.png)   

- Heap 자료구조 : `완전 이진 트리`를 기초로 하는 자료구조
  - 완전 이진트리 : 모든 노드에서 마지막을 제외한 자식들이 꽉 채워진 이진트리
    - 트리 : 부모-자식처럼 계층적인 형태를 가지는 구조
- Heap tree는 중복값 허용
  - Heap은 최댓값 또는 최솟값을 쉽게 뽑기 위한 자료구조임으로 중복 허용(이진 탐색 트리는 중복값 허용X) 

![제목 없음](https://user-images.githubusercontent.com/103614357/209047374-6d604189-3f82-4257-96f8-1810522498c1.png)   

- Heap은 최대힙(Max heap)과 최소힙(Min Heap)으로 나누어짐
  - `최대힙` : 부모노드의 key값이 자식노드들의 ke값보다 항상 큼      
  - `최소힙` : 부모노드의 ke값이 자식노드의 ke값보다 항상 작음  

<br/><br/>

## Priority Queue 우선순위 큐

![제목 없음](https://user-images.githubusercontent.com/103614357/209045109-0795a5d5-4225-4c42-a995-a647951c3569.png)   

- 일반적인 `Queue`는 First in-First Out 구조
  - 어떤 부가적인 조건 없이 먼저 들어온 데이터가 먼저 나가는 구조
- `Priority Queue`는 들어온 순서에 상관없이 `우선순위가 높은 데이터`가 먼저 나가는 것
  - 일반적인 Priority Queue 구현 방식에는 배열, 연결 리스트, 힙이 있음
  - `Heap 방식`으로 구현하는 것이 가장 효율적(최악의 경우라도 O(logN)을 보장)   

<br/>

- 참고) 왜 Priority Queue는 배열이나 연결리스트로 구현하지 않을까?  

  ![제목 없음](https://user-images.githubusercontent.com/103614357/209041747-4fbc3ac2-5c06-4d2a-bdd9-60f3dbe0ed81.png)     

  - Priority Queue를 `리스트`로 구현한다고 가정  
    - 데이터 저장을 하고 새로 삽입할 때마다 서로 비교하면서 정렬해주는 작업 필요 
    - `우선순위가 중간인 것`이 들어가야 하는 `삽입` 과정에서는     
      최악의 경우 `삽입해야 하는 위치를 찾기` 위해 모든 인덱스를 탐색해야함           
    - 이 때의 시간 복잡도는 자료가 N개라고 할 때 O(N)
    - 배열로 구현 시 시간 복잡도 : 삭제는 O(1), 삽입은 O(N)

<br/><br/>     

- 그럼 배열에 저장하되,     
  `퀵 정렬, 이진 탐색` 같은 방식을 이용하면 훨씬 빠르게 삽입/추출/탐색 가능하지 않을까?  

![제목 없음](https://user-images.githubusercontent.com/103614357/209045401-fd209a4b-5ed5-494e-a387-cf6a8bc956e3.png)   

- Priority Queue를 `Heap`으로 구현하면 됨!      
  - Heap의 경우 삭제나 삽입 과정에서 모두 `부모와 자식 간의 비교만` 계속 이루어짐     
  - 이진 트리의 높이가 하나 증가할 때마다 저장 가능한 자료의 갯수는 2배 증가하며, 비교 연산 횟수는 1회 증가   
  - 즉 삭제나 삽입 모두 최악의 경우에는 O(logN) 의 시간 복잡도를 가짐
  - 힙으로 구현 시 시간 복잡도 : 삭제는 O(logN), 삽입은 O(logN)

<br/>

=> 이처럼 배열이나 연결 리스트가 `삭제`에서는 시간 복잡도면에서 유리하지만,       
`삽입`의 시간 복잡도가 Heap 기반이 월등하기 때문에,      
편차가 심한 배열과 연결리스트보다는 힙으로 구현하는 것!  

<br/><br/>

### Priority Queue와 Heap의 관계  
- `Heap의 key`를 `우선순위로 사용`한다면 Heap은 Priority Queue의 구현체가 됨  
- Priority Queue : `ADT`(Abstract Data Type 추상자료형)      
  => 실제로 구현을 설명하지 않고, 어떤 기능들이 있는지 개념적인것만 설명(순수한 기능이 무엇인지 나열한 것)
- Heap : `data structure`     
  => 구현까지 있음

<br/><br/>

## 우선순위 큐(최대 힙)에서 정렬, 삽입, 삭제  

### Heapify  

- Heap 속성을 유지하는 작업
  - 최대힙의 조건을 만족할 수 있게 노드들의 위치를 바꿔가며 heap을 재구조화      

![제목 없음](https://user-images.githubusercontent.com/103614357/209044293-b7452472-1a04-42af-bc05-21db53fdec3e.png)  

- 부모-자식 노드 간 비교  
  - 부모 < 자식 -> 교환  
  
<br/>

### 요소 삽입

![제목 없음](https://user-images.githubusercontent.com/103614357/209043668-61d0a2e5-e9e1-48f6-afdf-8ca33ee41478.png)      

1. heap 끝에 새 요소 삽입
2. 부모 노드와 비교하여 힙 속성을 위배하는 경우 부모 노드와 값 교환
3. 힙 속성이 유지될 때까지 2번 작업 반복   

<br/>

### 요소 삭제

![제목 없음](https://user-images.githubusercontent.com/103614357/209044803-5a585ffe-44c5-49a7-9a0a-8e326f2661cf.png)   

1. root node 값 추출
2. heap 마지막 요소를 root nod에 배치
3. 마지막 요소(root node였던 애) 제거
4. 새로운 root node부터 heapify 수행  

<br/><br/>

## Priority Queue와 Heap의 사용사례  
**Process scheduling**     
- multi-tasking(여러 process들이 번갈아가며 실행됨)  
  - 하나의 process가 CPU를 사용중이라면 나머지 process는 `ready queue`에서 대기 
    - 이 때, 대기 중인 process가 `우선순위에 따라 스케줄링`이 되어야한다면?       
      ready queue에 priority queue를 사용할 수 있음     
<br/>

**Heap sort**    
- 만약 10개의 `데이터를 정렬`해야한다면, 10개 데이터를 heap에 다 넣으면 정렬되어서 나오게됨  

<br/>

- 참고) heap 메모리의 heap과는 아무런 관련 없음  

<br/><br/>   

Reference     
https://www.youtube.com/watch?v=P-FTb1faxlo&ab_channel=%EC%89%AC%EC%9A%B4%EC%BD%94%EB%93%9C    
https://www.youtube.com/watch?v=AjFlp951nz0&ab_channel=%EB%8F%99%EB%B9%88%EB%82%98  
https://www.youtube.com/watch?v=iyl9bfp_8ag&ab_channel=%EB%8F%99%EB%B9%88%EB%82%98   
https://velog.io/@emplam27/%EC%9E%90%EB%A3%8C%EA%B5%AC%EC%A1%B0-%EA%B7%B8%EB%A6%BC%EC%9C%BC%EB%A1%9C-%EC%95%8C%EC%95%84%EB%B3%B4%EB%8A%94-%ED%9E%99Heap     
https://hongjw1938.tistory.com/22    
https://yoongrammer.tistory.com/81     
https://gmlwjd9405.github.io/2018/05/10/algorithm-heap-sort.html   

<br/>
