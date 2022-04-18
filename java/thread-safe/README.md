## Thread Safe

Collection

    List
        safe : vector -> stack extends vector / Stack / HashTable / ConcurrentHashMap
        non-safe : arraylist / ArrayDeque / HashMap


Collections.synchronizedMap(new HashMap()) vs ConcurrentHashMap

-> Thread Safe 하려면 synchronized 예약어가 사용되어야 한다.