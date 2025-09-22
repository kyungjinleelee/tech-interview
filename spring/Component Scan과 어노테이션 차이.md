# Component Scan과 @Component, @Service, @Repository 차이

## 1. Component Scan이란?
- Spring이 특정 패키지 이하에서 @Component 계열 어노테이션이 붙은 클래스들을 자동으로 스캔(탐지)하여, 빈(Bean)으로 등록하는 기능
- 기본적으로 `@SpringBootApplication`(또는 `@ComponentScan`)이 선언된 클래스의 패키지 하위가 스캔 대상

## 2. @Component 계열 어노테이션

### 1) @Component
- 가장 기본적인 "스프링 빈 등록" 어노테이션
- 개발자가 만든 모든 일반 빈에 사용 가능

### 2) @Service
- 서비스(비즈니스 로직) 계층을 나타내는 역할
- @Component의 확장 개념으로 기능은 같으나, 코드 의미상 분리
- 관례적으로 “서비스 레이어” 클래스에 사용

### 3) @Repository
- 데이터 엑세스 계층(DAO)에 사용하는 어노테이션
- @Component의 확장.  
- **추가 기능:** 예외 변환 (Spring Data Access Exception 변환 기능 적용됨)
- 관례적으로 "DB 작업"의 DAO/Repository 구현 클래스에 사용

### 4) @Controller
- 웹/REST 컨트롤러에 사용하는 어노테이션 (참고: REST에서는 @RestController)

## 3. 정리

| 어노테이션     | 용도                | 스프링 빈 자동 등록 | 추가 기능/차별화                  |
| -------------- | ------------------ | ------------------ | --------------------------------- |
| @Component     | 범용(일반)         | O                  | 없음                              |
| @Service       | 서비스 (비즈 로직)  | O                  | 의미상 구분, 관례                 |
| @Repository    | 데이터 접근 (DAO)   | O                  | 예외 변환(예외 핸들러)            |
| @Controller    | 웹 컨트롤러        | O                  | 웹 요청 처리(뷰/html or REST 등)  |

## 4. 사용 예시

```java
@Component
public class FileUtils {}

@Service
public class UserService {}

@Repository
public class UserRepository {}
```

## 5. 참고
- 직접적인 기능 차이보다 “관례적 계층 분리”가 가장 큼  
- 예외: @Repository는 예외 변환 기능이 추가

---

### 참고 자료
- [Spring 공식 문서: Component Scan](https://docs.spring.io/spring-framework/docs/current/reference/html/core.html#beans-annotation-config)
- [Baeldung: Spring Stereotype Annotations](https://www.baeldung.com/spring-component-repository-service)
