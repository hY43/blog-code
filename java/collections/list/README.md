# ArrayList

## 1. 사이즈를 초과하도록 데이터를 넣으면, 내부적으로 어떻게 동작할까?
(오라클 공식문서)[] 기본 생성자를 사용하여 ArrayList 객체를 생성하면 

- 기본 생성자로 생성(10) -> 10 초과
- 생성자(5)로 생성 -> 5 초과

Collections.synchronizedList(new ArrayList());