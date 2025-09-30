# Java Stream API의 실전 활용 패턴

## 1. Stream API란?

- Java 8에서 도입된 컬렉션 데이터 처리의 함수형 프로그래밍 도구
- 내부 반복(internal iteration) 기반: 요소를 추상화하여 연산을 선언적으로 표현
- **중간 연산**(intermediate)과 **최종 연산**(terminal)으로 구성

---

## 2. Stream의 기본 동작 구조

1. **컬렉션/배열/값 → Stream 생성**
2. **중간 연산**: map, filter, sorted 등 (하나 또는 여러 번 수행)
3. **최종 연산**: collect, forEach, reduce 등이 마지막에 실행

---

## 3. 실전 패턴 예제

### (1) Map/Filter/Collect

```java
List<String> names = Arrays.asList("Alice", "Bob", "Charlie");

List<String> result = names.stream()
    .filter(n -> n.length() <= 4)
    .map(String::toUpperCase)
    .collect(Collectors.toList());
// 결과: ["ALICE", "BOB"]
```

### (2) Reduce를 활용한 합계/통계 집계

```java
int sum = Arrays.asList(3, 5, 7).stream()
    .reduce(0, Integer::sum); // 결과: 15
```

### (3) GroupingBy/Counting 등 Collector 조합

```java
Map<Integer, Long> byLength = names.stream()
    .collect(Collectors.groupingBy(
        String::length,
        Collectors.counting()
    ));
// 결과: {3=1, 4=1, 7=1}
```

---

## 4. 실무 꿀팁 & 자주 하는 실수

- **Stream은 1회성!**: 한 번 소비된 stream은 재사용 불가 → 필요 시 .forEach(), .collect() 등 최종 연산 후 새로 생성 필요
- **지연 연산(Lazy Evaluation)**: 중간 연산은 실제 최종 연산이 실행될 때만 작동
- **병렬 처리**: .parallelStream() 사용 시 순서 보장X, 성능은 상황에 따라 다름
- **Null 처리 주의**: stream으로 변환하는 시점에 널 체크 필수

---

## 5. 실전 활용 패턴 예시

- 중간 연산 체이닝(map, filter, distinct 등)은 선언적 코드 스타일로 가독성↑
- List → Map 변환, 중복 제거, 통계 분석 등에서도 반복문 없이 간결 처리

```java
// List<String> -> Set<Character> 변환 예시
Set<Character> chars =
    names.stream()
        .flatMap(name -> name.chars().mapToObj(c -> (char) c))
        .collect(Collectors.toSet());
```

---

## 6. 한 줄 요약/TIP

- “Stream API는 복잡한 데이터 처리 로직을 짧고 명확하게 선언적으로 표현하게 해준다.”
- “for, if 반복문이 복잡할 때 Stream으로 리팩터링 해보면 코드가 더 읽기 쉬워짐!”

---

### 참고 자료

- [공식 Java Stream 문서](https://docs.oracle.com/javase/8/docs/api/java/util/stream/package-summary.html)
- [Baeldung - Guide to Java 8 Streams](https://www.baeldung.com/java-8-streams)
- [OKKY - Java Stream 실전 예제 모음](https://okky.kr/article/275734)
