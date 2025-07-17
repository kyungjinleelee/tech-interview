## SQL 인젝션(SQL Injection)에 대해 설명해 주세요.
**SQL 인젝션**이란, 웹 애플리케이션에서 사용자 입력값을 제대로 검증하지 않고 SQL 쿼리에 포함시킬 때, 공격자가 악의적인 SQL 코드를 삽입하여 데이터베이스를 조작하거나 정보를 탈취할 수 있 공격 기법입니다.

공격자는 이 취약점을 이용해 쿼리를 조작하여 인증을 우회하거나, 데이터를 조작하거나, 테이블 자체를 삭제할 수도 있습니다.

<br>

## 상황 예시
예를 들어, 로그인 검증 시 아래와 같은 코드를 사용한다고 가정해 보겠습니다.
```java
public boolean login(String username, String password) {
    String sql = "SELECT * FROM users WHERE username = '" + username + "' AND password = '" + password + "'";

    try (Connection conn = DriverManager.getConnection("url");
         Statement stmt = conn.createStatement();
         ResultSet rs = stmt.executeQuery(sql)) {
        return rs.next();
    } catch (SQLException e) {
        throw new RuntimeException("Database error", e);
    }
}
```

사용자가 로그인 폼에 아래와 같이 입력한다면,
```plaintext
username: admin' -- 
password: (아무거나)
```

생성되는 쿼리는 다음과 같이 변형됩니다.
```sql
SELECT * FROM users WHERE username = 'admin' -- ' AND password = '1q2w3e4r!';
```
`--` 이후는 주석 처리되므로, 비밀번호 조건은 무시되어 admin 사용자에 대한 정보가 반환됩니다. (admin 계정의 정보가 반환되면 admin 계정으로 로그인도 할 수 있게 될 것) 

이처럼 공격자는 **SQL 쿼리를 조작**하여 **인증을 우회**하거나 **데이터베이스의 정보를 탈취**할 수 있습니다.

<br>

이 외에도 공격자는 다음과 같은 페이로드를 사용할 수 있습니다.
- `' OR '1'='1` : 항상 참이 되는 조건
- `' UNION SELECT * FROM accounts --` : 다른 테이블의 정보 조회
- `'; DROP TABLE users; --` : 테이블 삭제

<br>

## SQL 인젝션을 방지하는 방법은 무엇인가요?
- **PreparedStatement**(프리페어드 쿼리)를 사용하면 place holder(`?`)에 값을 바인딩하고 내부적으로 이스케이프 처리하기 때문에 SQL 인젝션을 방지할 수 있습니다.
  ```sql
  String sql = "SELECT * FROM users WHERE username = ? AND password = ?";
  PreparedStatement pstmt = conn.prepareStatement(sql);
  pstmt.setString(1, username);
  pstmt.setString(2, password);
  ```

- **JPA, Hibernate와 같은 ORM 프레임워크를 사용**하면 SQL 쿼리를 직접 작성하지 않고도 데이터베이스와 상호작용할 수 있습니다.
- 사용자 입력에 대해 공격에 사용되는 SQL 구문의 포함 여부를 검증합니다.
- 웹 애플리케이션에서 사용하는 데이터베이스 계정에 최소한의 권한만 부여합니다.
- SQL 오류나 예외 메시지를 사용자에게 직접 노출하지 않도록 합니다.

> 
> **Prepared Statement**란?
> 
> Prepared Statement는 SQL Injection을 방어하는 최선의 보안 대책이며, SQL구문이 미리 컴파일 되어 있어 입력값을 변수로 선언해 두고 필요에 따라 값을 대입하여 처리하는 방식이다.
>

<br>

## 정리
1. **공격 목적** : 인증 우회, 데이터 유출, 데이터베이스 조작/삭제 등
2. **원인** : 사용자 입력값을 적절히 필터링/이스케이프하지 않음
3. **방지 방법** :
    - Prepared Statement 사용
    - ORM 사용하기
    - 입력값 검증 및 이스케이프
    - 최소 권한 원칙 적용

---
### 꼬리 질문
- [Statement vs Prepared Statement?]()

### 참고 자료
- [[10분 테코톡] 로비의 SQL 인젝션](https://www.youtube.com/watch?v=qzas_-u4Nxk)
- [코딩애플 - SQL injection 공격](https://www.youtube.com/watch?v=FoZ2cucLiDs)
- [한국재정정보원 - 웹 취약점과 해킹 매커니즘 #6 SQL Injection 보안대책](https://www.fis.kr/ko/major_biz/cyber_safety_oper/attack_info/security_news?articleSeq=2588)
