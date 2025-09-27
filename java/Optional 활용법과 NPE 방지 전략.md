# Optional 활용법과 NPE 방지 전략

## 1. Optional이란?

- Java 8에 추가된 클래스(java.util.Optional)
- 값이 있을 수도, 없을 수도 있는 상황을 명시적으로 표현
- NullPointerException(NPE) 리스크를 줄이고, 코드의 의도를 명확하게 함

---

## 2. 왜 Optional이 필요한가?

- 기존에는 null 반환 및 null 체크 → 실수로 NPE 발생 가능
- Optional을 통해 명시적으로 값의 부재(possible absence)를 표현 → 실수 예방 & 가독성 향상

---

## 3. Optional 생성과 기본 사용법

```java
// Optional 생성
Optional<String> opt1 = Optional.of("hello");    // 값이 반드시 있어야 함(Null 금지)
Optional<String> opt2 = Optional.ofNullable(null); // null이어도 예외 안 남
Optional<String> opt3 = Optional.empty();        // 빈 Optional
// Optional 사용 예 (값이 있으면 출력)
opt1.ifPresent(System.out::println);
// 값 꺼내기
String result = opt3.orElse("default");           // 값 없으면 "default"
String result2 = opt3.orElseGet(() -> "lambda");  // Supplier 사용
```

---

## 4. NPE 방지 & 안전한 Optional 패턴

- Null 체크 없이 안전하게 값을 처리
  - `ifPresent()`: 값이 있을 때만 동작
  - `orElse()`, `orElseGet()`: 기본값/지연 평가
  - `orElseThrow()`: 직접 예외 던지기

```java
String name = Optional.ofNullable(user.getName())
                      .orElse("Unknown");
```

- 체이닝

```java
Optional<User> userOpt = findUser();
String city = userOpt.map(User::getAddress)
                     .map(Address::getCity)
                     .orElse("도시 정보 없음");
```

- NPE 예방 원칙
  - 메서드 반환값/파라미터로 Optional 사용 → null 직접 반환/받기 지양
  - 내부 필드엔 Optional 대신 null 체크 코드 권장(JPA 엔티티 등 예외)

---

## 5. Optional 관련 실수와 주의점

- Optional 내부에 또 Optional/컬렉션/스트림 등 중첩 X(불필요하게 복잡)
- JSON 직렬화/역직렬화에서 Optional을 필드에 직접 쓸 땐 라이브러리 호환 이슈 필요
- Optional.get()은 값 없을 때 예외, 직접 호출보다 orElse 계열 권장

---

## 6. 요약

- Optional을 통해 NPE 리스크를 줄이고, 보다 안전하고 명시적인 코드 유지 가능
- null 처리 조건문 없는 체계적 값 처리 가능
- 적절한 위치(메서드 반환값)에만 제한적으로 활용 추천

---

### 참고 자료
- [공식 자바 Optional 문서](https://docs.oracle.com/en/java/javase/17/docs/api/java.base/java/util/Optional.html)
- [Baeldung - Optional in Java 8](https://www.baeldung.com/java-optional)
