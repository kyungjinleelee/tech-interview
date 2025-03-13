## DB Replication이란 무엇이고, 왜 쓰나요?
DB Replication이란 한 데이터베이스에서 다른 데이터베이스로 데이터가 **동기화**되는 것으로, **DB를 다중화**하는 것을 말합니다. 두 개의 이상의 DBMS 시스템을 **Mater / Slave**로 나눠서 동일한 데이터를 저장하는 방식입니다.

특히, 대규모 애플리케이션 환경에서는 데이터의 지속적인 가용성과 신뢰성이 매우 중요하기 때문에, 원본(Source) 서버와 복제(Replica) 서버 간의 데이터 동기화는 필수입니다.
![DBReplication](https://github.com/user-attachments/assets/f88ff23b-01ff-49fe-9e0b-295a1d5603f8)

<br>

트래픽이 많은 상황에서도 **부하 분산**을 통한 **고가용성**을 확보할 수 있습니다. 예를 들면, 쓰기 요청은 원본인 source 데이터베이스로, 읽기 요청은 복제본인 replica 데이터베이스로 보내서 부하를 분산시킬 수 있습니다.

## 어떻게 Replication 방식은 과부하 문제를 해결할 수 있는 건가요?
![image](https://github.com/user-attachments/assets/061165b7-0451-43e8-8f39-1d5bd16ba507)

Replication은 Master DB에는 **Insert, Update, Delete** 작업을 수행하도록 하고 **Select** 작업을 Slave DB에서 하도록 구성합니다.

왜 Select 작업만 따로 뺄까에 대한 의문이 생길 수 있습니다. 그 이유는 보통 select 작업이 시간이 많이 걸리기 때문입니다. 
(e.g. 데이터가 5만개가 있을때, Table Full Scan을 해야 한다면 매우 많은 시간이 소요될 것이고, 이 시간동안 다른 작업을 하지 못하게 되니 병목 현상의 주요 원인이 될 수 있습니다.)


## 어떤 상황에서 DB Replication을 쓸 수 있을까요?
트래픽이 몰리지 않더라도 다양한 상황에서 DB Replication을 통해 고가용성을 달성할 수 있습니다.
1. 트래픽 과다가 예상되서, 부하를 분산해야 될 상황
2. 데이터베이스가 자꾸 죽을경우, 복제를 이용해 예비용 서버를 만들 수도 있음
3. 분석이나 데이터 백업 같은 무거운 작업을 처리하는 전용 서버를 구축할 수도 있음


### 참고 자료
- [[DB] Replication이란 무엇이고 장점과 한계점 정리](https://velog.io/@zpswl45/DB-Replication-%EA%B0%9C%EB%85%90-%EC%A0%95%EB%A6%AC)
- [Database Replication](https://nesoy.github.io/blog/Database-Replication)
- [[10분 테코톡] 리비의 DB Replication](https://www.youtube.com/watch?v=7DwxuWyCNHA)
