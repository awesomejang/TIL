# 스프링의 트랜잭션 관리 

## 트랜잭션 추상화
JDBC, JPA등 데이터 접근 기술이 변경되면, 서비스 계층의 트랜잭션 코드도 함께 수정되야한다. 기술마다 트랜잭션 사용 방법이 다르기 때문이다. 

스프링은 위와 같은 문제를 해결하기 위해 트랜잭션 기술을 추상화 하여 제공하고 데이터 접근 기술에 따라 구현체도 대부분 이미 만들어 두어서 가져다 사용하기만 하면된다. 

스프링 트랜잭션의 추상화 인터페이스는 `PlatformTransactionManager`이다. 

__트랜잭션 구현체__
* DataSourceTransactionManager
* JpaTransactionManager
* HibernateTransactionManager


## 리소스 동기화 

트랜잭션을 유지하려면 트랜잭션의 시작과 끝까지 같은 DB 커넥션(세션)을 유지해야한다.
스프링은 __트랜잭션 동기화 매니저__ 를 제공한다. 쓰레드로컬을 사용해서 커넥션을 동기화 하고 트랜잭션 매니저는 내부에서 이 트랜잭션 동기화 매니저를 사용한다. 

쓰레드로컬을 사용하기 때문에 멀티쓰레드 환경에서 안전하게 커넥션을 동기화할 수 있고 커넥션이 필요하다면 트랜잭션 동기화 매니저를 통해 커넥션을 획득하면 된다. 

__트랜잭션 동기화 매니저 동작 방식__
* 트랜잭션을 시작하기 위해서는 DB 커넥션이 필요하기 때문에 dataSource를 통해 
  커넥션을 생성하고 트랜잭션을 시작
* 트랜잭션 매니저는 커넥션을 __트랜잭션 동기화 매니저__ 에 보관한다. 
* 데이터 접근 로직은 트랜잭션 동기화 매니저에 보관된 커넥션을 꺼내서 사용한다. 
* 트랜잭션이 종료(commit, rollback)되면 트랜잭션 매니저는 트랜잭션 동기화 매니저에 보관된 커넥션을 통해 트랜잭션 종료 후 커넥션을 닫는다. 

스프링 부트는 적절한 트랜잭션 매니저를 자동으로 스프링 빈에 등록한다. 
 * 등록된 빈 이름 : transactionManager
트랜잭션 매니저를 선언적으로 등록하면 스프링 부트는 트랜잭션 매니저를 자동으로 등록하지 않는다. 

## 스프링의 트랜잭션 
서비스의 비즈니스 로직과 트랜잭션 처리 로직을 분리하여 사용할 수 있도록 스프링은 AOP기반의 트랜잭션 처리 기능을 제공한다. 스프링 부트를 사용하면 트랜잭션 AOP를 처리하기 위한 스프링 빈들도 자동으로 등록해준다. 

트랜잭션 처리가 필요한 곳에 `@Transactional` 애노테이션만 붙여주면 스프링이 애노테이션을 인식해서 트랜잭션 프록시를 적용해준다. 

`@Transactional` 애노테이션은 메서드에 붙여도 되고, 클래스에 붙여도된다. 클래스에 붙이면 클래스 내부의 public 메서드가 대상이 된다. 

```java
/**
 * @Transactional 예시 코드 
 */
@RequiredArgsConstructor
public class MemberService {

    private final MemerRepository memberRepository;

    @Transactional
    public void updateMember(long memberId, UpdateDto updateDto){
        memberRepository.updateMemberInfo(memberId, updateDto);
        memberRepository.updateMemberStatus(memberId, updateDto);
    }
}
```


