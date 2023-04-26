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

<br/>

## map()
스트림의 각 요소들을 지정된 함수에 따라 변환하여 __새로운 스트림을 반환합니다.__ 
기존 스트림의 각 요소를 인자로 받는 함수를 적용하여 새로운 값을 반환하고, 이를 모아 새로운 스트림을 만드는 역할을 합니다. 

map()은 Function 인터페이스를 구현한 람다 표현식을 인자로 받고 람다 표현식은 현재 요소를 받아 변환된 값을 반환합니다.
```java
<R> Stream<R> map(Function<? super T, ? extends R> mapper);
```
```java
List<String> strings = Arrays.asList("map", "filter", "grouping");
// 문자열의 길이가 요소가 되는 List로 변환
List<Integer> collect = strings.stream().map(item -> item.length())
                                        .collect(Collectors.toList());
```

<br/>

## sorted()
스트림의 요소를 정렬하여 새로운 스트림을 반환합니다. 
```java
Stream<T> sorted();
Stream<T> sorted(Comparator<? super T> comparator);
```
Comparable 인터페이스를 구현한 객체의 경우 기본적으로 compareTo 메소드를 
사용하여 정렬합니다. 
Comparable을 구현하지 않았을 경우, Comparator를 인자로 전달하여 정렬 기준을 지정 가능하다. 
처리 후 새로운 스트림을 리턴한다. 
```java
List<Ad> adList = new ArrayList<>();
adList.add(new Ad(1, "1A"));
adList.add(new Ad(2, "2A"));
adList.add(new Ad(3, "3A"));
adList.add(new Ad(4, "1B"));
adList.add(new Ad(5, "2B"));

// ComponentSeq를 기준으로 역순으로 정렬 한다.
List<Ad> collect = adList.stream()
                         .sorted(Comparator.comparingLong(Ad::getComponentSeq)
                         .reversed())
                         .collect(Collectors.toList());
```

## groupingBy()
그룹핑은 스트림의 요소들을 원하는 기준에 따라 그룹핑하고, 그룹별로 처리할 수 있는 방법을 제공한다. java8부터는 Collectors 클래스에서 다양한 그룹핑 연산을 제공하며, groupingBy 메서드를 사용하여 스트림의 요소들을 그룹화할 수 있다. 

이를 통해 스트림에서 요소들을 그룹별로 처리하거나, 그룹별로 결과를 수집할 수 있다. 
```java
public static <T, K> Collector<T, ?, Map<K, List<T>>> groupingBy(Function<? super T, ? extends K> classifier)

public static <T, K, A, D>
    Collector<T, ?, Map<K, D>> groupingBy(Function<? super T, ? extends K> classifier,
                                          Collector<? super T, A, D> downstream)
```

```java
//== 초기 데이터 ==//
List<Ad> adList = new ArrayList<>();
adList.add(new Ad(1, "1A"));
adList.add(new Ad(1, "2A"));
adList.add(new Ad(3, "3A"));
adList.add(new Ad(4, "1B"));
adList.add(new Ad(5, "2B"));

//===============//

// Ad의 첫번째 파라미터를 기준으로 groupingBy
Map<Long, List<Ad>> collect1 = adList.stream().collect(Collectors.groupingBy(ad -> ad.getComponentSeq()));

//== 두번째 파라미터로 Collectors을 주면 Map의 각 그룹에 대한 결과를 수집하는데 사용
Map<Long, Long> collect1 = adList.stream().collect(Collectors.groupingBy(ad -> ad.getComponentSeq(), Collectors.counting()));
```

<br/>

## reduce()
스트림의 모든 요소를 결합하여 단일 값으로 축소하는 기능을 제공한다. 
reduce() 메서드는 스트림의 최종 연산 중 하나이며, 스트림의 요소를 하나의 값으로 합치는 작업을 수행한다. 
```java
// Stream의 요소의 타입이 최종연산의 결과타입이 된다. 
T reduce(T identity, BinaryOperator<T> accumulator);

// 최종 연산 결과를 커스텀 할수 있다.
<U> U reduce(U identity, BiFunction<U, ? super T, U> accumulator, BinaryOperator<U> combiner);
```
```java
List<Integer> integers = Arrays.asList(1, 2, 3, 4, 5);
// reduce = 15
Integer reduce = integers.stream().reduce(0, Integer::sum);
```

<br/>

## flatMap()
flatMap 메소드는 Stream의 각 요소에 대해 하나 이상의 새로운 요소를 생성하고, 이러한 새로운 요소를 담은 하나의 새로운 Stream을 반환하는 중간 연산입니다.

flatMap은 Stream을 인자로 받고 매핑된 Stream을 하나의 Stream으로 통합합니다. flatMap은 다차원의 컬렉션에 대한 Stream을 생성하여 각 요소를 일차원화 할 때 사용하기 용이합니다. 

```java
<R> Stream<R> flatMap(Function<? super T, ? extends Stream<? extends R>> mapper);
```
```java
List<String> strings = Arrays.asList("hello", "world");

// map함수의 반환값은 하나의 요소로 변환
// {["h","e","l","l","o"], ["w","o","r","l","d"]}
List<String[]> collect2 = strings.stream()
                                 .map(item -> item.split(""))
                                 .collect(Collectors.toList());

// 각 요소를 Stream으로 변환 후 이러한 Stream들을 하나의 새로운 Stream으로 평면화
// ["h","e","l","l","o", "w","o","r","l","d"]
List<String> collect3 = strings.stream()
                               .flatMap(item -> Arrays.stream(item.split("")))
                               .collect(Collectors.toList());
```

## collect()
Stream API의 collect() 메서드는 Stream의 요소를 컬렉션으로 변환하는 메서드입니다. collect() 메서드를 사용하면 Stream의 요소를 List, Set, Map 등의 컬렉션으로 변환할 수 있습니다.

collect() 메서드는 Collector객체를 인수로 받고, Stream의 요소를 
어떤 형식의 컬렉션으로 수집할지 지정하는 인터페이스이다. 

```java
<R, A> R collect(Collector<? super T, A, R> collector);
```
```java
// 결과를 List로 변환한다.
List<String> collect = strings.stream().flatMap(item -> Arrays.stream(item.split(""))).collect(Collectors.toList());
```
