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
