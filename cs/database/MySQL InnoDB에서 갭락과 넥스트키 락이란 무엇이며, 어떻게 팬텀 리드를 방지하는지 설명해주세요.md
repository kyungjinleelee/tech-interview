## MySQL InnoDB에서 갭락과 넥스트키 락이란 무엇이며, 어떻게 팬텀 리드를 방지하나요?

**갭락(Gap Lock)**과 **넥스트키 락(Next-Key Lock)**은 MySQL InnoDB 스토리지 엔진의 REPEATABLE READ 격리 수준에서 발생하는 잠금 메커니즘입니다.
이들은 팬텀 리드(Phantom Read) 현상을 방지하기 위해 사용됩니다.

## 갭락(Gap Lock)이란?
- 갭 락은 **특정 인덱스 값 사이의 공간(Gap)을 잠그는** 락입니다. 
- 기존 레코드 간의 간격을 보호하여 새로운 레코드의 삽입을 방지합니다. 갭 락은 범위 내에 특정 레코드가 존재하지 않을 때 적용됩니다. 
- 트랜잭션이 특정 범위 내에서 데이터의 삽입을 막아 팬텀 읽기(Phantom Read) 현상을 방지할 수 있게 됩니다.
- 예를 들어, 인덱스 값 10과 20 사이의 갭을 잠그면 이 범위 내에 새로운 레코드 15를 추가할 수 없습니다.

```sql
-- id 1, 3, 5가 저장된 orders 테이블

-- 트랜잭션 A 시작
START TRANSACTION;

-- 트랜잭션 A 1-3과 3-5 사이의 갭과 3 레코드 락 설정(넥스트키 락)
SELECT * FROM orders WHERE orders_id BETWEEN 2 AND 4 FOR UPDATE;

-- 트랜잭션 B 시작
START TRANSACTION;

-- 트랜잭션 B가 id 4에 데이터 삽입 시도 시, 갭락으로 인해 삽입이 차단되어 대기
INSERT INTO orders (orders_id, orders_amount) VALUES (4, 200);
...
```

## 넥스트키 락(Next-Key Lock)이란?
- 넥스트키 락은 레코드 락과 갭락을 결합한 형태로, 특정 인덱스 레코드와 그 주변의 갭을 동시에 잠그는 락입니다. (즉, 레코드 자체뿐만 아니라 해당 레코드 앞의 갭까지 함께 잠금)
- 이를 통해 레코드 자체의 변경과 함께 그 주변 공간의 변경도 동시에 제어할 수 있습니다.
- 넥스트키 락은 특정 레코드와 그 주변 공간을 잠그기 때문에, 다른 트랜잭션이 새로운 레코드를 삽입하여 팬텀 리드를 발생시키는 것을 방지합니다.

| orders_id | orders_amount |
|-----------|--------------|
| 1         | 100          |
| 2         | 200          |
| 3         | 300          |

```sql
-- 트랜잭션 A 시작
START TRANSACTION;

-- 트랜잭션 A amount = 200인 orders_id = 2 레코드에 대한 레코드 락과 1-2, 2-3에 대한 갭락을 동시에 잠금으로써 넥스트키 락을 설정
SELECT * FROM orders WHERE orders_amount = 200 FOR UPDATE;

-- 트랜잭션 B 시작
START TRANSACTION;

-- 트랜잭션 B orders_id = 4, orders_amount = 200인 레코드 삽입 시도 시, 넥스트키 락으로 인해 차단되어 대기
INSERT INTO orders (orders_id, order_amount) VALUES (4, 200);
...
```

## 갭락과 넥스트키 락을 통한 팬텀 리드 방지 메커니즘
**트랜잭션 A**가 특정 범위의 데이터를 조회할 때, 해당 범위에 대해 갭락 또는 넥스트키 락을 설정합니다. 락이 설정된 범위 내에서는 **트랜잭션 B**가 새로운 레코드를 삽입하거나 기존 레코드를 수정하는 것이 차단됩니다. 따라서, 트랜잭션 A가 다시 동일한 조건으로 조회를 수행하더라도, 트랜잭션 B에 의해 새로운 데이터가 삽입되지 않아 팬텀 리드가 발생하지 않습니다.

### 참고 자료
[[MySQL] InnoDB 의 잠금 방식: 레코드락, 갭락, 넥스트키락](https://pushedrumex.tistory.com/14)
[MySQL - InnoDB Locking](https://dev.mysql.com/doc/refman/8.0/en/innodb-locking.html)
[MySQL - Transaction Isolation Levels](https://dev.mysql.com/doc/refman/8.0/en/innodb-transaction-isolation-levels.html)
[[MySQL] 스토리지 엔진 수준의 락의 종류(레코드 락, 갭 락, 넥스트 키 락, 자동 증가 락)](https://mangkyu.tistory.com/298)

