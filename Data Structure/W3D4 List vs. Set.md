## List

> **An ordered collection**
> 
> 
> The user of this interface has precise control over where in the list each element is inserted. The user can **access elements by their integer index (poisition in the list)**, and search for elements in the list.
> 
- 특징
    - 순서가 있고 중복을 허용
    - 가변적인 배열 길이
- In Java
    - ArrayList
        - 단방향 포인터 구조
            - Index를 가지고 있어 검색이 빠르나 특정 객체 탐색은 순차적 탐색
            - 데이터 추가, 삭제시 임시 배열을 생성해 데이터 복사
        - 동기화를 고려하지 않았기 때문에 속도가 빠른 편
    - Vector
        - ArrayList (ver. OLD School)
        - Syncronized (동시성 보장, 그로 인한 속도 저하 발생 가능)
    - LinkedList
        - 양방향 포인터 구조
            - 각 노드가 이전, 다음 노드 상태만 공유
            - 빈번한 삽입, 삭제에 유리
            - 검색 자체가 처음부터 순회하기에 느린 편
        - getFirst, getLast, addFirst, addLast, removeFirst, removeLast

### 성능 비교

|  | ArrayList | LinkedList |
| --- | --- | --- |
| add | O(1) | O(1) |
| remove | O(n) | O(1) |
| get | O(1) | O(n) |
| contains | O(n) | O(n) |

## Set

> A collection that contains **no duplicate elements**.
> 
> 
> More formally, sets contain no pair of elements e1 and e2 such that e1.equals(e2), and at most one null element. As implied by its name, this interface models the mathematical set abstraction.
> 
- 특징
    - 일반적으로 순서가 없고 중복 허용 X
- In Java
    - HashSet
        - hashCode 값을 통해 중복 여부 확인
        - 내부적으로 HashMap 자료구조 사용 (Null 허용)
    
    ```java
    Set<String> a = new **HashSet**<>();
    a.add("가");
    a.add("나");
    a.add("다");
    a.add("라");
    a.add("마");
    a.add("바");
    
    Iterator<String> iterator = a.iterator();
    while(iterator.hasNext()) {
        System.out.println(iterator.next());
    }
    ```
    
    ```
    가
    다
    바
    나
    마
    라
    
    종료 코드 0(으)로 완료된 프로세스
    ```
    
    - SortedSet (TreeSet)
        - Set의 특징을 갖으며 정렬 기능 포함
        - 내부적으로 BST(Binary Search Tree, 이진 탐색 트리) 사용
            - 더 구체적으로는 **Red**-**Black**-Tree (검색 성능 향상)
    
    ```java
    Set<String> a = new **TreeSet**<>();
    a.add("바");
    a.add("마");
    a.add("라");
    a.add("다");
    a.add("나");
    a.add("가");
    
    Iterator<String> iterator = a.iterator();
    while (iterator.hasNext()) {
        System.out.println(iterator.next());
    }
    ```
    
    ```java
    가
    나
    다
    라
    마
    바
    
    종료 코드 0(으)로 완료된 프로세스
    ```
    
![IMG1](https://p1dgey.notion.site/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2F0d05ffce-02d3-4a2c-9516-7b3d55b4bcc0%2FUntitled.png?id=ca7d1527-9f20-42ee-aff3-a47562018e07&table=block&spaceId=b76551b9-9f24-4a91-9bcd-340caa404f60&width=1000&userId=&cache=v2)

### 성능 비교

|  | HashSet | TreeSet |
| --- | --- | --- |
| add | O(1) | O(log N) |
| contains | O(1) | O(log N) |
| next | O(h/n) | O(log N) |

## List vs. Set

|  | LIST | SET |
| --- | --- | --- |
| Duplicates | YES | NO |
| Order | Ordered | Depends on Implementation |
| Positional Access | YES | NO |

## 참고

[What is the difference between Set and List in java](https://www.edureka.co/community/2283/what-is-the-difference-between-set-and-list-in-java)

[자료 구조 list, set, map의 차이](https://milkoon1.tistory.com/44)

[[자료구조] 레드-블랙 트리(Red-Black Tree)란? | 레드-블랙 트리 쉽게 이해하기](https://code-lab1.tistory.com/62)

[자바 자료구조 성능 비교](https://www.grepiu.com/post/9)
