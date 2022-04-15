## Thread Safety

Collection

    List
        safe : vector -> stack extends vector / Stack / HashTable / ConcurrentHashMap
        non-safe : arraylist / ArrayDeque / HashMap


Collections.synchronizedMap(new HashMap()) vs ConcurrentHashMap