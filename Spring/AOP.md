# [SPRING] SPRING AOP 개념 및 사용법 

## Spring의 AOP종류 
* JDK 동적 프록시
    * 자바언어에서 제공하는 기능 
    * 인터페이스 기반으로 프록시를 생성
    * 인터페이스 기반으로 생성되기 때문에 구현체로 타입캐스팅이 안되는 문제가 있다.
* CGLIB
    * 구체 클래스 기반으로 프록시 생성 
    * 스프링부트 2.0 부터 CGLIB를 기본 사용하도록 변경 
    * final이 붙어있는 클래스의 경우 예외가 발생한다. 
    * final이 붙어있는 메소드는 오버라이딩 할 수 없어 프록시 로직이 동작하지 않는다.



## SPRING AOP의 단어

* 조인 포인트(Join Point)
    * 어드바이스가 적용될 수 있는 위치, 메소드 실행, 생성자 호출, 필드 값 접근, 같은 지점
    * 스프링 AOP는 프록시 방식 이기에 조인 포인트는 항상 메소드 실행 시점으로 제한된다.
* 포인트컷(PointCut)
    * 어디에 부가 기능을 적용할지, 어디에 부가 기능을 적용하지 않을지 판단하는 필터링 
    * 주로 클래스와 메서드 이름으로 필터링 한다, 이름그대로 어떤 포인트(Point)에
       기능을 적용할지 하지 않을지 잘라서(cut)구분한다. 
* 어드바이스(Advice)
    * 프록시가 호출하는 부가 기능으로 단순히 프록시 로직이라 생각하면 된다. 
    * @Around, @Before, @After와 같은 다양한 종류의 어드바이스가 있다.
* 어드바이저(Advisor)
    * 단순하게 하나의 포인트컷과 하나의 어드바이스를 가지고 있는 것 
    * 포인트컷 + 어드바이스이다. 
* 타켓(Target) 
    * 어드바이스를 받는 객체(실제 대상), 포인트컷으로 결정
* 애스펙트(Aspect)
    * 어드바이스 + 포인트컷을 __모듈화__ 한것 
    * @Aspect이고 여러 어드바이스와 포인트 컷이 함께 존재한다.

<br/>


## 어드바이스 종류 
* @Around
    * 메서드 호출 전후에 수행, 가장 강력한 어드바이스
* @Before
    * 조인 포인트 실행 이전에 실행 
* @AfterReturning
    * 조인 포인트가 정상 완료후 실행
* @AfterThrowing
    * 메서드가 예외를 던지는 경우 실행
* @After
    * 조인 포인트가 정상, 예외에 관계없이 실행
* 같은 @Aspect안에서 어드바이스의 실행순서
    * 실행순서 : @Around, @Before, @After, @AfterReturning, @AfterThrowing

<br/>


## 포인트컷 지시자 종류 
* 엑스펙트J는 포인트컷을 편리하게 사용하기 위한 표현식을 제공한다.
* 포인트컷 지시자의 종류 
    * execution : 메소드 실행 조인 포인트를 매칭한다. 스프링 AOP에서 가장 많이 사용하고, 기능도 복잡한다.         
    * within : 특정 "타입" 내의 조인 포인트를 매칭한다. 
        * 표현식에 부모 타입을 지정하면 안된다. 정확한 타입을 지정해야 하고 이것이 execution과 차이가 난다.
    * args : 인자가 주어진 타입의 인스턴스인 조인 포인트
        * execution은 클래스에 선언된 정보를 기반으로 하지만 args는 실제 넘어온 파라미터 객체 인스턴스를 보고 판단.
    * this : 스프링 빈 객체(스프링 AOP 프록시)를 대상으로 하는 조인 포인트 
    * target : Target객체(스프링 AOP 프록시가 가르키는 실제 대상)를 대상으로 하는 조인 포인트 
    * bean : 스프링 전용 포인트컷 지시자, 빈의 이름으로 포인트컷을 지정한다.
    * @target : 실행 객체의 클래스에 주어진 타입의 애노테이션이 있는 조인 포인트 
    * @within : 주어진 애노테이션이 있는 타입 내 조인 포인트
    * @annotation : <b>메서드</b>가 주어진 애노테이션을 가지고 있는 조인 포인트를 매칭 
    * @args : 전달된 실제 인수의 런타임 타입이 주어진 타입의 애노테이션이 있는 조인 포인트

<br/>

## 지시자 파라미터 매칭 규칙 
* execution
    * (String) : 정확하게 String 타입 파라미터 
    * () : 파라미터가 없어야 한다. 
    * (*) : 정확히 하나의 파라미터, 단 모든 타입을 허용한다.
    * (*, *) : 정확히 두 개의 파라미터, 단 모든 타입을 허용한다. 
    * (..) : 숫자와 무관하게 모든 파라미터, 모든 타입을 허용한다. 참고로 파라미터가 없어도 된다. 0..*로 이해하면 된다. 
    * (String, ..) : String 타입으로 시작해야 대상이 된다. 숫자와 무관하게 모든 파라미터, 모든 타입을 허용
* within 
    * "within(패키지.타켓클래스)
* args
    * "args(String)", "args(..)", "args(String, ..)"
    * execution과 달리 부모 타입의 매칭도 허용한다. 
* @target, @within
    * @target : 실행 객체의 <b>클래스</b>에 주어진 타입의 애노테이션이 있는 조인 포인트
    * @within : 주어진 애노테이션이 있는 타입 내 조인 포인트
    * <b>@target와  @within의 차이</b>
        * @target은 부모 클래스의 메소드에 대해서도 적용된다.
        * @within은 부모 클래스의 메소드에 대해선 적용되지 않는다.
* @annotation, @args
    * @annotation : 메서드가 주어진 애노테이션을 가지고 있는 조인 포인트를 매칭
        * ex) @Around("@annotaion(패키지.어노테이션)")
    * @args : 전달된 실제 인수의 런타입 타입이 주어진 타입의 애노테이션을 갖는 조인 포인트
* bean
    * Around("bean(orderService) || bean(*Repository*)")  
    * 스프링 전용 포인트컷 지시자, 빈의 "이름"으로 지정한다.  
* this, target
    * this : 스프링 빈 객체(스프링 AOP 프록시)를 대상으로 하는 조인 포인트
    * target : 실제 target객체를 대상으로 포인트컷을 매칭    


## 지시자 사용 시 유의사항 
args, @args, @target은 단독사용을 지양해야 한다.
해당 지시자는 실제 객체 인스턴스가 생성되고 실행될 때 어드바이스 적용 여부를 확인 할 수 있다. 

프록시가 없다면 판단이 불가능한데 위의 지시자가 있으면 모든 스프링 빈에 AOP를 적용하려 한다. 스프링 빈에 final로 지정된 빈도 있기 때문에 오류가 발생할 수 있다. 

</br>


## 스프링의 빈 후처리기 
__AnnotationAwareAspectJAutoProxyCreator__ 라는 빈후처리기는 스프링 빈으로 등록된 어드바이저들을 자동으로 찾아서 프록시가 필요한 곳에 자동으로 프록시를 적용한다. 

어드바이저안에는 포인트컷과 어드바이스가 포함되어 있기때문에 어떤 스프링 빈에 프록시를 적용해야 하는지 알 수 있다. 

* 스프링 프록시 생성기 작동 과정 
    * 스프링 빈 대상이 되는 객체를 생성
    * 생성된 객체를 빈 저장소에 등록하기 직전 빈 후처리기에 전달
    * 스프링 컨테이너에서 Advisor 빈 모두 조회
    * @Aspect 어드바이저 빌더 내부에 저장된 Advisor를 모두 조회 
    * Advisor에 포함된 포인트컷을 사용해서 해당 객체가 프록시 적용 대상인지 판단
        * 해당 객체의 모든 메서드를 매칭하고 하나라도 만족하면 프록시 대상이된다. 
    * 프록시를 생성하고 반환하여 프록시를 스프링 빈으로 등록한다.

<br/>

## 다수의 어드바이저가 등록될 경우 
만약 "CardService"라는 클래스가 다수의 어드바이저의 대상이 된다면 CardService의 프록시는 대상만큼 생성될까? 그렇지 않다. 프록시는 하나만 생성하고 여러 어드바이스를 등록한다.

즉 다수의 어드바이저의 대상이 되도 스프링은 target마다 __하나의 프록시__ 만 생성하고 
여러 어드바이스를 등록한다. 

<br/>



## 프록시의 내부 호출 문제 
예시 코드 
```java 
@Component
public class Payment {
    /**
     * external() {
     *  log.info("call external");
     *  internal();
     * }
     **/
    @Autowired
    PaymentService paymentService; // AOP 적용대상 

    public void serviceCall1() {
        paymentService.external();
    }    
}
```
만약 PaymentService의 external(), internal() 둘다 AOP의 대상일때 external()
처럼 내부에서 __같은 클래스__ 의 메서드를 호출할때 AOP가 정상적으로 적용되지 않는다.

<h2>원인</h2>

external()가 실행될때 스프링 컨테이너에 있는 프록시가 실제 paymentService(target)을 호출하는데 실제 target에서 실행되는 external() 내부에서 실행되는 internal()은 AOP의 대상이라 해도 프록시가 아닌 실제 target클래스에서 실행되기 때문에 AOP의 대상에서 제외된다. 

<h2>해결방법</h2>
다양한 방법이 있지만 대표적으로 내부 호출되는 메소드의 기능을 다른 클래스로 분리하여 호출하는 것이 가장좋다. 다른 클래스로 분리하고 해당 기능을 내부에서 호출한다.

```java
@RequiredArgsConstructor
@Service
public class paymentService {

    private final InternalService internalService;
    
    public void external() {
        log.info("call external");
        internalService.internal();
    }
}
```

혹은 가능하다면 두 기능을 분리한 후 클라이언트에서 external(), internal()를 각 각 호출하는 방법도 가능하다.





