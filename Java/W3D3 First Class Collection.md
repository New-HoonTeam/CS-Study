일급 컬렉션 (First Class Collection)은 [객체지향 생활체조](https://jamie95.tistory.com/99)에서 처음 언급되었다.

### 객체지향 생활체조에 따르면 Collection을 래핑하고, 다른 멤버 변수가 없는 상태를 뜻한다.

```java
public class PostList {
private Map<Long, PostDto> posts;

public PostList(Map<Long, PostDto> posts) {
this.posts = posts;
  }
}
```

### 왜 Collection을 래핑 해야 할까? 다음의 이점을 얻을 수 있기 때문이다.

- 비즈니스에 종속적인 자료구조 생성
- Collection의 불변성 보장
- 상태와 행위를 한 곳에서 관리
- 컬렉션에 이름 부여


### 자세히 알아보자

- #### 비즈니스에 종속적인 자료구조 생성
    
    postList의 최대 크기는 10이어야 한다는 조건과, post의 키값이 중복되지 않아야 한다는 요구사항이 있을 경우 보통은 PostService에서 검증과정을 거친다. 
    이런 경우 모든 postList가 필요한 곳에서 모두 검증을 해주어야 한다. → 많은 문제를 야기한다.
    
    하지만 일급 컬렉션을 사용하게 되면 최대 크기가 10이고 키값이 중복되지 않는 List를 만들 수 있다.
    
- #### Collection의 불변성 보장
    
    final이 있는데 이게 왜 장점일까 생각이 듭니다. 사실 final 키워드는 재할당이 안될 뿐 값 변경은 가능하다.
    
    ```java
    @Test
    void 파이널키워드() {
    final Map<Long, String> map = new HashMap<>();
    
      map.put(1L, "이건");
      map.put(2L, "몰랐지");
    
      Assertions.assertThat(map.size()).isEqualTo(2);
    }
    ```
    
    따라서 재할당이 아닌 불변 Collection을 만들고 싶으면 일급 컬렉션을 생성 한 뒤, 변경 메서드를 만들지 않는 방법을 사용하자.
    
- #### 상태와 행위를 한 곳에서 관리
    
    만약 Post의 카테고리가 자바게시판, 파이썬게시판, 고랭게시판이 있고 게시판 별로 별도의 관리가 필요한 경우에 일급 컬렉션을 사용하면 상태와 로직을 한 곳에서 관리할 수 있다.
    
    ```java
    public class PostList {
    	private Map<Long, PostDto> posts;
    
    	public PostList (Map<Long, PostDto> posts) {
    	this.posts = posts;
    	}
    
    	public Long javaPostListMethod() {
    		return posts.stream()
    			.filter(post -> post.getCategory().equals(JAVA))
    			.sum();
    	}
    
    	public void pythonPostListMethod() {
    		어쩌구 저쩌구 로직
    	}
    
    	public void goPostListMethod() {
    		어쩌구 저쩌구 로직
    	}
    }
    ```
    
    일급 컬렉션을 사용하지 않는 경우에는 
    
    ```java
    Map<Long, PostDto> posts = new HashMap<>();
    
    posts.put(1L, "이건");
    posts.put(2L, "몰랐지");
    
    Long size = posts.stream()
    			.filter(post -> post.getCategory().equals(JAVA))
    			.sum();
    ;
    ```
    
    이와 같이 map과 메서드의 관계가 코드에서 전혀 표현되지 않는다.
    
    그리고 post의 카테고리에 따른 계산을 **강제**할 수 없다.
    
    수정이 필요할 때 관련 메서드를 찾아서 수정해야 한다.
    
- #### 컬렉션에 이름 부여
    
    ```java
    //1번
    Map<Long, PostDto> posts = ~~~
    
    //2번
    PostList posts = ~~~
    JavaPostList javaPosts = ~~~
    PythonPostList pythonPosts = ~~~
    ```
    
    1번과 달리 2번은 명확한 표현으로 posts를 나타낼 수 있다.
    
    명확한 표현을 사용하기 때문에 검색에도 용이하다.
    

참고

---

[일급 컬렉션 (First Class Collection)의 소개와 써야할 이유](https://jojoldu.tistory.com/412)
