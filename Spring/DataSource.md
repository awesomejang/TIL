# DataSource

다양한 커넥션 풀 오픈소스가 존재하는데 만약 현재 사용중인 커넥션 풀에서 다른 커넥션 풀로 변경해야 한다면 커넥션을 획득하는 애플리케이션 코드도 함께 변경되어야하고 사용법도 다를것이다.

자바에서는 이런 문제를 해결하기 위해 javax.sql.DataSource라는 인터페이스를 제공한다. 
DataSource는 커넥션을 획득하는 방법을 __추상화__ 하는 인터페이스이다. 

대부분의 커넥션 풀은 `dataSource` 인터페이스를 이미 구현해놨다. 각각의 커넥션 풀에 의존하는게 아닌 `dataSource` 인터페이스에만 의존하도록 애플리케이션 로직을 작성하면 된다. 

__간단한 HikariDataSource 생성 예제__
```java
HikariDataSource dataSource = new HikariDataSource();
dataSource.setJdbcUrl(URL); // JDBC URL
dataSource.setUsername(USERNAME); // DB username
dataSource.setPassword(PASSWORD); // DB password
dataSource.setMaximumPoolSize(10); // custom Pool Size
dataSource.setPoolName("CustomPoolName");
```

<h3>커넥션 생성 주요사항</h3>

커넥션 풀에 커넥션을 채울때 별도의 쓰레드를 통하여 커넥션을 채운다. 
별도의 쓰레드를 통해 커넥션을 채우는 이유는 커넥션을 채우는 것은 상대적으로 오래 걸리는 일이기 때문에 같은 쓰레드로 채운다면 애플리케이션 실행 시간이 늦어진다. 애플리케이션 실행시간에 영향을 주지 않기 위해 별도의 쓰레드를 사용해서 커넥션 풀을 채운다.