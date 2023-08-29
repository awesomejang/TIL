# [JPA] 영속성 컨텍스트

JPA에서 사용되는 논리적인 개념으로 엔티티매니저를 통해 접근하여 사용한다. 
스프링 프레임워크에서는 엔티티 매니저와 영속성 컨텍스트가 N:1 관계로 사용된다. 

![](https://www.baeldung.com/wp-content/uploads/2019/11/transition-persistence-context.png)

## 엔티티의 생명주기
* 비영속(new/transient)
    * 영속성 컨텍스트와 __관계 없는 새로운 상태__
* 영속(managed)
    * 영속성 컨텍스트에 __관리되는 상태__
* 준영속(detached)
    * 영속성 컨텍스트에 저장되었다가 __분리__된 상태
* 삭제(removed)
    * __삭제__ 된상태

### 비영속

객체를 생성은 했으나 영속성 컨텍스트에서는 관리 하지 않는 상태
```java
Person person = new Person();
person.setName("KIM");
```

### 영속
객체를 생성하고 영속성 컨텍스트에서 관리하는 상태
```java
Person person = new Person();
person.setName("KIM");
// factory = EntitiyManagerFactory
EntityManager em = factory.createEntityManager();
em.getTransaction().begin(); // 트랜잭션 시작

em.persist(person) // 객체 영속
```

### 준영속
```java
em.detach(person); // person 엔티티를 영속성 컨텍스트 에서 분리, -> 더이상 관리 하지 않는다. 
```

### 삭제
```java
em.remove(person) // 객체 자체를 삭제
```

## 영속성 컨텍스트 사용 이점 
* 1차 캐시 활용
    * 엔티티의 id를 키로 엔티티 자체를 value로 영속성 컨텍스트에 저장한다. 
    * 엔티티에 대한 조회 요청이 왔을때 먼저 1차캐시에서 조회 하고 없으면 DB조회 후 캐싱한다. 
    * __같은 트랜잭선__ 안에서 같은 id의 엔티티간의 동일성(== 비교)를 보장한다.
* 쓰기 지연 
    * persist() 시점에 insert 쿼리가 나가지 않고 커밋(트랜잭션 종료)시점에 쿼리가 나간다. 
    * 영속성 컨텍스트 안에 쓰기 지연 SQL 저장소라는 논리적 공간에 쿼리를 보관 후 트랜잭션 종료 시점에 flush한다.
* 변경 감지 
    * 트랜잭션 종료 시점전에 엔티티의 데이터에 변경사항이 생길 경우 스냅샷(원본)과 비교 하여 변경사항을 발견하고 자동으로 update 쿼리를 생성하여 실행한다. 

## 영속성 컨텍스트 플러시 시점 
영속성 컨텍스트의 사항들을 트랜잭션 종료 시점 전에 DB에 전송하려면 flush 처리를 해야하는데 직접 플러시를 하지 않고 자동으로 flush 처리되는 시점이 존재.
* 트랜잭션 커밋 시점
* JPQL 쿼리 실행 시점 