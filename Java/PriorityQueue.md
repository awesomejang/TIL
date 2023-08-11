# [JAVA] Priority Queue

`PriorityQueue`란 일반적인 Queue의 구조 FIFO를 가지면서 데이터의 우선순위를 설정하고 우선순위가 높은 데이터가 먼저 나가는 자료구조이다. Queue와 동일하게 이진트리 구조로 이루어져 있으며 데이터의 우선순위를 사용하는 상황에 용의하다. 

`PriorityQueue`의 기본 우선순위는 '자연순서', '오름차순'이며 `Comparable Interface`를 구현하여 우선순위를 설정할 수 있다.  

## 기본 우선순위 
```java
import java.util.PriorityQueue;
public class QueueTest {
    public static void main(String[] args) {
        //== Comparable Interface 구현 X ==//
        PriorityQueue<Integer> queue = new PriorityQueue<>();        
        // 데이터 입력
        queue.addAll(Arrays.asList(3,1,2,5,4));
        //== queue 출력 결과 ==//        
        // 1, 2, 3, 4, 5 -> 오름차순 
    }
}
```

## Comparable 우선순위 설정
```java
import java.util.PriorityQueue;
public class QueueTest {
    public static void main(String[] args) {
        //== Comparable Interface 구현 X ==//
        PriorityQueue<Integer> t = new PriorityQueue<>((Integer o1, Integer o2) -> {
            // 내림차순으로 우선순위 설정
            return o2 - o1;
        });
        // 데이터 입력
        queue.addAll(Arrays.asList(3,1,2,5,4));
        //== queue 출력 결과 ==//        
        // 5, 4, 3, 2, 1 -> 내림차순 
    }
}
```





