# Generic

> 이 문서에서는 java 1.8을 기준으로 작성되었습니다.

## 1. Generic을 왜 사용하지?
[오라클의 튜토리얼 문서](https://docs.oracle.com/javase/tutorial/java/generics/why.html) 에 따르면 아래 3가지 이유 정도가 Generic 을 사용해서 얻을 수 있는 이득이라고 합니다.

첫번째 가장 큰 이유로는 *<span style="color:red">컴파일 시에 강력한 타입 체크</span>* 를 들수 있습니다.

일반적으로 형변환을 할때는 컴파일 시에 에러를 발견하지 못하고 런타임 상황에서 에러가 발견되는 경우가 있습니다.

그런데 만약 generic을 사용하게 되면 어떤 타입인지 컴파일 시에 확인을 하게 되고, 만약 타입이 맞지 않게되면 에러를 반환하여 컴파일이 종료됩니다.

```java
public class GenericTest {
    public static void main(String[] args) {
        ArrayList list = new ArrayList<>();
        list.add("1");
        list.add("2");
        
        // 중간에 이런 저런 소스가 들어갔다고 가정하겠습니다.

        list.add(3);
        System.out.println(list);

        String str0 = (String) list.get(0);
        String str1 = (String) list.get(1);
        String str2 = (String) list.get(2);

        System.out.println(str0 + str1 + str2);
    }
}
```
예를 들어 위와 같이 ArrayList 객체를 하나 생성하고 그 안에 String 타입과 int 타입의 데이터를 넣은 후 출력하는 코드를 작성하고 컴파일/실행을 하게 되면 아래와 같이 컴파일 시점엔 에러가 발생하지 않으나, 실행 시에 에러가 발생하는 것을 확인할 수 있습니다.

```
> javac d/generic/GenericTest.java 
Note: d/generic/GenericTest.java uses unchecked or unsafe operations.
Note: Recompile with -Xlint:unchecked for details.

> java d/generic/GenericTest
[1, 2, 3]
Exception in thread "main" java.lang.ClassCastException: class java.lang.Integer cannot be cast to class java.lang.String (java.lang.Integer and java.lang.String are in module java.base of loader 'bootstrap')
        at d.generic.GenericTest.main(GenericTest.java:15)
```

이를 방지하기 위해 아래와 같이 코드를 작성하게 되면 컴파일 시에 에러가 발생하여, 런타임 에러를 방지할 수 있습니다.
```java
public class GenericTest {
    public static void main(String[] args) {
        ArrayList<String> list = new ArrayList<String>();
        list.add("1");
        list.add("2");
        list.add(3);
        System.out.println(list);

        String str0 = list.get(0);
        String str1 = list.get(1);
        String str2 = list.get(2);

        System.out.println(str0 + str1 + str2);
    }
}
```

```
> javac d/generic/GenericTest.java 
d/generic/GenericTest.java:10: error: incompatible types: int cannot be converted to String
        list.add(3);
```

두번째로 얻을 수 있는 이점은 *<span style="color:red">별도의 형 변환이 필요없다</span>* 라는 것입니다.

위에서 사용한 예제를 확인해보면 기존에 generic을 사용하지 않을때와 달리 str0, str1, str2에 값을 대입할 때 String 타입으로 형 변환을 하지 않는 것을 확인할 수 있습니다.

마지막으로 얻을 수 있는 이점은 *<span style="color:red">collection 사용 시에 다양한 타입으로 선언하여 그에 해당하는 알고리즘을 사용할 수 있다</span>* 라는 점인데 이 부분은 아래와 같은 의미로 추측됩니다.
```java
public class GenericTest {
    public static void main(String[] args) {
        ArrayList<String> list1 = new ArrayList<String>();
        list1.add("1234");
        list1.add("5678");

        String subStr = list1.get(0).substring(2);
        System.out.println(subStr);

        ArrayList<Integer> list2 = new ArrayList<Integer>();
        list2.add(1234);
        list2.add(5678);

        System.out.println("Integer 최소 값은 " + list2.get(0).MIN_VALUE + "입니다.");
    }
}
```
위와 같이 ArrayList가 String 타입이면 String 타입에 해당되는 기능을, Integer 타입이면 Integer 타입에 해당되는 기능을 사용할 수 있는 것을 확인할 수 있습니다.

그외에도 생각해보면, 타입을 직접 명시하기 때문에 *<span style="color:red">유지보수 시에 그 타입을 직접 눈으로 확인할 수 있다</span>* 는 부분도 장점이 될 것같습니다.

## 2. Wildcard Generic
> 아래 예제는 자바의 신 3판(이상민 저)를 참고하여 작성하였습니다. 좀더 자세한 내용은 자바의 신을 참조 바랍니다.

wildcard generic이란 기존의 generic 타입을 정확하게 명시했다면, 그렇게 사용할 수 없는 상황에서 타입을 제한하지 않고 generic을 사용할 수 있도록 **타입 대신 ? 문자를 사용**하는 것을 말합니다.

그런데 타입 형변환에 대해 명시적으로 사용하기 위해 generic을 사용하는데, 굳이 타입을 명확하게 둘 수 없는 wildcard generic을 사용하는 이유가 뭘까요?

한 가지 예를 을어 사례를 확인해보면 아래와 같습니다.
```java
// WildcardGeneric.java
public class WildcardGeneric<T> {
    private T object;

    public T getObject() {
        return object;
    }

    public void setObject(T object) {
        this.object = object;
    }
}

// WildcardGenericMain.java
public class WildcardGenericMain {
    public static void main(String[] args) {
        WildcardGenericMain main = new WildcardGenericMain();
        WildcardGeneric<String> generic = new WildcardGeneric<String>();
        generic.setObject("object");
        main.checkGenericMethod(generic);
    }

    public void checkGenericMethod(WildcardGeneric<String> generic) {
        System.out.println(generic.getObject());
    }
}
```
위의 상황에서 만약 Integer 타입으로 선언된 WildcardGeneric<Integer>를 사용하기는 어려운 부분이 있습니다.
이때 아래와 같이 wildcard generic을 사용하면 아래와 같이 처리할 수 있습니다.

```java
// WildcardGenericMain.java
public class WildcardGenericMain {
    public static void main(String[] args) {
        WildcardGenericMain main = new WildcardGenericMain();
        WildcardGeneric<String> generic = new WildcardGeneric<String>();
        generic.setObject("object");
        main.checkGenericMethod(generic);
    }

    public void checkGenericMethod(WildcardGeneric<?> generic) {
        Object value = generic.getObject();
        if(value instanceof String) {
            System.out.println(generic.getObject());    
        } else if (value instanceof Integer) {
            System.out.println(generic.getObject());
        }
    }
}
```

위와 같은 상황에서 사용을 주로 하기 때문에 wildcard generic은 일반적인 지역변수 보다는 메소드의 매개 변수에 주로 사용합니다.

어떤 타입이던 받을 수 있기 때문에 좀더 명확한 조건을 위해 아래와 같이 제한을 둘 수도 있습니다.
```java
// WildcardGenericMain.java
public class WildcardGenericMain {
    public static void main(String[] args) {
        WildcardGenericMain main = new WildcardGenericMain();
        WildcardGeneric<String> generic = new WildcardGeneric<String>();
        generic.setObject("object");
        main.checkGenericMethod(generic);
    }

    public void checkGenericMethod(WildcardGeneric<? extends Object> generic) {
        Object value = generic.getObject();
        if(value instanceof String) {
            System.out.println(generic.getObject());    
        } else if (value instanceof Integer) {
            System.out.println(generic.getObject());
        }
    }
}
```
? 뒤에 extends 키워드를 둬서 해당 클래스의 확장 클래스만 사용할 수 있다라는 제약을 둘 수도 있습니다.

```
정리해보면 generic은 기본적으로 형 변환에 대한 이득을 취하기 위해 사용되는데, 메소드의 매개변수에 대해서는 여러 타입을 받아야한다면 wildcard generic을 사용하여 좀더 폭넓게 사용할 수 있습니다.
```
