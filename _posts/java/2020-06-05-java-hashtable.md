---
published: true
permalink: /java/hashtable
comments: true
categories:
  - java
title: '[java] Hashtable'
---

자바 Hashtable  

## Reference
- [Docs](https://docs.oracle.com/javase/8/docs/api/java/util/Hashtable.html) 


## Notes
1) Constructors 
- `Hashtable()` : 빈 hashtable 생성 
- `Hashtable(int initialCapacity)` : 특정 크기의 hashtable 생성 
- `Hashtable(int initialCapacity, float loadFactor)`: 특정 크기와 특정 load factor(부하율)의hashtable 생성 
- `Hashtable(Map<? extends K,? extends V> t)` : 주어진 Map 형식의 Hashtable 생성 
  - `Hashtable<String, Integer> numbers = new Hashtable<String, Integer>();`
  

2) 자주 쓰는 Methods
  - 값 추가하거나 삭제할 때 
    - `put(K key, V value)` : entry 추가 
      -  `numbers.put("first", 0);` 
    - `replace(K key, V value)` : entry 수정 
      -  `numbers.replace("first", 1);` 
    - `remove(Object key)` : entry 삭제 
      -  `numbers.remove("first");`
    
  - 값 조회 
    - `get(Object key)` : entry 조회 
      -  `numbers.get("first");`
    
  - Key나 Value가 있는 지 확인할 때 
    - `containsKey(Object key)` : 특정 Key가 있는 지 확인 
      -  `numbers.containsKey("first");`
    - `containsValue(Object value)` : 특정 Value가 있는 지 확인 
      - `numbers.containsValue("first");`
