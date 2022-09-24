# [Lombok] 자주 사용하는 어노테이션과 주의점

## @Getter, @Setter
* __클래스 단위, 필드단위로 생성이가능하다.__
* __필요하면 사용해야 하지만 최대한 Setter사용을 지양한다.__
* __생성자를 사용한 데이터 주입을 우선으로 하고 setter사용 시 의미 있는 이름을 부여하여 사용한다. ex) xxxChange()__
* __무분별한 setter사용은 유지보수를 힘들게한다.__

# 생성자 생성
### @NoArgsConstructor
* __매개변수를 가지지 않는 기본생성자(default constructor)를 생성한다.__
### @RequiredArgsConstructor
* __final 키워드가 붙은 필드에 대한 생성자를 생성한다.__
### @AllArgsConstructor
* __클래스의 모든 필드에 대한 생성자를 생성한다.__

### @AllArgsConstructor, @RequiredArgsConstructor의 주의점!
---
만약 여러 로직에서 생성자가 사용된 상황에서 같은 타입의 dirctor, movieName의 순서가 변경된다면 어떨까

타입이 달랐다면 컴파일 오류가 났겠지만 타입이 같기 때문에 IDE도 모르고 입력값이 서로 바뀌어 들어가는 문제가 발생한다. 

이러한 문제를 해결하는 여러 방법 중 필드명으로 데이터를 설정, 생성하여 변경에 영향이 없는 방식이 빌더 패턴이고

Lombok은 아래의 어노테이션으로 패턴 생성을 자동화해줍니다.

# @Builder
빌더 패턴을 사용하고자 할 때 @Builder를 사용하면 빌더를 대신 생성한다. 

결국 생성자가 필요하기 때문에 기본적으로 @AllArgsConstructor를 내포하여 private 한 생성자를 만들어 외부에서 호출하진 못하지만 내부에서 사용할 수 도 있기 때문에 직접 생성한 생성자 혹은 static 생성 메서드에서 사용하면 위험이 덜하다. 

생성자 별로 멤버 변수를 정의하고 생성자마다 붙이면 해당 생성자들을 사용하는 Builder를 생성하므로 의미 있는 객체를 생성할 수 있습니다.

```java
@Getter
public class Movie {

    private String director;
    private String movieName;
    private int attendance;

    @Builder
    public Movie(String director, String movieName) {
        this.director = director;
        this.movieName = movieName;
        this.attendance = 0;		
    }

    @Builder
    public Movie(String director, String movieName, int attendance) {
        this.director = director;
        this.movieName = movieName;
        this.attendance = attendance;
    }
 }
 ```
# @Data
__@RequiredArgsConstructor + @Getter + @Setter + @ToString + @EqualsAndHashCode를 모두 생성한다.__
* toString의 경우 JPA를 사용한다면 연관관계에 따라 호출 시 무한루프가 돌 수 있기 때문에 잘 판단하여 사용하자! 
* @EqualsAndHashCode는 equals, hashcode 메서드를 생성하는데 해당 메서드의 경우 IDE가 제공하는 방식으로  직접 생성 후 사용이 필드 간 정확한 기능 구현이 가능하다. 

__결론적으로 사용을 자제하는 것이 좋다고 생각된다.__@RequiredArgsConstructor의 같은 경우도

위에서 작성한 문제가 있기 때문에 정말 특별하게 사용해야 하는 경우가 아닐 경우에는 되도록 이면 사용을 자제하자. 



