# __[JAVA] Stream API 주요 기능__


## __Stream 이란?__
<p>
JAVA8에서 도입된 기능으로, 컬렉션의 요소들을 처리하는 기능을 제공한다. 

기존에는 for-each 루프 혹은 iterator를 이용하여 컬렉션의 요소를 하나씩 처리했지만, Stream을 이용하여 더 간결하고 가독성이 좋은 코드를 작성 할 수 있다. 

Stream은 컬렉션의 요소들의 연속된 처리를 지원하며, 각 처리 단계에서 요소들을 필터링, 매핑, 정렬 등 다양한 작업을 수행할 수 있다. 

Stream을 이용하면 코드의 가독성과 유지보수성이 향상되며, 불필요한 반복문을 줄일 수 있어 성능 개선에 도움이되나 기본적으로 속도는 for-each문 보다 오래 소요된다.
</p>

---
<br/>

## filter()
Predicate 함수형 인터페이스에 따라 요소를 선택하며, 이 함수는 boolean값을 리턴합니다.
filter()를 호출하면 해당 스트림의 각 요소가 Predicate 함수에 전달되며 함수가
 'true'를 반환하는 경우에만 해당 요소가 새로운 스트림으로 유지된다. 

```java
Stream<T> filter(Predicate<? super T> predicate);
``` 

```java
// name필드에 "B"문자가 포함되는 요소만 선택된다. 
Stream<Ad> list = adList.stream().filter(ad -> ad.getName().contains("B"));
```

