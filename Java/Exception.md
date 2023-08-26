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


## 체크 예외 활용 원칙
* 기본적으로는 언체크 예외 사용
* 체크 예외는 비즈니스 로직상 의도적으로 던지는 예외에만 사용 
    * 계좌 이체, 포인트 적립 등 예외가 발생하면 반드시 처리 해야 하는 문제에만 체크 예외를 사용한다. 
* 복구 불가능한 예외를 복구하려 하지말자
    * `SQLException`등 애플리케이션 서버에서 복구 불가능한 예외는 `ControllerAdvice`, `Filter`등을 사용해서 공통으로 처리한다. 
* 꼭 필요한 경우가 아니면 `Exception`자체를 던지지 말자.
    * 최상위 타입인 `Exception`자체를 던지면 다른 체크 예외를 체크할 수 없게 된다. 
    * 체크 예외가 발생해도 컴파일러는 최상위 예외를 던지기 때문에 문법에 맞다고 판단 컴파일 오류가 발생하지 않는다. 

## 언체크 예외 활용 원칙
*   체크 예외를 언체크 예외로 변경 시 기존 예외를 꼭 포함시킨다.
    * 기존 예외를 포함시켜야 스택 트레이스에서 기존 예외도 함께 확인 가능하다.
    * 로그를 출력하고 가능한 공통 예외 처리 영역에서 처리 되도록 한다.


