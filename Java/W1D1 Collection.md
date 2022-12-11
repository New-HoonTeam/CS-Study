## Java Collection Framework(JCF)
- Java에서의 `Collection` : 데이터의 집합, 그룹
- `Java Collection Framework` : data를 저장하는 자료 구조 & data를 처리하는 알고리즘을 구조화하여 클래스로 구현해놓은 것        
  - 배열과 다르게 상황에 따라 객체의 수를 동적으로 정할 수 있음    
    => 프로그램의 공간적인 효율성 높여줌     
  - Java의 interface를 사용하여 구현됨 & 다수의 data를 다루는데 표준화된 클래스들을 제공          
    => DataStructure를 직접 구현하지 않고 편하게 사용할 수 있음    

<br/><br/>

### Collection framework 주요 interface
데이터를 저장하는 자료 구조에 따라 주요 인터페이스를 정의해보면,    
List interface / Set interface / Map interface   

![제목 없음](https://user-images.githubusercontent.com/103614357/206513702-c521012a-d316-4c4d-a38f-851b6e0d4cba.png)   

List, Set은 `Collection interface` 상속받지만, 구조상의 차이(key-value)로 인해 Map interface는 별도로 정의   
(Map은 Collection interface를 상속받고 있지 않지만 Collection으로 분류)   

<br/><br/>   

![제목 없음](https://user-images.githubusercontent.com/103614357/206519918-03a06f08-2677-4409-8de0-c095e7de068d.png)    
  
| 인터페이스   | 순서  | 중복  | 구현한 클래스들(Collection class)                                           |
| ------ | -------- | -------- | -------- |
| List<E> | 있음(index 라는 식별자) | O | ArrayList, LinkedList, Stack, Queue, Vector
| Set<E> | 없음 | X(데이터 유일성) | HashSet, TreeSet
| Map<K, V> | 있음 | key는 X, value는 O | HashMap, TreeMap, Hashtable, Properties

<br/><br/>
  
### Collection interface에서 제공하는 method
- `int size()` : 요소 총 개수
- `boolean add(E e)` : 요소 추가
- `boolean contains(Object o)` : 포함하고 있는지
- `boolean isEmpty()` : 비어있는지
- `void clear()` : 모든 요소 제거
- `boolean remove(Object o)` : 전달 객체 제거 후 `boolean 반환`  

<br/><br/>
  
### Collection class   
Collection framework에 속하는 interface를 구현한 class = List, Set, Map interface 중 하나 구현      
클래스 이름에 구현한 interface 이름 포함    
  
<br/>   
  
**1) ArrayList**   
- 단방향 포인터 구조, 각 데이터에 대한 `index` 가짐 -> `조회 기능` 성능 뛰어남
- 배열과 달리 초기크기를 지정하지 않아도 되므로, 삽입 삭제가 자유로움

<br/> 
   
**2) LinkedList**
- 양방향 포인터 구조, 다음 노드의 주소를 기억하고 있음   
  -> 데이터의 삽입, 삭제 시 데이터의 위치정보만 수정 하면 되기에 유용   
  -> but, 탐색은 첫번째 노드부터 다 탐색하기 때문에 느림   
- Stack, Queue 등을 만들기 위한 용도로 쓰임

<br/> 
  
**3) Vector**     
- 과거에 많이 사용
- 내부에서 자동으로 동기화처리 -> 비교적 성능 좋지 않음, 무거워 잘 쓰이지 않음

 <br/>

**4) HashSet**  
- 가장빠른 임의 접근 속도
- 순서를 예측할 수 없음
- HashMap에서 key값이 없는 자료형 집합이라고 생각해도 무방

<br/> 
  
**5) TreeSet**  
- 정렬 방법 지정가능

<br/> 

**6) Hashtable**  
- HashMap보다는 느리지만 동기화 지원
- null 불가

<br/> 
  
**7) HashMap**
- 중복과 순서가 허용되지 않음
- null값 가능

<br/> 
  
**8) TreeMap**
- 정렬된 순서대로 key와 value를 저장하여 검색이 빠름
  

<br/><br/>

### HashMap, HashTable, ConcurrentHashMap의 차이    
  
| 차이   | Hashtable  | HashMap  |  ConcurrentHashMap                                         
| ------ | -------- | ---------- | -------- 
| Thread-safe | O | X | O 
| key에 Null 값 허용 | X | O | X

<br/>
    
**ConcurrentHashMap**   
- HashMap의 동기화 문제를 보완  
- 동기화처리
  - `ConcurrentHashMap` 
    - map의 일부에만 lock (`Entry 아이템별로 락`을 걸어)   
    - 따라서, 데이터를 다루는 속도가 `빠름`
  - `HashTable`
    - map 전체에 lock(`쓰레드간 락`을 걸어)   
    - 따라서, 성능 오버헤드 + 데이터를 다루는 속도가 `느림`     
    - 하나의 쓰레드가 락을 유지하는 동안, 다른 모든 쓰레드는 컬렉션을 사용할 수 없음     
- Hashtable과는 다르게 put() 메서드에 synchronized 키워드가 붙어있지 않음

<br/>
  
=> HashTable보다 ConcurrentHashMap이 성능적으로 우수       

=> MultiThread 환경에서 동기화 처리가 필요하다면 ConcurrentHashMap을 사용     
  동기화가 필요하지 않은 경우라면 HashMap을 사용    
  [과정, 코드 참고](https://junghyungil.tistory.com/104)
  
<br/>
  
- 참고) [HashMap이 null을 허용하는 이유](https://jammdev.tistory.com/119)   
  
<br/>
  
- 참고) [Collection framework 시간복잡도](https://www.grepiu.com/post/9)   
  
<br/><br/>
  
### P.S Collections     
- Java 1.2이상부터 Collections라는 static class 존재(Collection interface를 구현한 클래스들)
- Collections 사용하기 위해서는 `java.util.Collections` import  
- Collection framework에 속하는 class를 지원해주는 다양한 method 존재  
  - `Collections.sort(List L)` : 리스트 정렬
  - `Collections.sort(List L , reverseOrder())` : 리스트 역순 정렬  
  - `Collections.max(List L)` , `min(List L)` : 리스트에서 최대, 최솟값
  - `Collections.shuffle(List L)` :	리스트 랜덤으로 섞음  
  - `Collections.binarySearch(List L , Key)` : 오름차순으로 정렬된 리스트에서 이진검색 통해 위치 반환(실패시 -1 반환)  
  - `Collections.disjoint(List L1 , List L2)` :	두 리스트 값이 완전히 다른지 검사 , 하나라도 같은값 있으면 False 
  
<br/><br/>

Reference    
http://www.tcpschool.com/java/java_collectionFramework_concept     
https://bangu4.tistory.com/194     
https://backendcode.tistory.com/163      
https://devlog-wjdrbs96.tistory.com/253   
https://junghyungil.tistory.com/104   

<br/>     
