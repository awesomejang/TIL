# [AssertJ] 예외 발생 테스트 방법

## assertThatThrownBy
```java
@Test
public void textException() {
    assertThatThrownBy(() -> {
        // 예외발생 테스트 로직 
        throw new Exception("Exception Occur!");
    }).isInstanceOf(Exception.class)// 예외 클래스 확인
      .hasMessageContaining("Exception Occur!") // 예외 메세지 확인....
}
```
__예외클래스 종류, 예외 메세지 뿐만 아니라 다양한 테스트 케이스 존재__

## assertThatExceptionOfType
```java
@Test
public void textException() {
    assertThatExceptionOfType(IOException.class).isThrownBy(() -> { // 예외발생 테스트 로직 
    throw new IOException("boom!"); }).withMessage("%s!", "boom") // format 형식의 메시지 확인
                 .withMessageContaining("boom") // 예외 메세지 포함여부 확인
                 .withNoCause();

}
```
* 두 방식은 테스트 케이스의 종류만 다를뿐 동일하게 동작한다.