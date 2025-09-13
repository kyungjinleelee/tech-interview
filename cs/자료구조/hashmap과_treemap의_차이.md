# HashMap과 TreeMap의 차이

Java에서 가장 많이 사용되는 Map 컬렉션인 HashMap과 TreeMap은 내부 구조와 주요 특징이 다릅니다. 용도와 상황에 맞게 선택하는 것이 중요합니다.

---

## 1. 내부 구조 및 정렬 방식

- **HashMap**
  - 내부적으로 해싱(Hashing) 사용
  - 원소(키-값 쌍) 저장 시 순서 보장 없음
  - O(1)에 가까운 빠른 조회/삽입/삭제 성능(평균)

- **TreeMap**
  - 내부적으로 이진 탐색 트리(Red-Black Tree) 구조 사용
  - 키(Key) 기준으로 자동 정렬 (오름차순 기본)
  - O(logN)의 탐색 성능, 정렬 필요시 사용

---

## 2. Null 허용 여부

- **HashMap**: Key에 1개, Value에 여러 개의 null 허용
- **TreeMap**: Key에 null 허용하지 않음(NullPointerException 발생), Value엔 허용

---

## 3. 정렬 및 순회

- **HashMap**: 순서 무작위 (정렬 보장 X)
- **TreeMap**: 항상 정렬된 상태로 순회

---

## 4. 주요 사용 예시

- HashMap: 데이터가 빠른 조회가 필요하고, 순서가 중요하지 않을 때
- TreeMap: 키의 정렬이 필요하거나, 범위 쿼리(예: subMap, headMap, tailMap)를 자주 쓸 때

---

| 구분         | HashMap                | TreeMap                          |
|--------------|------------------------|----------------------------------|
| 내부구조     | 해시 테이블            | 이진 탐색 트리(Red-Black Tree)  |
| 정렬         | 보장 X                 | 키 기준 정렬                     |
| 성능         | ~O(1)                  | O(logN)                          |
| Null key     | 1개 허용               | 허용 안함                        |

---

## 참고자료
- [Java HashMap 공식문서](https://docs.oracle.com/javase/8/docs/api/java/util/HashMap.html)
- [Java TreeMap 공식문서](https://docs.oracle.com/javase/8/docs/api/java/util/TreeMap.html)
