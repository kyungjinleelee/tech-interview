# List, Set, Map의 내부 구조와 주요 차이점

## 1. List, Set, Map의 목적 및 기본 특징

| 컬렉션 종류 | 특징                           | 대표 구현체                |
|-------------|-------------------------------|-------------------------|
| List        | 순서(인덱스) 보장, 중복 허용     | ArrayList, LinkedList   |
| Set         | 중복 불허, 순서 보장 여부는 구현체별 | HashSet, LinkedHashSet, TreeSet |
| Map         | 키-값 쌍 저장, 키 중복 불허      | HashMap, LinkedHashMap, TreeMap |

---

## 2. 내부 구조

### List

- **ArrayList**
  - 내부 구조: 동적 배열(Object[] 사용)
  - 요소 추가/접근 속도 빠름 (인덱스 기반, O(1))
  - 중간/앞부분 삽입 삭제는 느림(O(n))
- **LinkedList**
  - 내부 구조: 이중 연결 리스트(Double Linked List)
  - 요소 삽입/삭제 빠름(특정 위치에서 O(1)), 검색은 느림(O(n))
- **특징:** 인덱스(index) 기반 접근, 중복 O, 순서 O

### Set

- **HashSet**
  - 내부 구조: 해시 테이블(HashMap 내부적으로 사용)
  - 요소 순서 보장 X, 중복 X
- **LinkedHashSet**
  - 내부 구조: 해시 테이블 + Linked List
  - 요소 추가 순서 유지 O, 중복 X
- **TreeSet**
  - 내부 구조: Red-Black Tree(이진 탐색 트리)
  - 정렬된 순서로 저장, 중복 X
- **특징:** 인덱스 사용 X, 중복 X

### Map

- **HashMap**
  - 내부 구조: 해시 테이블(키 → 값 매핑)
  - 순서 보장 X(자바 8 이후 일정 조건에서만)
  - 키 중복 X, 값 중복 O
- **LinkedHashMap**
  - 내부 구조: 해시 테이블 + Linked List
  - 입력 순서 유지
- **TreeMap**
  - 내부 구조: Red-Black Tree(이진 탐색 트리)
  - 키 기준 정렬
- **특징:** 키-값 쌍 저장, 키는 Set의 규칙 적용

---

## 3. 주요 차이점 정리(비교표)

|             | List           | Set            | Map                   |
|-------------|----------------|----------------|-----------------------|
| 중복 허용    | O              | X              | 키: X, 값: O          |
| 순서 보장    | O              | HashSet: X     | LinkedHashMap만 보장  |
| 접근 방식    | 인덱스         | 값             | 키                    |
| 대표 메소드  | get(index)     | contains(value)| get(key), put(key, v) |

---

## 4. 예제 코드

```java
List<String> list = new ArrayList<>();
list.add("A"); list.add("A"); // 중복 허용

Set<String> set = new HashSet<>();
set.add("A"); set.add("A"); // 중복 무시

Map<String, Integer> map = new HashMap<>();
map.put("A", 1); map.put("A", 2); // 마지막 값으로 덮어씀
```

---

## 5. 요약

- **List:** 순서 O, 중복 O
- **Set:** 순서(구현체마다), 중복 X
- **Map:** 키 중복 X, 값 중복 O, 키/값 쌍 관리

---

### 참고 자료
- [공식 Java 컬렉션 프레임워크 문서](https://docs.oracle.com/en/java/javase/17/docs/api/java.base/java/util/Collection.html)
- [Baeldung - Java Collections Guide](https://www.baeldung.com/java-collections)
