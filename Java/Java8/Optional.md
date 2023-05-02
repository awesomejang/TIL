# __[JAVA] Optional__

## __Optional 이란?__


null-safe한 코드를 위해 도입된 개념으로 Optional은 데이터를 갖거나, null일 수 있는 객체를 감싸고 있다. 이를 통해 NullPointerException을 방지하여 코드의 안정성을 높일 수 
있다. 

<br/>

## __Optional 생성__

* of()
```java
// Optional.of(value)
// value가 null이 아닌 경우 -> Optional 생성
// Value가 null인 경우 -> NullPointerException발생
Optional<Ad> ad1 = Optional.of(ad);
```
* empty()
```java
// Optional.empty()
// 빈 Optional 생성
Optional<Object> empty = Optional.empty();
```
* ofNullable(value)
```java
// Optional.ofNullable(value)
// of()와 달리 value가 null일 경우 NPE발생이 아닌 비어있는 Optional 객체를 생성
Optional<String> optionalValue = Optional.ofNullable(value);
```


