## Connection Timeout, Socket Timeout, Read Timeout의 차이점은 무엇인가요?
**네트워크 타임아웃(Timeout)** 은 서버로 요청을 보냈지만 일정 시간 동안 답변을 받지 못하면 발생합니다.
요청을 보내고 일정 시간을 기다렸다가 응답이 없으면 최초 요청을 실패 처리하고 다시 요청을 보내는 방식으로 작동합니다.

타임아웃 유형에는 대표적으로 `Connection Timeout`과 `Socket Timeout`, `Read Timeout`이 있습니다.

## 1. Connection Timeout
**Connection Timeout**은 클라이언트가 서버에 연결을 시도할 때, 일정 시간 내에 연결이 이루어지지 않으면 발생하는 타임아웃입니다. 

TCP 소켓 통신에서 클라이언트와 서버가 연결될 때, 정확한 전송을 보장하기 위해 사전에 세션을 수립하는데, 이 과정을 3-way-handshake라고 합니다. 
Connection Timeout은 이 3-way-handshake가 일정 시간 내에 완료되지 않을 때 발생합니다. 즉, 서버의 장애나 응답 지연으로 인해 연결을 맺지 못하면 Connection Timeout이 발생합니다.

## 2. Socket Timeout
**Socket Timeout**은 Connection Timeout 이후에 발생할 수 있는 타임아웃입니다. 

클라이언트와 서버가 연결된 후, 서버는 데이터를 클라이언트에게 전송합니다. 
이때 하나의 데이터 덩어리가 아니라 여러 개의 패킷 단위로 쪼개서 전송되는데, **각 패킷이 전송될 때의 시간 차이 제한**을 Socket Timeout이라고 합니다. 만약 서버가 일정 시간 내에 다음 패킷을 보내지 않으면, 클라이언트는 Socket Timeout을 발생시키고 연결을 종료할 수 있습니다.

## 3. Read Timeout
**Read Timeout**은 클라이언트와 서버가 연결된 후, 특정 I/O 작업이 일정 시간 내에 완료되지 않으면 발생하는 타임아웃입니다. 

클라이언트와 서버가 연결된 상태에서, 서버의 응답이 지연되거나 I/O 작업이 길어져 요청이 처리되지 않을 때 클라이언트는 연결을 끊습니다. 
Read Timeout은 이러한 상황을 방지하기 위해 설정된 타임아웃으로, 일정 시간 내에 데이터가 읽혀지지 않으면 클라이언트가 연결을 종료합니다.

## 네트워크 통신에 타임아웃이 필요한 이유
타임아웃이 필요한 이유는 **자원을 절약**하기 위함입니다. 

가령, 외부 서비스로 요청을 보냈지만 해당 요청이 무한정 길어질 수 있습니다. 이때 서비스의 요청이 자원을 가지고 있으면, 서비스의 자원이 고갈되어 장애가 발생할 수 있습니다. 타임아웃을 설정하면 이렇게 요청이 무한정 길어지는 상황을 예방할 수 있습니다.

### 참고 자료
- [토스페이먼츠 - 타임아웃](https://docs.tosspayments.com/resources/glossary/timeout)
- [성공/실패를 단언할 수 없는 네트워크 에러 안전하게 다루기](https://hudi.blog/safely-handling-network-errors/)
