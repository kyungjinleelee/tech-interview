## 교착 상태(deadlock)에 대해서 설명해주세요.
교착 상태란 **멀티스레드 시스템**에서 **두 개 이상의 프로세스 혹은 스레드**가 **서로 상대방이 점유한 자원을 기다리며 무한히 대기**하는 상태를 말합니다.

교착상태가 발생하면 시스템이 멈추고, 해결하기 어려운 상태가 됩니다.
![image](https://github.com/user-attachments/assets/15d9d88c-8a97-4249-ac46-e1f9da8588fe)

## 교착 상태가 발생하는 조건이 뭔가요?
4가지 조건(상호 배제, 점유 대기, 비선점, 원형 대기)이 모두 만족하는 경우, 교착 상태에 빠질 수 있습니다.

- **상호 배제(mutual exclusion)** 는 한 프로세스가 사용하는 자원을 다른 프로세스가 사용할 수 없는 경우를 의미합니다.
- **점유 대기(hold and wait)** 는 자원을 할당받은 상태에서 다른 자원을 할당받기를 기다리는 상태를 의미합니다.
- **비선점(non-preemption)** 은 자원이 강제적으로 해제될 수 없으며 점유하고 있는 프로세스의 작업이 끝난 이후에만 해제되는 것을 의미합니다.
- **순환 대기(= 원형 대기, circular wait)** 은 프로세스 간에 자원 요구가 순환 형태를 이루는 것을 의미합니다.

---

## 그렇다면 어떻게 해결할 수 있을까요? (OS에서의 교착 상태 해결 방법)
![image](https://github.com/user-attachments/assets/49b85282-4e78-409e-9d06-6d8eb0e06b13)
- **예방**: 위 4가지 조건 중 하나 이상을 미리 막음
- **회피**: 은행원 알고리즘 등으로 안전 상태 유지
- **탐지 및 회복**: 주기적으로 교착 상태 탐지 후 해결
- **무시**: "교착 상태는 드물다"는 가정 하에 무시 (ex. 대부분의 OS), 데드락은 매우 드물게 발생하기 때문에 이를 처리하는 비용이 더 커서, 데드락이 발생하면 사용자가 작업을 종료하는 식으로 동작하게 합니다. (ex. ‘응답없음’ 알림)

## 자바에서 교착 상태는 어떻게 해결할 수 있나요?
자바에서 교착 상태(Deadlock)는 주로 `synchronized`, `Lock` 등의 동기화 메커니즘을 잘못 사용할 때 발생합니다.

#### 문제상황 예시
```java
// thread 1
synchronized (resource1) { 
  synchronized(resource2) { ... }
}

// thread 2
synchronized (resource2) { 
  synchronized(resource1) { ... }
}
```
자바의 syncronized 키워드를 잘못 사용하여 이로 인한 교착 상태가 발생하고 있습니다.

### 1. 락 획득 순서를 일관되게 유지하기 (Lock Ordering)
자원에 접근할 때 **항상 정해진 순서대로 락을 획득**하면 순환 대기를 피할 수 있습니다.
```java
// 자원 A -> B 순서 고정
synchronized (lockA) {
    synchronized (lockB) {
        // 작업 수행
    }
}
```

### 2. 타임아웃을 사용하는 Lock (tryLock 사용)
ReentrantLock의 **tryLock(timeout) 메서드**를 활용하면, 일정 시간 안에 락을 못 얻으면 대기를 포기하게 할 수 있습니다.
```java
import java.util.concurrent.locks.Lock;
import java.util.concurrent.locks.ReentrantLock;
import java.util.concurrent.TimeUnit;

Lock lockA = new ReentrantLock();
Lock lockB = new ReentrantLock();

try {
    if (lockA.tryLock(100, TimeUnit.MILLISECONDS)) {
        try {
            if (lockB.tryLock(100, TimeUnit.MILLISECONDS)) {
                try {
                    // 작업 수행
                } finally {
                    lockB.unlock();
                }
            }
        } finally {
            lockA.unlock();
        }
    }
} catch (InterruptedException e) {
    e.printStackTrace();
}
```

### 3. 락을 하나로 합치기 (Lock Coarsening)
여러 락 대신 단일 락으로 묶어서 처리하면 deadlock을 피할 수 있지만, 병렬성이 떨어질 수 있으므로 주의해야 합니다.
```java
synchronized (globalLock) {
    // 여러 자원을 한꺼번에 처리
}
```

### 4. 불필요한 락 제거하여 데드락 발생 확률 낮추기
synchronized 블록을 꼭 필요한 영역으로 최소화하고, 공유 자원을 줄이면 교착 상태 발생 확률을 낮출 수 있습니다.

---

## Java에서의 데드락 해결 방법 요약
| 해결 전략             | 설명 |
|----------------------|------|
| **락 순서 고정**       | 항상 동일한 순서로 락 획득 |
| **tryLock 사용**       | 락 획득 타임아웃 설정으로 대기 회피 |
| **단일 락 사용**        | 단일 자원으로 묶어 충돌 방지 |
| **락 최소화**          | 동기화 구역을 최소화하여 충돌 지점 제거 |

정리하자면 교착 상태가 발생하는 4가지 조건 중 하나를 충족하지 못하게 하거나, 대기하는 경우 무한정 기다리지 않는 방식으로 교착 상태를 풀어볼 수 있습니다.

---

### 참고 자료
- [[OS] 데드락(Deadlock)은 언제 발생하고, OS, Java에서 어떻게 해결할까?](https://engineerinsight.tistory.com/290)
- [[JAVA] 자바 쓰레드 동기화(1) - synchronized, wait()/notify()](https://jhkimmm.tistory.com/34)
- [[JAVA] 자바 쓰레드 동기화(2) - ReentrantLock과 Condition](https://jhkimmm.tistory.com/36)
