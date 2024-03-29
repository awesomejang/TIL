# 엔티티 매핑 

## @Entity
JPA사용해서 테이블과 매핑할 클래스에는 @Entity 애노테이션을 붙인다. 

#### 기본 생성자
JPA 정책 상 기본생성자를 필수로 생성해야한다. 
public, proteced 생성자 필수 / DB에 저장할 필드에 final 사용하지 않아야 한다. 

__속성__
* JPA에 사용할 엔티티 이름을 지정 
* 기본은 클래스 이름으로 사용된다. 
* 가급적 기본 사용 권장
```java
@Entity(name = "")
public Class Board {

}
```

## @Table
엔티티와 매핑할 테이블을 지정한다. 
|속성|기능|기본값|
|---|---|---|
|name|매핑할 테이블명 지정|엔티티 이름을 그대로 사용|
|uniqueConstraints|DDL 생성 시에 유니크 제약 조건 생성|

## 데이터베이스 스키마 자동 생성
DDL이 AP 실행 시점에 자동으로 실행된다. 
__절대 개발DB 붙었을때만 사용__
스키마 자동 생성에 대한 옵션을 아래와 같다. 
* propertis name = hibernate.hbm2ddl.auto

|옵션|기능|
|---|---|
|crate|기존 테이블 삭제 후 재생성(DROP + CREATE)|
|create-drop|create와 같지만 AP 종료 시점에 테이블 DROP|
|update|변경분만 반영|
|validate|엔티티와 테이블이 정상 매핑된지 여부만 확인|
|none|사용 X|

__운영 DB 환경에선 create, create-drop, update 절대 사용 XXX(생각만 해도 재앙)__

## 컬럼 매핑 
* @Column : 컬럼 매핑 
    * @name : 필드와 매핑할 컬럼명 지정 (기본은 필드명으로)
    * insertable, updatable : 등록가능여부 설정 / 수정 가능여부 설정
    * nullable(DDL) : null 값의 허용 여부 설정 
    * unique(DDL) : @Table의 유니크 제약 조건 설정 
    * precision, scale(DDL) : BigDecimal 타입에서 사용, 전체 자릿수, 소수점 자릿수를 설정 double, float에는 적용 X
* @Temporal : 날짜 타입 매핑 
    * Date, Calender을 매핑할때 사용
    * LocalDate, LocalDateTime 사용 시에는 생략 가능
    * TemporalType.DATE : 날짜, DB date 타입과 매핑 (2021-01-11)
    * TemporalType.TIME : 시간, 데이터베이스 time타입 매핑 (11:23:12)
    * TemporalType.TIMESTAMP : 날짜, 시간, 데이터베이스 timestamp타입과 매핑
* @Enuerated : enum 타입의 필드 컬럼 매핑 
    * EnumType.ORDINAL : enum 필드의 순서를 DB에 저장(1,2...) 권장 X, __기본값__
    * EnumType.STRING : enum 이름을 데이터베이스에 저장 (권장)
* @Lob : BLOB, CLOB 매핑 
    * 매핑 필드가 문자면 CLOB, 나머지는 BLOB
* @Transient : 필드를 컬럼에 매핑 하지 않음
    * 매핑을 하지 않는다. 
    * DB에 저장, 조회도 하지않음 

## 기본키(PK) 매핑
* @Id
* @GenertatedValue
```java
@Id
@GeneratedValue
private Long id;
```

* 직접 할당만 한다면? -> @Id만 사용
* 자동 생성 사용 -> @GeneratedValue 사용
    * IDENTITY : DB에 위임 -> AutoIncrement ex) MySQL
    * SEQUENCE : 데이터베이스 시퀸스 사용 ex) ORACLE
        * @SequenceGenerator 설정 필요
    * TABLE : 키 생성용 테이블 사용, DB에 테이블 생성하여 사용하기에 모든 DB에서 사용 가능  
        * @TableGerator 필요
    * AUTO : DB에 따라 자동 지정(__기본값__)

### IDENTITY
* 주로 MySQL, PostgreSQL ...에서 사용 
* 트랜잭션 커밋 시점에 INSERT 쿼리가 실행된다. -> 커밋전에는 데이터를 알 수 없다. 
    * 그래서 em.persist()시점에 즉시 INSERT INTO 쿼리를 실행하여 DB에서 Id값 조회
```java
@Id
@GeratedValue(strategy = GenerationType.INDENTITY)
private Long id;
```

### SEQUENCE
시퀸스로 PK값을 순서대로 생성하는 DB에서 사용 
ex) 오라클, H2....

```java
@Entity
@SequenceGenerator(name ="BOARD_SEQ_SEQUENCE"
                , sequenceName = "BOARD_SEQ" // DB에 생성된 시퀸스 이름 
                , initalValue = 1, allocationSize = 1) // 초기값과 얼만큼 증가할지
public Class Board {
    @Id
    @GeneratedValue(strategy = GenertationType.SEQUENCE
                  , generator = "BOARD_SEQ_SEQUENCE") // 상단에 생성한 generator 와 매핑 
    private Long id;
}
```

## 객체간 연관관계
테이블은 FK로 조인을 사용하여 연관관계를 만들지만 객체는 참조를 통해 연관 객체와 연결한다. 

* 단방향 연관관계를 지향하되, 필요에 따라 양방향으로 설정할 수 도 있다. 
* 양방향 연관관계 설정 시 연관관계의 주인을 설정하고, 주인 객체를 통해 데이터를 조작한다. 
* mappedBy로 단순 양방향 연관임을 설정하고, 해당 필드는 데이터의 변경, 수정등에서 제외된다. 

```java
@Entity
public Class Player {

    @Id @GeneratedValue
    private Long id;

    private String name;

    @ManyToOne
    @JoinColumn(name = "TEAM_ID") // 연관관계 연결 할 FK 설정_연관관계의 주인
    private Team team;
}

@Entity
public Class Team {

    @Id @GeneratedValue
    private Long id;

    private String name;

    @OneToMany(mappedBy = "team") // Player의 team 필드에 연관 설정 
    private List<Player> players = new ArrayList<Player>();
}
```

__양방향 연관관계__
*   양방향으로 설정 시 서로 다른 단방향 관계 2개라고 생각해야한다. 
    * 연관관계의 실질적인 양방향은 없다. 
* 양방향으로 설정 시 데이터를 두군데 모두에 입력해두는게 좋다. 
    * 1차 캐시에 데이터가 있을 경우 1차 캐시만 바라보기 때문 
* 주인이 아닌 쪽의 양방향 연관관계는 단순히 조회기능이 추가 되었다고 생각해야한다.
* 가능한 단방향으로 문제 해결 권장
* 실수를 방지하기 위해 편의 메소드 사용을 권장 
```java
public void chageTeam(Team team) {
    // 두 연관관계 모두에 데이터 입력 
    this.team = team;
    team.getPlayers().add(this); 
}
```
__연관관계 주인__
* 두 객체의 관계중 한개를 주인으로 설정
    * 외래키가 있는 객체를 주인으로 설정 -> __외래 키를 관리(등록, 수정)__
* 주인이 아닌쪽은 읽기만 가능, 수정이나 삭제 등 데이터 변경 안된다. 
