# @Transactional의 원리와 주의점

## 1. @Transactional이란?
- Spring에서 선언적 트랜잭션 관리를 간편하게 지원하는 어노테이션
- 메소드(또는 클래스)에 부여하면, 해당 범위 내의 DB 작업이 “하나의 트랜잭션”으로 실행됨
- 예외 발생 시 전체 작업을 자동으로 롤백, 정상 종료 시 커밋

## 2. 동작 원리
- AOP(Aspect Oriented Programming)를 활용: 실제 메서드 호출 전후에 트랜잭션 시작/종료 논리를 자동 삽입
- 트랜잭션 프록시(Proxy)가 해당 객체의 메서드 호출을 감싸 수행  
- PlatformTransactionManager로 트랜잭션 시작/커밋/롤백 제어

## 3. Spring @Transactional의 기본 규칙
- 기본적으로 “런타임 예외(unchecked exception)” 발생 시 롤백, “체크 예외(checked exception)”에는 롤백하지 않음
- readOnly = true 옵션 지원: 조회 전용 트랜잭션 최적화  
- 롤백 조건 등 다양한 설정을 파라미터로 지정 가능

## 4. 주요 주의점 & 실수 사례
- **프록시 방식 한계:**  
  - “자기 자신의 내부 메서드 호출”엔 적용되지 않음 (프록시 바깥에서 진입해야 동작)
  - public 이외의 접근제어자 메서드에는 기본적용 안 됨
- **예외 처리 실수:**  
  - try-catch 내부에서 예외를 삼키면 롤백되지 않는다  
  - 체크 예외는 특별히 rollbackFor 옵션을 줘야 롤백됨
- **트랜잭션 전파/격리 수준 설정 실수**  
  - 여러 트랜잭션이 겹칠 때 전파 옵션(propagation)과 격리 레벨 확인 필요

## 5. 실전 사용 예시

```java
@Transactional
public void saveData() {
    // ... DB 작업들 ...
    if (error) throw new RuntimeException("롤백!");
}
```

## 6. 요약
- @Transactional은 Spring AOP 기반 트랜잭션 자동 관리
- AOP 프록시, 예외 타입, 내부 호출, 옵션처리 등 주의

---

### 참고 자료
- [공식 문서: Spring @Transactional](https://docs.spring.io/spring-framework/docs/current/reference/html/data-access.html#transaction)
- [Baeldung: Spring @Transactional 설명](https://www.baeldung.com/transaction-configuration-with-jpa-and-spring)
