## 팬텀 리드(Phantom Read)란?

팬텀 리드(Phantom Read)는 같은 트랜잭션 내에서 동일한 조건으로 데이터를 조회했을 때, **처음에는 없던 새로운 데이터가 두 번째 조회에서 나타나는 현상**을 의미합니다.

이는 주로 읽기 일관성(Read Consistency) 을 유지하는 과정에서 발생할 수 있는 문제로, 데이터의 삽입(INSERT)이나 삭제(DELETE)가 다른 트랜잭션에 의해 이루어질 때 발생합니다.

---

### 팬텀 리드 발생 예제 시나리오
```sql
-- 트랜잭션 A 시작
START TRANSACTION;

-- 트랜잭션 A 첫 번째 조회
SELECT * FROM orders WHERE amount > 150;

-- 트랜잭션 B 시작
START TRANSACTION;

-- 트랜잭션 B 새로운 행 삽입
INSERT INTO orders (customer_id, amount) VALUES (4, 250);

-- 트랜잭션 B 커밋
COMMIT;

-- 동일한 조건으로 트랜잭션 A 두 번째 조회시, 트랜잭션 A의 첫 번째 조회에는 존재하지 않던, 트랜잭션 B에서 삽입된 새로운 행이 함께 조회된다.
-- 단, MVCC를 지원하는 경우 해당 케이스에서 팬텀 리드가 발생하지 않는다.
SELECT * FROM orders WHERE amount > 150;
```

----

### 팬텀 리드 vs 더티 리드 vs 반복 가능 읽기

| 문제 유형 | 발생 원인 | 예제 |
|----------|----------|------|
| **더티 리드 (Dirty Read)** | 커밋되지 않은 데이터 읽기 | `UPDATE`된 미완료 데이터를 읽음 |
| **반복 가능 읽기 (Repeatable Read)** | 같은 데이터를 조회할 때 값이 달라짐 | `UPDATE`에 의해 값이 변경됨 |
| **팬텀 리드 (Phantom Read)** | 새로운 데이터가 삽입되거나 삭제됨 | `INSERT`, `DELETE`로 인해 조회 결과가 달라짐 |

---

### 팬텀 리드 해결 방법

### 1. REPEATABLE READ (기본 격리 수준)
- InnoDB에서는 넥스트키 락(Next-Key Lock)을 사용하여 팬텀 리드를 방지합니다.
- 레코드뿐만 아니라 레코드 앞의 갭(Gap)까지 잠그기 때문에 새로운 데이터 삽입을 차단할 수 있습니다.

#### 예제
```sql
-- 트랜잭션 A가 특정 조건의 데이터를 조회하며 잠금
SELECT * FROM employees WHERE salary >= 5000 FOR UPDATE;
```
이렇게 하면 다른 트랜잭션에서 salary >= 5000인 데이터를 삽입할 수 없음!

### 2. SERIALIZABLE (가장 강력한 격리 수준)
- 트랜잭션을 직렬화하여 완벽하게 팬텀 리드를 방지
- SELECT 쿼리를 실행할 때마다 **암묵적으로 공유 잠금(Shared Lock)**이 걸림
- 하지만 성능이 저하될 가능성이 높음

#### 적용 예제
```sql
SET TRANSACTION ISOLATION LEVEL SERIALIZABLE;
START TRANSACTION;
SELECT * FROM employees WHERE salary >= 5000;
```
이제 트랜잭션 A가 종료되기 전까지 다른 트랜잭션에서 INSERT 불가능!

---

### 결론
- 팬텀 리드는 트랜잭션 중 다른 트랜잭션이 데이터를 삽입 또는 삭제하여 발생하는 문제
- InnoDB에서는 기본적으로 REPEATABLE READ와 넥스트키 락을 사용하여 방지 가능
- 보다 강력한 보호를 위해 SERIALIZABLE 격리 수준을 사용할 수 있음 (하지만 성능 저하 가능)

MySQL InnoDB의 기본 설정(REPEATABLE READ + 넥스트키 락)으로도 대부분의 팬텀 리드를 해결할 수 있음!

---
#### 참고 자료
[[DB] Mysql은 어떻게 Phantom Read를 예방할까?](https://velog.io/@bbbbooo/DB-Mysql%EC%9D%80-%EC%96%B4%EB%96%BB%EA%B2%8C-Phantom-Read%EB%A5%BC-%EC%98%88%EB%B0%A9%ED%95%A0%EA%B9%8C)
[[MySQL] 트랜잭션 격리 수준(Isolation Level)에 대한 이해](https://mozzi-devlog.tistory.com/49#:~:text=%F0%9F%91%A9%E2%80%8D%F0%9F%92%BB%20%ED%8C%AC%ED%85%80%20%EB%A6%AC%EB%93%9C,%EC%9D%BD%EA%B8%B0%EC%97%90%EC%84%9C%EB%8F%84%20%EB%B0%9C%EC%83%9D%ED%95%A0%20%EC%88%98%20%EC%9E%88%EB%8B%A4.)
