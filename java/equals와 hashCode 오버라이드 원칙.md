# equals()와 hashCode() 오버라이드 원칙

## 1. equals()와 hashCode()란?
- **equals()**: 두 객체의 논리적 동등성을 비교하는 메서드 (Object 클래스에서 상속)
- **hashCode()**: 객체의 해시값(정수)을 반환하는 메서드. 주로 HashMap, HashSet 등 해시 기반 컬렉션에서 사용

## 2. 오버라이드 원칙
- equals()를 오버라이드하면 hashCode()도 반드시 오버라이드해야 함
- 두 객체가 equals()로 같다고 판단되면, hashCode()도 같아야 함(반대는 성립하지 않음)
- **계약(Contract)**  
  - x.equals(y) == true 이면, x.hashCode() == y.hashCode() 여야 함
  - x.hashCode() == y.hashCode()라고 해서 x.equals(y)가 true일 필요는 없음

## 3. 왜 동시에 오버라이드해야 할까?
- HashSet, HashMap 사용 시, 해시값이 일치해야만 논리적 동등 비교 진행
- equals만 오버라이드하고 hashCode를 오버라이드하지 않으면,
  동등한 객체가 해시 기반 컬렉션에 동시에 여러 번 저장될 수 있음(버그)

## 4. 구현 예시

```java
@Override
public boolean equals(Object o) {
    if (this == o) return true;
    if (!(o instanceof User)) return false;
    User user = (User) o;
    return Objects.equals(id, user.id) && Objects.equals(name, user.name);
}

@Override
public int hashCode() {
    return Objects.hash(id, name);
}
```

## 5. 실수 & 주의할 점
- equals만 오버라이드함 → 해시 기반 컬렉션에서 문제 발생할 수 있음
- hashCode 구현 시, equals에 사용한 필드는 모두 hashCode에도 포함시켜야 함
- @Data(lombok) 등 자동 생성 시 원칙 자동 적용됨

## 6. 요약 정리
- equals와 hashCode는 항상 세트로 오버라이드!
- 컬렉션 동작의 핵심 원칙. 실무/면접 단골 질문

---

### 참고 자료
- [공식 Java docs: equals 및 hashCode의 일반 규약](https://docs.oracle.com/javase/8/docs/api/java/lang/Object.html#equals-java.lang.Object-)
- [Baeldung - equals() and hashCode() in Java](https://www.baeldung.com/java-equals-hashcode-contracts)
