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


