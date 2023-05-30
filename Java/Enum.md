# [JAVA] Enum타입의 활용

## Enum타입이란? 

자바에서 열거형을 나타내는 특별한 유형의 클래스이다. 
상수들을 정수나 문자열과 같은 기본 타입으로 나타내어 사용했지만, 이러한 방식은 의미를 알기 어렵고, 오타가 발생하거나 잘못된 값을 사용할 수 있는 문제가 있었고 Enum은 이러한 문제를 해결하기 위해 설계되었다. 

Enum을 정의하려면 'enum'키워드를 사용한다. 각 상수는 enum내에 ','로 구분되어 선언된다. 
상수들은 대문자로 작성하는 것이 관례이고, 각 상수는 Enum클래스의 인스턴스이다. 

불규칙한 값을 상숫값으로 설정하고 싶으면 상수의 이름 옆에 괄호()를 추가하고, 그 안에 상숫값을 명시할 수 있다. 하지만 이때에는 불규칙한 특정 값을 저장할 수 있는 인스턴스 변수와 생성자를 별도로 추가해야한다. 

```java
public enum Day {
    MONDAY("monday"),
    TUESDAY("tuesday"),
    WEDNESDAY("wednesday"),
    THURSDAY("thursday"),
    FRIDAY("friday"),
    SATURDAY("saturday"),
    SUNDAY("sunday");
    
    private final String dayString;
    
    Day(String dayString) {
        this.dayString = dayString;
    }
}
```

## Enum의 주요 메소드 

* values() 

values() 메소드는 해당 열거체의 모든 상수를 저장한 배열을 생성하여 반환합니다. 
```java
@Test
void test() {
    Day[] values = Day.values();
    for (Day day : values) {
        System.out.println(day);
    }
}
//===결과====//
MONDAY
TUESDAY
WEDNESDAY
THURSDAY
FRIDAY
SATURDAY
SUNDAY
```
* valueOf()

valueOf 메소드는 전달된 문자열과 일치하는 해당 열거체의 상수를 반환한다. 
```java
CallLogAspect.Day day = CallLogAspect.Day.valueOf("MONDAY");
System.out.println(monday == CallLogAspect.Day.THURSDAY); // "MONDAY"
System.out.println(monday == CallLogAspect.Day.TUESDAY); 
// == 출력 ==//
// true
// false
```

* name()

해당 열거체 상수의 이름을 반환한다.






