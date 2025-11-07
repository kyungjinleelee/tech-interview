## private 메서드에 @Transactional을 선언하면 트랜잭션이 동작할까요?
private 메서드에 @Transactional을 선언하면 트랜잭션이 **동작하지 않습니다.**

## 이유
### 1. Spring의 @Transactional은 AOP(프록시) 기반으로 동작
Spring에서 @Transactional은 **프록시 객체**를 사용하여 트랜잭션을 관리합니다.

스프링 aop에서 프록시는 크게 **`JDK Dynamic proxy`** 또는 **`CGLIB`** 으로 작동합니다. 그리고 spring boot 1.4 버전 이후부터는 default로 **`CGLIB`** 을 사용합니다.
**`CGLIB`** 은 동적으로 **상속**을 통해 프록시를 생성합니다. 따라서 private 메서드는 상속이 불가능하기 때문에 프록시로 만들어지지 않는 것입니다.

즉, @Transactional이 적용된 메서드는 프록시 객체를 통해 호출될 때만 트랜잭션이 적용됩니다.

### 2. private 메서드는 프록시 객체를 통해 호출되지 않음
Spring 프록시는 public 메서드에만 트랜잭션을 적용할 수 있습니다. 

private 메서드나 private 필드는 **해당 클래스에서만 접근 가능**하기 때문에 외부에서 접근해서 프록시 객체를 생성할 수 없게 되는데, 그래서 @Transactional 애너테이션을 가지고 있는 private 메서드에 접근하려고 해도 프록시 객체를 생성할 수 없기 때문에 해당 애너테이션을 무시하게 됩니다. 

private 메서드는 외부 클래스나 동일 클래스 내 다른 메서드에서 직접 호출될 뿐, 프록시를 거치지 않으므로 트랜잭션이 동작하지 않습니다.

#### 예제 코드
```java
@Service
public class TransactionService {

    @Transactional
    private void privateMethod() {
        // 트랜잭션이 적용되지 않음
        System.out.println("트랜잭션이 동작하지 않습니다.");
    }

    public void publicMethod() {
        privateMethod(); // 프록시를 거치지 않고 직접 호출 -> 트랜잭션 적용되지 않음
    }
}
```
## 트랜잭션이 적용되도록 하려면?
트랜잭션 적용을 원하면 public 메서드로 변경하거나, 프록시를 거치도록 해야 합니다. 
- 프록시를 거치도록 하기 위해선 자기 자신을 프록시로 주입 받아 프록시를 통해 메서드를 호출하거나, 별도의 클래스로 분리하거나, AspectJ를 이용하는 방법이 있습니다.
- AspectJ를 사용하면 동일 클래스 내에서의 메서드 호출에도 AOP 어드바이스를 적용할 수 있습니다.

### 1. 자기 자신을 프록시로 주입 받기
```java
@Slf4j  
@RequiredArgsConstructor  
@Service  
public class SelfInvocation {  
  
    private final MemberRepository memberRepository;  
    private final SelfInvocation selfInvocation;  
  
    public void outerSaveWithPublic(Member member) {  
        selfInvocation.saveWithPublic(member);  
    }  
  
    @Transactional  
    public void saveWithPublic(Member member) {  
        log.info("call saveWithPublic");  
        memberRepository.save(member);  
        throw new RuntimeException("rollback test");  
    }
    ...
}
```
이 방법은 순환 의존성 문제를 일으킬 수 있어 권장되지 않습니다.

### 2. 별도의 클래스로 분리하기
```java
@Slf4j  
@RequiredArgsConstructor  
@Service  
public class TransactionService {  
  
    @Transactional  
    public void outer() {  
        log.info("call outer");  
        logCurrentTransactionName();  
        logActualTransactionActive();  
        inner();  
    }  
  
    @Transactional(propagation = Propagation.REQUIRES_NEW)  
    public void inner() {  
        log.info("call inner");  
        logCurrentTransactionName();  
        logActualTransactionActive();  
    }  
  
    private void logActualTransactionActive() {  
        boolean actualTransactionActive = TransactionSynchronizationManager.isActualTransactionActive();  
        log.info("actualTransactionActive = {}", actualTransactionActive);  
    }  
  
    private void logCurrentTransactionName() {  
        String currentTransactionName = TransactionSynchronizationManager.getCurrentTransactionName();  
        log.info("currentTransactionName = {}", currentTransactionName);  
    }  
}

// 로그
// call outer  
// currentTransactionName = server.transaction.TransactionService.outer  
// actualTransactionActive = true  
// call inner  
// currentTransactionName = server.transaction.TransactionService.outer  
// actualTransactionActive = true
```
outer가 inner 메서드를 호출하는데, outer의 propagation 속성은 REQUIRED, inner는 REQUIRES_NEW로 서로 다른 트랜잭션으로 분리되어야 합니다.
하지만, 로그를 보면 동일한 outer의 트랜잭션에 속해있습니다. 이처럼 트랜잭션 전파 속성이 다른 두 메서드가 동일한 클래스 내부에서 self invocation 호출하면 의도대로 동작하지 않습니다.
이 때 outer와 inner 메서드를 각각 다른 클래스로 분리하여 호출하면 해결할 수 있습니다.

```java
// OuterTransactionService
@Slf4j  
@RequiredArgsConstructor  
@Service  
public class OuterTransactionService {  
  
    private final InnerTransactionService innerTransactionService;  
  
    @Transactional  
    public void outer() {  
        log.info("call outer");  
        logCurrentTransactionName();  
        logActualTransactionActive();  
        innerTransactionService.inner();  
    }  
  
    private void logActualTransactionActive() {  
        boolean actualTransactionActive = TransactionSynchronizationManager.isActualTransactionActive();  
        log.info("actualTransactionActive = {}", actualTransactionActive);  
    }  
  
    private void logCurrentTransactionName() {  
        String currentTransactionName = TransactionSynchronizationManager.getCurrentTransactionName();  
        log.info("currentTransactionName = {}", currentTransactionName);  
    }  
}

// InnerTransactionService
@Slf4j  
@RequiredArgsConstructor  
@Service  
public class InnerTransactionService {  
  
    @Transactional(propagation = Propagation.REQUIRES_NEW)  
    public void inner() {  
        log.info("call inner");  
        logCurrentTransactionName();  
        logActualTransactionActive();  
    }  
  
    private void logActualTransactionActive() {  
        boolean actualTransactionActive = TransactionSynchronizationManager.isActualTransactionActive();  
        log.info("actualTransactionActive = {}", actualTransactionActive);  
    }  
  
    private void logCurrentTransactionName() {  
        String currentTransactionName = TransactionSynchronizationManager.getCurrentTransactionName();  
        log.info("currentTransactionName = {}", currentTransactionName);  
    }  
}

// 로그
// call outer  
// currentTransactionName = server.transaction.OuterTransactionService.outer  
// actualTransactionActive = true  
// call inner  
// currentTransactionName = server.transaction.InnerTransactionService.inner  
// actualTransactionActive = true
```
이처럼 각각 프록시를 생성할 수 있게 두 클래스로 분리하면 AOP 어드바이스가 적용되어 의도한 대로 독립적인 트랜잭션을 시작할 수 있게 됐습니다.

## 결론
@Transactional은 **Proxy 형태**로 동작하기 때문에 외부에서 접근 가능한 메소드만 @Transactional 설정이 가능합니다.

### 참고 자료
- [Spring - Proxying Mechanisms](https://docs.spring.io/spring-framework/reference/core/aop/proxying.html)
- [Spring - Programmatic Transaction Management](https://docs.spring.io/spring-framework/reference/data-access/transaction/programmatic.html)
- [테코블 - AOP에 대한 사실과 오해 그런데 트랜잭션을 사알짝 곁들인..](https://tecoble.techcourse.co.kr/post/2022-11-07-transaction-aop-fact-and-misconception/)
