## SQL 조인(Join) 종류와 차이

관계형 데이터베이스에서 여러 테이블의 데이터를 조합할 때 사용하는 SQL 조인의 종류와 각각의 차이점에 대해 정리.

### 1. INNER JOIN
- 두 테이블 모두에서 일치하는 값이 있는 행만 반환
- 교집합 연산

### 2. LEFT (OUTER) JOIN
- 왼쪽(기준) 테이블의 모든 행을 반환, 오른쪽 테이블에 일치하는 값이 있으면 결합, 없으면 NULL
- 왼쪽 기준의 전체 집합 + 교집합

### 3. RIGHT (OUTER) JOIN
- 오른쪽(기준) 테이블의 모든 행을 반환, 왼쪽 테이블에 일치하는 값이 있으면 결합, 없으면 NULL
- 오른쪽 기준의 전체 집합 + 교집합

### 4. FULL (OUTER) JOIN
- 두 테이블의 모든 행을 반환, 일치하는 값이 있으면 결합, 없으면 한 쪽에 NULL
- 합집합 + 교집합

---

### 표로 한눈에 비교

| 조인 종류           | 기준 테이블 | 반환 행                                 | 반환 원리                |
|---------------------|-------------|-----------------------------------------|--------------------------|
| INNER JOIN          | 둘 다       | 교집합 (양쪽에 모두 존재)               | 일치하는 값만            |
| LEFT OUTER JOIN     | 왼쪽        | 왼쪽 전체 + 교집합                      | 왼쪽 기준, 없으면 NULL   |
| RIGHT OUTER JOIN    | 오른쪽      | 오른쪽 전체 + 교집합                    | 오른쪽 기준, 없으면 NULL |
| FULL OUTER JOIN     | 둘 다       | 합집합 (NULL 포함)                      | 없는 쪽은 NULL 채움      |

---

### 다이어그램으로 이해하기

```mermaid
venn
    A("LEFT 테이블") 
    B("RIGHT 테이블") 
    A & B : "INNER JOIN"
    A : "LEFT OUTER JOIN"
    B : "RIGHT OUTER JOIN"
    A | B : "FULL OUTER JOIN"
```

---

### 참고 SQL 예시

```sql
-- INNER JOIN 예시
SELECT * FROM student INNER JOIN score ON student.id = score.student_id;

-- LEFT OUTER JOIN 예시
SELECT * FROM student LEFT JOIN score ON student.id = score.student_id;

-- FULL OUTER JOIN 예시 (DBMS에 따라 다름)
SELECT * FROM student FULL OUTER JOIN score ON student.id = score.student_id;
```

---

### 참고 자료
- [SQL JOIN 종류 정리 (Velog)](https://velog.io/@csy1122/SQL-JOIN-%EC%A2%85%EB%A5%98-%EC%A0%95%EB%A6%AC)
- [W3Schools - SQL Joins](https://www.w3schools.com/sql/sql_join.asp)
