## DB Replication(데이터베이스 복제)의 장/단점에 대해 설명해주세요.
**DB Replication**은 하나의 데이터베이스(Primary/Leader) 데이터를 다른 데이터베이스(Replica/Follower)로 복제하는 기술입니다. 
이를 통해 여러 개의 DB 인스턴스를 운영할 수 있으며, 부하 분산과 데이터 가용성을 향상시킬 수 있습니다.

DB Replication은 데이터 가용성, 성능, 안정성을 향상시키는 중요한 기술이지만, 운영 복잡성 및 데이터 동기화 문제 등의 단점도 존재합니다.

## 장점
### 1. 성능 향상 (읽기 부하 분산)
- 읽기(SELECT) 요청을 Replica(복제본)에서 처리하여 Primary DB의 부담을 줄일 수 있음
- 트래픽이 많은 서비스에서 DB 성능을 최적화하는 데 유리함
- 예시: 이커머스 상품 조회 API → Primary가 아닌 Replica에서 조회

### 2. 고가용성 (High Availability)
- Primary DB 장애 발생 시, Replica를 승격(Promotion)하여 서비스 지속 가능
- Zero Downtime 유지보수 가능 → Primary 유지보수 중에도 Replica가 읽기 요청을 처리
- 예시: AWS RDS Multi-AZ 설정 시, 장애 발생 시 자동 Failover

### 3. 데이터 백업 & 장애 복구
- Master의 내용을 복제하기 때문에 데이터 베이스를 지워먹는다고 하더라도 Slave 중 하나를 Master로 활용하면 되기 때문에 데이터를 백업하는 용도로도 사용 가능
- 실시간 데이터 복제를 통해 데이터 유실을 최소화
- 백업 데이터가 있어 재해(Disaster) 복구에 용이
- 예시: 금융 서비스에서 데이터 손실 방지를 위해 Multi-Region Replication 적용

### 4. 글로벌 서비스 지원 (지연 시간 감소)
- 지리적으로 분산된 Replica를 배포하여 사용자의 위치에 따라 가까운 DB에서 읽기 요청 처리
- 글로벌 서비스의 네트워크 레이턴시(latency)를 줄이는 효과
- 예시: Netflix는 미국, 유럽, 아시아 등에 Replica를 배치하여 빠른 응답 속도 제공


## 단점
### 1. 데이터 정합성을 보장할 수 없음
- Slave는 Master의 복사본을 사용하기 때문에 그것이 정말 완벽하다고 할 수 없음
- 예를 들어 Slave가 Master의 쿼리 처리량을 따라가지 못한다면 데이터 정합성이 보장되지 않음
- 예시: 쇼핑몰에서 결제 완료 후에도 상품 재고 수량이 실시간으로 반영되지 않는 문제 발생 가능

### 2. Binary Log File 관리
- Master는 Binary Log가 무분별하게 쌓이는 것을 막기 위해 데이터 보관 주기를 설정함
- 하지만 Master는 Slave까지 관리 하지 않기 때문에 Master에서 Binary Log File을 삭제했다고 Slave에서 Binary Log를 삭제하지 못함

### 3. 복잡한 관리 & 운영 비용 증가
- 기본적으로 master에서 Error가 발생했을 경우 Slave로 Failover 하는 기능을 지원하지 않음
- 따라서 DB 장애 시 Replica 승격(Failover) 및 Load Balancing 관리 필요
- 여러 개의 DB 인스턴스를 운영해야 하므로 관리 부담 증가

## 4. 쓰기 성능 저하 (쓰기 부하 분산 어려움)
- 쓰기(INSERT, UPDATE, DELETE) 요청은 Primary에서만 처리해야 하므로 부하 집중
- 쓰기 요청을 분산하려면 추가적인 샤딩(Sharding) 전략 필요
- 예시: 고트래픽 서비스(예: SNS, 블로그)에서는 Replication 만으로는 쓰기 부하 해결이 어려움

## 5. 데이터 유실 가능성
- 비동기 복제 사용 중 Primary가 장애 발생하면, 일부 데이터가 Replica로 복제되지 않을 위험
- 동기 복제를 사용하면 해결 가능하지만, 쓰기 성능이 저하됨
- 예시: 트랜잭션이 Primary에만 커밋되고 Replica로 반영되지 않은 상태에서 장애 발생 시 데이터 유실 가능

## 정리
DB는 기본적으로 Read 작업에서 자원 소모가 많기 때문에 Read 작업이 많아 성능 이슈가 발생하는 경우 가장 먼저 고려해볼 전략입니다. Replication을 적용하면 성능 향상을 체감 할 수 있지만, 단점 또한 치명적이기 때문에 만능이라고 할 수는 없습니다.

- **읽기 부하**가 많은 서비스 → Replication 적극 활용
- **쓰기 부하**가 많은 서비스 → 샤딩(Sharding)과 함께 고려
- 장애 발생 시 데이터 정합성이 중요한 서비스 → 동기 복제 사용 고려

Use Case에 따라 적절한 Replication 전략을 선택하는 것이 중요합니다!

### 연관 질문
- [DB Replication이란 무엇이고, 왜 쓰나요?](https://github.com/kyungjinleelee/tech-interview/blob/main/cs/database/DB%20Replication%EC%9D%B4%EB%9E%80%20%EB%AC%B4%EC%97%87%EC%9D%B4%EA%B3%A0%2C%20%EC%99%9C%20%EC%93%B0%EB%82%98%EC%9A%94%3F.md)

---

### 참고 자료
- [[DB] Replication이란 무엇이고 장점과 한계점 정리](https://velog.io/@zpswl45/DB-Replication-%EA%B0%9C%EB%85%90-%EC%A0%95%EB%A6%AC)
- [[Database] 리플리케이션(Replication) vs 클러스터링(Clustering)](https://mangkyu.tistory.com/97)
