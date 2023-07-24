# Jackson 주요 어노테이션 정리

## @JsonCreator
 * 생성자, 정적 팩토리를 통해 역직렬화를 할때 사용한다. 
    * 만약 불변한 상태를 유지하려면 setter를 제거하고 @JsonCreator를 사용한다.
 * @JsonProperty을 사용하여 파라미터와 필드명을 맞춰준다.
 ``` java
public class ResponseDto {
    private String name;
    private String age;
    private String email;    

    @JsonCreator
    public ResponseDto(@JsonProperty("name") String name, @JsonProperty("age") String age, @JsonProperty("email") String email) {        
        this.name = name;
        this.age = age;
        this.email = email;
    }        
}

// ==== Enum 사용 예시 ===//
public enum Color {
    RED("FF0000"),
    GREEN("00FF00"),
    BLUE("0000FF");

    private String value;

    Color(String value) {
        this.value = value;
    }

    @JsonCreator
    public static Color fromString(String value) {
        for (Color color : Color.values()) {
            if (color.value.equalsIgnoreCase(value)) {
                return color;
            }
        }
        throw new IllegalArgumentException("Invalid color value: " + value);
    }

    @JsonValue
    public String getValue() {
        return value;
    }
}
 ```
<br/>

 ## @JsonInclude
 * Jackson의 직렬화 대상은 객체의 모든 필드 이지만 어노테이션을 사용하여 대상을 지정한다.
    * Include 열거형 타입으로 전략을 결정
```java
@Getter
@JsonInclude(JsonInclude.Include.NON_NULL) //NULL이 아닌 필드만 직렬화 대상으로 지정
public class ResponseDto {
    private String name;
    private String age;
    private String email;
    private target tt;
}
```
* Include.NON_NULL : NULL값이 아닌 필드만 대상으로 지정 
* Include.NON_EMPTY : NULL값이 아니며, Collection, 배열등의 값이 비어있지 않은 필드만 
                     대상으로 지정 
* Include.NON_DEFAULT : 필드의 값이 기본값(숫자 : 0, boolean : false)과 다른 경우에만 대상으로 지정                      
* Include.ALWAYS : 모든 필드를 대상으로 포함합니다.(default)
* Include.CUSTOM : 직렬화 필드를 선택하는 커스텀 필터를 지정합니다. 
    * FilteringJacksonValue 클래스를 상속받아 커스텀 필터를 생성할 수 있습니다.

<br/>

## @JsonProperty
* 직렬화, 역직렬화 시 JSON key와 자바 객체의 필드를 매핑할때 사용한다. 
```java
@Data
public class ResponseDto {
    // 역직렬화 : JSON key "username"를 필드 name에 매핑한다.
    // 직렬화 : name필드는 JSON key "username"으로 직렬화 된다.
    @JsonProperty("username") 
    private String name;
    private String age;
    private String email;
    private target tt;
}
```
* Getter, Setter 메소드에 붙여서 직렬화, 역직렬화 매핑을 원할때 지정가능하다.

<br/>

## @JsonIgnore 
* 필드레벨에서 사용하며 JSON 직렬화/역직렬화 대상에서 제외시킨다.
```java
public class ResponseDto {    
    private String name;
    @JsonIgnore // age는 직렬화/역직렬화 대상에서 제외된다.
    private String age;
    private String email;
    private target tt;
}
```

## @JsonIgnoreProperties
* 클래스 레벨에서 사용하며 직렬화/역직렬화 제외 대상 필드명을 파라미터로 전달하여 제외시킨다.
```java
@JsonIgnoreProperties({"name", "age"}) // name, age필드를 대상에서 제외시킨다.
public class ResponseDto {    
    private String name;    
    private String age;
    private String email;
    private target tt;
}
```
* allowSetters
    * true로 지정하면 역직렬화는 수행된다.
* allowGetters
    * true로 지정하면 직렬화는 수행된다.

<br/>

## @JsonValue
JSON 직렬화 시 사용되며, 객체를 JSON형식으로 변환할 때 해당 객체의 어떤 값을 기준으로 JSON 데이터를 생성할지 결정한다. 

@JsonValue 어노테이션을 사용하면 특정 객체의 값을 JSON 문자열로 직접 지정할 수 있다. 

일반적으로 Jackson은 객체의 모든 필드를 직렬화하는데 @JsonValue를 사용하여 특정 메소드를 지젖하여 해당 메서드가 반환하는 값을 JSON으로 직렬화합니다. 

일반적인 Dto보다는 Enum타입에서 사용성이 있을거라 생각된다. 
```java
public class Person {
    private String name;
    private int age;

    public Person(String name, int age) {
        this.name = name;
        this.age = age;
    }

    @JsonValue
    public String toJson() {
        return name + " is " + age + " years old";
    }
}
// Person 직렬화 : "John is 30 years old"

public enum Status {
    ACTIVE("active"),
    INACTIVE("inactive"),
    PENDING("pending");

    private String value;

    private Status(String value) {
        this.value = value;
    }

    @JsonValue
    public String getValue() {
        return value;
    }
}
// Status(Active) 직렬화 시 : "active"
```

