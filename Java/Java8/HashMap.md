# JAVA8 HashMap 추가사항

* putIfAbsent()
* computeIfAbsent()
* compute()
* computeIfPresent()
* merge()
* getOrDefault()

---
<br/>

## putIfAbsent()

```java
hashmap.putIfAbsent(K key, V value)
```
* key : Map의 key값
* value : key의 value값
* key값이 존재하면 
    * key값의 value값을 리턴
* key값이 존재하지 않으면
    * Map에 key, value를 저장하고 null을 리턴한다. 

```java
HashMap<String, String> map = new HashMap<>();
String value = map.putIfAbsent("A", "A_VALUE"); // null
```


