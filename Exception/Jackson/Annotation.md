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
@JsonInclude(JsonInclude.Include.NON_NULL) //NULL이 아닌 필드만 역직렬화 대상으로 지정
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



