## JPA에서 Soft Delete를 구현하는 방법은 무엇인가요?

> Soft Delete란?
>
> **Soft Delete**는 데이터를 실제로 삭제하지 않고, 삭제된 것처럼 표시만 하는 방식이다.
> 예를 들어, isDeleted 같은 컬럼을 두고, 해당 값이 true면 삭제된 것으로 간주한다.
> 이 방식은 데이터 복구, 이력 추적, 참조 무결성 유지 등에 유리하다.

### 1. 단순히 필드만 바꾸는 방법
가장 간단한 방법은 삭제 API에서 엔티티의 `isDeleted`(혹은 `deleted`) 필드만 true로 바꿔주는 것이다.
```java
entity.setIsDeleted(true);
repository.save(entity);
```

**장점**
1. 구현이 쉽다.

**단점**
1. 모든 조회 쿼리에서 isDeleted = false 조건을 직접 추가해야 한다.
2. 실수로 조건을 빼먹으면 삭제된 데이터가 노출될 수 있다.

### 2. @SQLDelete, @Where 애너테이션 활용
JPA(Hibernate)에서는 `@SQLDelete`와 `@Where` 애너테이션을 활용해 Soft Delete를 자동화할 수 있다.
```java
@Entity
@SQLDelete(sql = "UPDATE my_entity SET is_deleted = true WHERE id = ?")
@Where(clause = "is_deleted = false")
public class MyEntity {
    // ...
}
```
- **`@SQLDelete`**: 엔티티 삭제 시 실제로는 DELETE 쿼리가 아니라 UPDATE 쿼리(논리 삭제)가 실행된다.
- **`@Where`**: 엔티티를 조회할 때 항상 `is_deleted = false` 조건이 자동으로 붙는다.

**장점**
1. 논리 삭제가 자동으로 처리된다.
2. 모든 조회 쿼리에 조건이 자동으로 붙어 실수 위험이 줄어든다.
3. 코드 일관성과 유지보수성이 높아진다.

**단점**
1. JPA/Hibernate에 종속적이다.

### 정리
- 작은 프로젝트 나 간단한 경우 에는 직접 필드만 바꿔도 되지만, 쿼리 실수로 인해 삭제된 데이터가 노출될 수 있으니 주의가 필요하다.
- 규모가 크거나 여러 곳에서 해당 엔티티를 다룬다면 `@SQLDelete`, `@Where` 애너테이션을 사용하는 것이 더 안전하고 편리할 수 있다.

---
### 연관 질문
- [논리 삭제와 물리 삭제의 차이점은 무엇인가요?](https://github.com/kyungjinleelee/tech-interview/blob/main/cs/database/%EB%85%BC%EB%A6%AC%20%EC%82%AD%EC%A0%9C%EC%99%80%20%EB%AC%BC%EB%A6%AC%20%EC%82%AD%EC%A0%9C%EC%9D%98%20%EC%B0%A8%EC%9D%B4%EC%A0%90%EC%9D%80%20%EB%AC%B4%EC%97%87%EC%9D%B8%EA%B0%80%EC%9A%94%3F.md)
### 참고 자료
- [JPA에서 Soft Delete를 구현하는 방법](https://engineerinsight.tistory.com/172)
