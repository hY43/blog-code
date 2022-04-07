# Equals Method

## 1. String.equals vs Object.equals
equals는 기본적으로 해당 객체의 값이 같은지 확인하는 메소드로 알고 있다.

그런데 String에서의 equals 역시 해당 객체의 값을 비교할까? 이에 대해 한번 확인해 보자.

- String.equals
    - 오라클의 공식 문서를 살펴보면 아래와 같이 작성되어 있다.
        ```
        Compares this string to the specified object. 
        The result is true if and only if the argument is not null and is a String object that represents the same sequence of characters as this object.
        ```
        간단히 번역해보면 equals를 호출한 string과 전달 받은 매개 변수의 object 객체를 비교하여 매개 변수가 null이 아니고, object 객체가 같은 문자열로 되어있다면 true를 반환한다.
        라는 말인데 쉽게 말해 object가 null이 아니고 호출한 String 객체의 문자열과 object 객체의 문자열이 일치하면 true를 반환한다는 의미이다.

    - 추가로 오버라이드 정보를 보면 아래와 같이 Object 클래스의 equals를 override 하고 있다.
- Object.equals 
    - 오라클의 공식 문서를 살펴보면 아래와 같이 작성되어 있다.
        ```
        Indicates whether some other object is "equal to" this one.
        The equals method implements an equivalence relation on non-null object references:

        It is reflexive: for any non-null reference value x, x.equals(x) should return true.
        It is symmetric: for any non-null reference values x and y, x.equals(y) should return true if and only if y.equals(x) returns true.
        It is transitive: for any non-null reference values x, y, and z, if x.equals(y) returns true and y.equals(z) returns true, then x.equals(z) should return true.
        It is consistent: for any non-null reference values x and y, multiple invocations of x.equals(y) consistently return true or consistently return false, provided no information used in equals comparisons on the objects is modified.
        For any non-null reference value x, x.equals(null) should return false.
        The equals method for class Object implements the most discriminating possible equivalence relation on objects; that is, for any non-null reference values x and y, this method returns true if and only if x and y refer to the same object (x == y has the value true).

        Note that it is generally necessary to override the hashCode method whenever this method is overridden, so as to maintain the general contract for the hashCode method, which states that equal objects must have equal hash codes.
        ```
        내용이 엄청나게 복잡해보이지만 간단히 정리해보면 아래와 같다.
        
        Object.equals 는 해당 객체와 다른 객체가 같은지 다른지에 대해 판별하는 메소드이다.
        
        그리고 이 equals method는 null이 아닌 객체가 서로 같은지에 대해 구현하며 이를 위해 구현되어야 하는 조건 4가지에 대해 설명하고 있다.
        해당 조건에 등장하는 x, y, z는 전부 null이 아닌 객체를 의미한다.
        - x.equals(x) == true
        - x.equals(y) == true 이면 y.equals(x) == true
        - x.equals(y) == true 이고, y.equals(z) == true 이면, x.equals(z) == true
        - equals에서 객체의 정보가 중간에 수정되지 않았다는 전제하에, x.equals(y)는 몇번을 실행하던 동일한 결과를 반환해야 한다.
        - equals method는 내부적으로 x와 y가 동일한 객체인지 판별을 위해 ==를 사용한다.
        - equals method를 override 하게 되면 hashCode method 역시 함께 override 해야 한다.
- String.equals vs Object.equals
    - String은 Object의 equals method를 override를 하여 사용하고 있는데, override를 하면서 내부적으로 전달 받은 매개 변수가 String 이 맞는지 그리고 문자열의 길이가 동일한지를 비교하고, 마지막으로 실제 문자열을 한글자씩 비교하게된다.

## 2. Object.equals vs hashcode
