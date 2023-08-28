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




