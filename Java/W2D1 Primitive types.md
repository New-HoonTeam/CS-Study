# Java의 Primitive type

# 자바에서 원시 타입

8가지가 존재

varchar(255)

| Name | Range | 크기 | 기본값 |
| --- | --- | --- | --- |
| byte | -128 to 127(i.e., -27 to 27 - 1) | 8 bits (Two's Complement) | 0 |
| short | -32768 to 32767(i.e., -215 to 215 - 1) | 16 bits (Two's Complement) | 0 |
| int | -2147483648 to 2147483647(i.e., -231 to 231 - 1) | 32 bits (Two's Complement) | 0 |
| long | -9223372036854775808 to 9223372036854775807(i.e., -263 to 263 - 1) | 64 bits (Two's Complement) | 0L |
| float | -3.4028235E+38 to -1.4E-45 ~ 1.4E-45 to 3.4028235E+38 | 32 bits (IEEE 754 Notation) | 0.0f |
| double | -1.7976931348623157E+308 to -4.9E-324 ~ 4.9E-324 to 1.7976931348623157E+308 | 64 bits (IEEE 754 Notation) | 0.0d |
| char | ‘\u0000’(0) ~ ‘\uffff’(65535)  | 16 bits | ‘\u0000’ |
| boolean | false, true | 1 bit | false |

## 기본 타입 vs 래퍼 클래스

위의 8가지 데이터 타입을 클래스로 포장해주는 클래스

기본 타입 그대로 사용하면 안되고 클래스로 표현을 해야 할 때 사용함

`java.lang` 패키지에 포함이 됨

| 기본 타입 | 래퍼(Wrapper) 클래스 |
| --- | --- |
| byte | Byte |
| short | Short |
| int | Integer |
| long | Long |
| float | Float |
| double | Double |
| char | Character |
| boolean | Boolean |

![Untitled](https://user-images.githubusercontent.com/39071638/207389776-a5a78e32-401a-45f0-a13e-7bee8f32e81d.png)
기본 타입 → 래퍼 클래스 : Boxing (박싱)

래퍼 클래스 → 기본 타입 : UnBoxing (언박싱)

## 박싱 언방싱은 왜 할까??

![Untitled 1](https://user-images.githubusercontent.com/39071638/207389796-6ce37a43-806b-441f-9fce-f1ab64ec7d4a.png)
Wrapper class 내부의 값은 변경할 수 없기 때문에 언박싱 과정을 거쳐서 연산을 진행해야 함..

자동 박싱/언박싱

![Untitled 2](https://user-images.githubusercontent.com/39071638/207389819-dff52560-cb34-4ef8-a7e1-eae5fd38c729.png)
기본 타입과 래퍼 클래스 사이에서 값을 방싱하거나 언박싱 하지 않아도 자동적으로 되는 것을 의미

boxing을 할 경우 heap 영역에 새로운 Integer 객체가 생성됨

# 자바에서 클래스 객체의 사이즈를 측정하는 방법

C/C++ 과는 다르게 자바에서는 객체의 크기를 직접 측정할 수 있는 연산자, 메소드가 존재하지 않음(sizeof와 같은 역할을 수행하는 게 없음)

자바에서 객체의 크기를 측정한다는 거는 RAM에서 사용할 대략적인 크기를 측정하는 것임[https://www.baeldung.com/java-size-of-object](https://www.baeldung.com/java-size-of-object)

## Instrumentation을 사용해서 객체의 크기 구하기

```jsx
public class InstrumentationAgent {
    private static volatile Instrumentation globalInstrumentation;

    public static void premain(final String agentArgs, final Instrumentation inst) {
        globalInstrumentation = inst;
    }

    public static long getObjectSize(final Object object) {
        if (globalInstrumentation == null) {
            throw new IllegalStateException("Agent not initialized.");
        }
        return globalInstrumentation.getObjectSize(object);
    }
}
```

```jsx
Premain-class: com.baeldung.objectsize.InstrumentationAgent

javac InstrumentationAgent.java
jar cmf MANIFEST.MF InstrumentationAgent.jar InstrumentationAgent.class
```

```jsx
public class InstrumentationExample {

    public static void printObjectSize(Object object) {
        System.out.println("Object type: " + object.getClass() +
          ", size: " + InstrumentationAgent.getObjectSize(object) + " bytes");
    }

    public static void main(String[] arguments) {
        String emptyString = "";
        String string = "Estimating Object Size Using Instrumentation";
        String[] stringArray = { emptyString, string, "com.baeldung" };
        String[] anotherStringArray = new String[100];
        List<String> stringList = new ArrayList<>();
        StringBuilder stringBuilder = new StringBuilder(100);
        int maxIntPrimitive = Integer.MAX_VALUE;
        int minIntPrimitive = Integer.MIN_VALUE;
        Integer maxInteger = Integer.MAX_VALUE;
        Integer minInteger = Integer.MIN_VALUE;
        long zeroLong = 0L;
        double zeroDouble = 0.0;
        boolean falseBoolean = false;
        Object object = new Object();

        class EmptyClass {
        }
        EmptyClass emptyClass = new EmptyClass();

        class StringClass {
            public String s;
        }
        StringClass stringClass = new StringClass();

        printObjectSize(emptyString);
        printObjectSize(string);
        printObjectSize(stringArray);
        printObjectSize(anotherStringArray);
        printObjectSize(stringList);
        printObjectSize(stringBuilder);
        printObjectSize(maxIntPrimitive);
        printObjectSize(minIntPrimitive);
        printObjectSize(maxInteger);
        printObjectSize(minInteger);
        printObjectSize(zeroLong);
        printObjectSize(zeroDouble);
        printObjectSize(falseBoolean);
        printObjectSize(Day.TUESDAY);
        printObjectSize(object);
        printObjectSize(emptyClass);
        printObjectSize(stringClass);
    }

    public enum Day {
        MONDAY, TUESDAY, WEDNESDAY, THURSDAY, FRIDAY, SATURDAY, SUNDAY
    }
}
```

```jsx
VM Options: -javaagent:"path_to_agent_directory\InstrumentationAgent.jar"
```

```jsx
Object type: class java.lang.String, size: 24 bytes
Object type: class java.lang.String, size: 24 bytes
Object type: class [Ljava.lang.String;, size: 32 bytes
Object type: class [Ljava.lang.String;, size: 416 bytes
Object type: class java.util.ArrayList, size: 24 bytes
Object type: class java.lang.StringBuilder, size: 24 bytes
Object type: class java.lang.Integer, size: 16 bytes
Object type: class java.lang.Integer, size: 16 bytes
Object type: class java.lang.Integer, size: 16 bytes
Object type: class java.lang.Integer, size: 16 bytes
Object type: class java.lang.Long, size: 24 bytes
Object type: class java.lang.Double, size: 24 bytes
Object type: class java.lang.Boolean, size: 16 bytes
Object type: class com.baeldung.objectsize.InstrumentationExample$Day, size: 24 bytes
Object type: class java.lang.Object, size: 16 bytes
Object type: class com.baeldung.objectsize.InstrumentationExample$1EmptyClass, size: 16 bytes
Object type: class com.baeldung.objectsize.InstrumentationExample$1StringClass, size: 16 bytes
```

sizeof(int) → 4
