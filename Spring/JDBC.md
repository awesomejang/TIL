# JDBC

## __JDBC 탄생 배경__

> 애플리케이션 개발할때 중요한 데이터는 데이터베이스에 저장한다. 수 많은 데이터베이스가 있는데 각각의 데이터베이스마다 커넥션을 연결하는 방법, SQL을 전달하는 방법, 결과를 응답 받는 방법이 전부 다르다. 만약 데이터베이스를 변경해야 된다고 하면 변경할 데이터베이스에 맞추어 모든 코드를 변경해야 하는 문제가 발생한다. 
    
> 이런한 문제를 해결하기 위해 "JDBC"라는 자바 표준이 등장한다. 

<br/>

## JDBC 표준 인터페이스 
> JDBC(Java Database Connectivity)는 자바에서 데이터베이스에 접속할 수 있도록 하는 자바 API다. JDBC는 데이터베이스에서 자료를 쿼리하거나 업데이트 하는 방법을 제공한다. 


* JDBC 인터페이스 구성 요소 
    * Connection - 연결 
    * Statement - SQL 전달
    * ResultSet - SQL 요청 응답 

JDBC 인터페이스를 각각의 DB에 맞도록 구현한 라이브러리를 DB벤더에서 제공하는데 이것을 JDBC 드라이버라 한다. 
EX) Oracle JDBC, MySQL JDBC...

<br/>

## JDBC를 직접 사용? 
JDBC는 오래되고 사용법도 복잡하기 때문에 JDBC를 편리하게 사용하게 하는 다양한 기술(SQL Mapper, ORM..)을 사용한다. 

<br/>

## JDBC를 사용한 DB연결 
```java
//== DriverManager를 통한 DB 연결 ==//
/**
 * java.sql.DriverManager
 **/
public Connection getConnection() {
    // URL -> JDBC URL
    // USERNAME -> DB 접속계정
    // PASSSWORD -> 계정 비밀번호
    try {
        Connection con = DriverManager.getConnection(URL, USERNAME, PASSWORD);
    } catch(SQLException e) {
            
    }
    
}
```
라이브러리 중 데이터베이스 드라이버(JDBC)를 찾아 해당 드라이버의 커넥션을 반환한다.





