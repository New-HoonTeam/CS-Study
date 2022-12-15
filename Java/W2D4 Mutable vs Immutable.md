# mutable Object

- 객체 생성이후 필드 값을 초기화한 후 내부 상태를 변경할 수 있는 객체
- 쓰레드 safe하지 않음 → 별도의 동기화 작업이 필요함

```java
public class MutableObject {
    private String name;
    private int count;

    MutableObject(String name, int count) {
        this.name = name;
        this.count = count;
    }

    public int getNum() {
        return num;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public void setNum(int num) {
        this.num = num;
    }
}
```

# Immutable Object

- 객체 생성 이후 내부의 상태가 변하지 않는 객체
- 신뢰성 있는 객체를 만들 수 있다
    - 실패 원자적 method를 만들 수 있다.
    - side effect 최소화

- thread - safe 하여 별도의 동기화 작업이 필요 없다.

## 무엇이 불변객체인가?

### Case 1. ImmutableObject1 : 불변객체 O

```java
public class ImmutableObject1 {
    private final String name;
    private final int count;

    MutableObject(String name, int count) {
        this.name = name;
        this.count = count;
    }

// 필요하다면 getter, setter는 안된다.
}
```

### Case 2. ImmutableObject2 : 불변객체 X

```java
public class ImmutableObject2 {
    private final String name;
    private final int count;
    private final InnerObject innerObject;

    public ImmutableObject2(String name, int count, InnerObject innerObject) {
        this.name = name;
        this.count = count;
        this.innerObject = innerObject;
    }
}
```

```java
public class InnerObject {
    private String innerName;
    
    public InnerObject(String innerName) {
        this.innerName = innerName;
    }

    public void setInnerName(String innerName) {
        this.innerName = innerName;
    }
}
```

⇒ 불변객체를 구성하는 객체도 불변객체로 만들어야 한다.

### Case3 - 1. ImmutableObject3 : 불변객체 X

```java
public class ImmutableObject3 {
    private final String name;
    private final int count;
    private final List<Integer> nums;

    public ImmutableObject3(String name, int count, List<Integer> nums) {
        this.name = name;
        this.count = count;
        this.nums = nums;
    }

		@Override
    public String toString() {
        return "ImmutableObject3{" +
                "name='" + name + '\'' +
                ", count=" + count +
                ", nums=" + nums +
                '}';
    }
}
```

⇒ 외부(`nums`)의 변화에 영향을 받을 수 있다.

```java
public class Main {
    public static void main(String[] args) {
        List<Integer> nums = new ArrayList<>(Arrays.asList(1, 2, 3));

        ImmutableObject3 object3 = new ImmutableObject3("IM", 1, nums);

        System.out.println(object3);
        nums.add(4);
        System.out.println(object3);
    }
}
```

```
ImmutableObject3{name='IM', count=1, nums=[1, 2, 3]}
ImmutableObject3{name='IM', count=1, nums=[1, 2, 3, 4]}
```

⇒ 방어적 복사를 통해 초기화 시켜주어야 한다.

```java
public class ImmutableObject3 {
    private final String name;
    private final int count;
    private final List<Integer> nums;

    public ImmutableObject3(String name, int count, List<Integer> nums) {
        this.name = name;
        this.count = count;
        this.nums = new ArrayList<>(nums);
    }

    @Override
    public String toString() {
        return "ImmutableObject3{" +
                "name='" + name + '\'' +
                ", count=" + count +
                ", nums=" + nums +
                '}';
    }
}
```

```
ImmutableObject3{name='IM', count=1, nums=[1, 2, 3]}
ImmutableObject3{name='IM', count=1, nums=[1, 2, 3]}
```

### Case3 - 2. ImmutableObject3 : 불변객체 X

```java
public class ImmutableObject3 {
    private final String name;
    private final int count;
    private final List<Integer> nums;

    public ImmutableObject3(String name, int count, List<Integer> nums) {
        this.name = name;
        this.count = count;
        this.nums = new ArrayList<>(nums);
    }

    public List<Integer> getNums() {
        return nums;
    }

    @Override
    public String toString() {
        return "ImmutableObject3{" +
                "name='" + name + '\'' +
                ", count=" + count +
                ", nums=" + nums +
                '}';
    }
}
```

```java
public class Main {
    public static void main(String[] args) {
        List<Integer> nums = new ArrayList<>(Arrays.asList(1, 2, 3));

        ImmutableObject3 object3 = new ImmutableObject3("IM", 1, nums);

        System.out.println(object3);
        object3.getNums().add(4);
        System.out.println(object3);

    }
}
```

```
ImmutableObject3{name='IM', count=1, nums=[1, 2, 3]}
ImmutableObject3{name='IM', count=1, nums=[1, 2, 3, 4]}
```

⇒ getter또한 방어적 복사해주어야 한다.

```java
public class ImmutableObject3 {
    private final String name;
    private final int count;
    private final List<Integer> nums;

    public ImmutableObject3(String name, int count, List<Integer> nums) {
        this.name = name;
        this.count = count;
        this.nums = new ArrayList<>(nums);
    }

    public List<Integer> getNums() {
        return new ArrayList<>(nums);
    }

    @Override
    public String toString() {
        return "ImmutableObject3{" +
                "name='" + name + '\'' +
                ", count=" + count +
                ", nums=" + nums +
                '}';
    }
}
```

```
ImmutableObject3{name='IM', count=1, nums=[1, 2, 3]}
ImmutableObject3{name='IM', count=1, nums=[1, 2, 3]}
```

# 불변 객체를 만드는 방법

- 모든 필드에 대해 final을 설정한다.
- 필드에 참조 타입이 있을 경우, 해당 객체도 불변성을 보장해야 한다.
- 필드에 컬렉션이 존재할 경우, 생성자 및 getter에 대해 방어적 복사를 수행해야 한다.

[[Java] 가변 객체 vs 불변 객체](https://steady-coding.tistory.com/559)

[[Java] 불변 객체, final을 사용해야 하는 이유 (Immutable Object)](https://sudo-minz.tistory.com/136)

[불변객체를 만드는 방법](https://tecoble.techcourse.co.kr/post/2020-05-18-immutable-object/)

[[Java] 불변 객체(Immutable Object) 에 대해 알아보자](https://devoong2.tistory.com/entry/Java-%EB%B6%88%EB%B3%80-%EA%B0%9D%EC%B2%B4Immutable-Object-%EC%97%90-%EB%8C%80%ED%95%B4-%EC%95%8C%EC%95%84%EB%B3%B4%EC%9E%90)
