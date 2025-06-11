## Keep Alive에 대해 설명해 주세요.
**Keep Alive**는 네트워크 통신에서 **커넥션을 지속해서 유지**하기 위해 사용되는 기술이나 메커니즘을 의미합니다.

즉, 클라이언트와 서버(혹은 두 시스템) 간의 연결이 끊기지 않았는지 확인하거나, 연결이 장시간 유지되도록 일정 주기로 신호(패킷)를 전송하는 방식입니다.
만약 상대방이 오랜 시간 응답하지 않으면 연결이 끊긴 것으로 간주하고, 자원을 해제하거나 재접속을 시도합니다. 

주로 TCP/IP, HTTP, WebSocket, DB 연결, 시스템 하트비트 등 다양한 분야에서 사용됩니다.

<br>

## 주요 사용 예시
1. **HTTP Keep-Alive**
   
   **HTTP 프로토콜**에서 Keep-Alive는 **하나의 TCP 연결로 여러 개의 HTTP 요청과 응답**을 주고받을 수 있도록 하는 기능입니다. HTTP/1.0에서는 요청마다 새로운 커넥션을 열고 닫았지만(즉 기본적으로 요청-응답 이후 연결을 끊었지만), HTTP/1.1부터는 Keep-Alive가 기본적으로 활성화되어 있어 커넥션을 재사용할 수 있습니다.

2. **TCP Keep Alive**

   **TCP 프로토콜**에서 Keep-Alive는 **커넥션이 유휴 상태일 때 커넥션이 끊어지지 않도록 주기적으로 패킷을 전송**하는 기능입니다. 장시간 데이터가 오가지 않아도 연결을 유지하기 위해 **OS 수준에서 주기적으로 작은 패킷을 전송**합니다. 상대방이 응답하지 않으면 연결이 끊어진 것으로 판단합니다.

3. **WebSocket Keep Alive / 하트비트**

    실시간 연결이 중요한 WebSocket 등에서는 Ping/Pong 패킷이나, 자체 정의 하트비트 메시지를 주고받아 연결을 유지합니다.

4. **DB Connection Keep Alive**

   DB Connection Pool 등에서는 연결이 유휴 상태로 오래 있으면 끊기는 문제를 막기 위해 주기적으로 쿼리를 보내 살아있는지 확인합니다.

<br>

## Keep Alive의 장점과 단점은 무엇이 있을까요?
**장점**

- 커넥션을 재사용하여 네트워크 비용을 절감할 수 있습니다. (속도 향상, 리소스 절약)
- handshake에 필요한 RTT(Round Trip Time)가 감소하여 네트워크 지연 시간(Latency)을 줄일 수 있습니다.
- 연결 상태를 신속하게 감지 가능합니다.

**단점**
- 유휴 상태일 때에도 커넥션을 점유하고 있기 때문에 서버의 소켓이 부족해질 수 있습니다.
- DoS 공격으로 다수의 연결을 길게 유지하여 서버를 과부하시킬 수 있습니다.
- 타임아웃 설정이 적절하지 않으면 커넥션 리소스가 낭비될 수 있습니다.

<br>

## 정리

| 구분        | 동작 방식 및 목적                                                    |
|-----------|------------------------------------------------------------------|
| HTTP      | 하나의 연결로 여러 요청/응답 처리(keep-alive 헤더 사용)                  |
| TCP       | 일정 주기마다 OS가 패킷을 보내 연결 상태를 확인                         |
| WebSocket | Ping/Pong, 하트비트 메시지 등으로 연결이 살아있는지 감시                  |
| DB        | 주기적으로 쿼리 등을 보내 연결이 끊기지 않도록 유지                     |

Keep Alive는 **연결 유지를 위한 주기적 신호 전송** 메커니즘이며, 각 기술/프로토콜 별로 구현 방식이 다르나 목적은 동일합니다.

---
### 꼬리 질문
- [HTTP와 TCP의 Keep Alive는 어떤 차이가 있나요?](#)

### 참고 자료
- [MDN - HTTP Keep-Alive](https://developer.mozilla.org/ko/docs/Web/HTTP/Reference/Headers/Keep-Alive)
- [위키백과 - TCP Keepalive](https://en.wikipedia.org/wiki/Keepalive)
- [[HTTP] keep alive란? (persistent connection에 대하여)](https://etloveguitar.tistory.com/137)
- [[Network] HTTP Keep-Alive VS TCP Keep-Alive 제대로 알기](https://sabarada.tistory.com/262)
- [HTTP Keep-Alive, 성능 최적화의 핵심일까? 쓸데없는 설정일까?](https://bin-repository.tistory.com/185)
