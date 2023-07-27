# Exception


## 예외 계층 
![](https://velog.velcdn.com/images/hyun6ik/post/25868047-817a-4bc1-871b-cad5d44b515a/image.png)

* `Throwable` : 최상위 예외, 하위에 `Exception`, `Error`가 있다. 
* `Error` : 메모리 부족이나 심각한 시스템 오류와 같이 애플리케이션에서 복구 불가능한 시스템 예외, 애플리케이션 개발자가 이 예외를 잡을 수 없다. 
    * 상위 예외를 catch하면 하위 예외까지 같이 잡는다. 그래서 `Throwable`로 예외를 잡으면 `Error`예외도 함께 잡기 때문에 `Exception`부터 필요한 예외라 생각하고 잡아야 한다. 
* `Exception` : 체크 예외 
    * 애플리케이션 로직에서 사용 가능한 최상위 예외 
    * Exception과 그 하위 예외는 모두 컴파일러가 체크하는 체크예외 
* `RuntimeException`` : 언체크, 런타임 예외 
    * 컴파일러가 체크 하지 않는 언체크 예외 
    * RuntimeException과 자식 예외는 모두 언체크 예외이다.      


__체크예외__
* `Exception`과 그 하위 예외는 모두 컴파일러가 체크하는 체크 예외이다. 
* 체크 예외는 잡아서 처리, 밖으로 던지도록 선언해야한다. 그렇지 않으면 컴파일 에러 발생

```java
// 예시 코드
public void inserMember(Member member) throws Exception {
    memberRepository.insertMember(member);
}

public void inserMember(Member member) {
    try {
        memberRepository.insertMember(member);
    } catch(Exception e) {
        log.info("message = {}", e.getMessage(), e);
    }    
}
```
__체크 예외 장단점__
* 장점 
    * 개발자가 실수로 예외 누락시키지 않도록 컴파일러를 통해 잡아주는 안전장치 
* 단점 
    * 모든 체크 예외를 반드시 잡거나 던지도록 처리해야 하기 때문에, 많이 번거롭고 모든 예외를 다 챙겨야한다. 

<br/>

__언체크 예외__

`RuntimeException`과 그 하위 예외는 언체크 예외로 구분된다. 
컴파일러가 체크하지 않는 예외로 `throws`를 생략할 수 있고 예외가 발생되면 자동으로 예외를 던진다. `throws`를 선언할수 있지만 IDE를 통한 인지하게 하는 정도이다. 

__언체크 예외 장단점__
* 신경쓰고 싶지 않은 언체크 예외를 무시할 수 있다. 예외처리가 강제되지 않기 때문에 유연한 작업이 가능하다.
* 언체크 예외는 실수로 예외를 누락할 가능성이 있다. 컴파일러가 예외처리를 잡아주지 않기 때문에 누락될 가능성이 존재한다. 

__체크예외, 언체크예외는 예외를 처리 불가할때 던지는 부분에서 차이가 있다.__


## 채크 예외를 활용 상황



