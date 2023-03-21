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

## computeIfAbsent()

```java
V computeIfAbsent(K key,Function<? super K, ? extends V> mappingFunction)
```

* key : Map의 key값
* mappingFunction : null이면 Exception 발생
* key값이 존재하면 
    * key값의 value값을 리턴 
* key값이 존재하지 않으면
    * Map에 key, value(mappingFunction 실행결과)를 저장하고 저장한 value를 리턴한다.

```java
HashMap<String, String> map = new HashMap<>();
String value = map.computeIfAbsent("A", "A_VALUE"); // A_VALUE
```
---

> compute(), computeIfPresent(), merge()는 Map의 value를 업데이트한다. 

## compute()
```java
V compute(K key, BiFunction<? super K, ? super V, ? extends V> remappingFunction)
```
compute()는 Map의 key, BiFunction을 매개변수로 받고 
key가 존재하지 않을경우 NullPointerException이 발생하고 BiFunction의 결과가 
null일 경우에도 NPE가 발생합니다. Key가 존재할경우 BiFunctino의 결과로 
Value를 변경합니다. 

```java
HashMap<String, String> map = new HashMap<>();
map.compute("D", (key, value) -> {
            if(value.length() > 10) {
                return value.concat("OVER10");
            } else {
                return value.concat("LESS10");
            }
        });
```

## computeIfPresent()
```java
V computeIfPresent(K key, BiFunction<? super K, ? super V, ? extends V> remappingFunction) 
```
* key값이 존재하면
    * remappingFunction 함수의 결과로 value를 변경하고 리턴한다.
* key값이 존재하지 않으면 
    * null을 리턴한다.
```java
HashMap<String, String> map = new HashMap<>();
map.put("A", "A_VALUE");

String A_value = map.computeIfPresent("A", (key, value) -> value.concat("UPDATE")); // A_VALUE -> A_VALUE_UPDATE

String B_value = map.computeIfPresent("B", (key, value) -> value.concat("UPDATE")) // null
```
