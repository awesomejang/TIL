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

<br/>

## __Optional 데이터 추출__
* get()
    * Optional 객체 안의 데이터를 리턴, 데이터가 없는 경우 예외(NoSuchElementException)이 발생
```java
String value = Optional.of("value").get();
```
* orElse()
    * 데이터가 null일 경우 기본값을 설정할 수 있다. 
    * 데이터가 null이 아니라면 랩핑된 데이터를 리턴한다.
```java

Object default_value = Optional.empty()
                               .orElse("default Value");
// "default Value"                               
System.out.println(default_value);
```
* orElseGet()
    * 데이터가 null일 경우 Supplier 함수로부터 데이터를 동적으로 설정할 수 있다. 
    * 데이터가 null이 아니라면 랩핑된 데이터를 리턴한다.
```java
Object value = Optional.empty()
                       .orElseGet(() -> "Supplier default Value");
// "Supplier default Value"                               
System.out.println(value);                       
```
* orElseThrow()
    * 데이터가 null일 경우 지정된 예외가 발생한다. 
```java
Optional<Object> optional = Optional.empty();
// RuntimeException이 발생한다.
Object value = optional.orElseThrow(() -> new RuntimeException("VALUE IS NULL!"));
System.out.println(value)
```

## __orElse(), orElseGet()의 차이점__
```java
public T orElse(T other) {
    return value != null ? value : other;
}

public T orElseGet(Supplier<? extends T> supplier) {
    return value != null ? value : supplier.get();
}
```
두 메소드를 보면 간단히 value가 null일 경우 파라미터로 받은 데이터를 리턴해주게 되어있는데 

orElse의 경우 파라미터 other가 메소드일때 유의점이 있다. 

```java
String value = "String Value";
Object object = Optional.ofNullable(value).orElse(getDefaultValue());
```
위의 상황일때 getDefaultValue()가 null이 아니기 때문에 실행되지 않을거라 생각되지만 
실제로는 실행되는데 이유는 아래와 같이 호출되기 때문에 null여부와 무관하게 getDefaultValue 메소드는 호출 되게 된다.
```java
public T orElse(T getDefaultValue()) {
    return value != null ? value : getDefaultValue();
}
```
orElseGet()의 경우 Supplier로 한 번 감싸여 있기때문에 Supplier.get()으로 호출될때 실행되기 때문에 null인 경우만 발생하게 된다. 


