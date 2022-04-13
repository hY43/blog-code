# Wrapper class

> Wrapper class 중 Integer를 사용하여 내용을 설명합니다.

## 1. parseInt Method
Integer.parseInt 의 경우, 내부에서 오버로딩된 parseInt method를 사용하여 입력 받은 문자열을 기본 10진수의 int type으로 변환하여 반환합니다.

## 2. valueOf Method
Integer.valueOf 의 경우, parseInt Method 과 동일하게 10진수의 int Type으로 변환하기는 하나, 이를 반환할때 Integer의 생성자를 통해 Integer Type 의 객체로 생성하여 반환을 합니다.

위에서는 Integer로만 설명을 하였으나, Byte, Double 등의 다른 Wrapper class 역시 parse method는 각각의 primitive Type으로 valueOf method는 Wrapper class로 객체를 생성하여 반환하는 것을 확인할 수 있습니다.

## 3. 그래서 Wrapper class는 왜 사용하는데?
뭐.. 일단 여러가지 이유가 있겠지만,
- Wrapper class를 사용하면 Number 라는 추상 클래스를 상속받아 사용하기 때문에,
MIN_VALUE, MAX_VALUE와 같은 공통의 상수를 사용하거나 하는 숫자에 필요한 다양한 기능을 확장하여 사용할 수 있습니다.
- Wrapper class는 reference Type이기 때문에 generic과 같이 기존의 primitive type으로는 사용할 수 없는 기능을 사용할 수 있게 해줍니다.