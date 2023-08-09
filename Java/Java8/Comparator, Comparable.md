# [JAVA] Comparator 와 Comparable

## Comparator / comparable 
Java8에서 정렬 순서를 제시할때 유용하게 사용 가능한 함수형 인터페이스인 Comparator,comparable를 제시했습니다. 해당 인터페이스는 각각 `compare(T o1, T o2)`, `compareTo(T o)`를 통해 정렬기준을 설정하여 사용합니다. 

## Comparator
`Comparator`는 `compare(T o1, T o2)`메소드 기준으로 정렬을 수행하며 파라미터의 비교 대상으로 2개의 객체를 차례대로 받고 메소드 리턴 결과에 따라 정렬 순서가 정해집니다. 
*   양수 : 첫번째 파라미터 정렬순위 뒤로(오름차순)
*   0 : 변경 없음
*   음수 : 첫번째 파라미터 정렬순위 앞으로(내림차순)

```java
List<Integer> integers = Arrays.asList(30, 20, 10, 60, 70, 80);

integers.sort(new Comparator<Integer>() {
    @Override
    public int compare(Integer o1, Integer o2) {
    // o1이 큰수면 음수    
    return o2 - o1;
    }
});
//== 출력결과 ==//
// 80 70 60 30 20 10
```

## Comparable
`Comparable`는 `compareTo(T o)`를 통해 정렬 순서를 정한다. 정렬기준은 `Comparator`와 동일하고 현재객체(this)를 `Comparator`의 첫번째 파라미터와 동일하게 정렬하면 된다. 

```java
// Person(String name, int weigh, int point)
List<Person> peoples = Arrays.asList(new Person("KIN", 80, 10)
                                   , new Person("LEE", 90, 15)
                                   , new Person("PARK", 80, 15)
                                   , new Person("JANG", 100, 50));
// ===================================================================//
public class Person implements Comparable<Person> {
    @Override
    public int compareTo(Person o) {        
        // weight 기준으로 내림차순 정렬
        return this.getWeight() - o.getWeight();
    }
}
// ===================================================================//
// 정렬 실행
Collections.sort(peoples); 

//== 출력 결과 ==//
// Person{name='KIN', weight=80, point=10}
// Person{name='PARK', weight=80, point=15}
// Person{name='LEE', weight=90, point=15}
// Person{name='JANG', weight=100, point=50}
```

