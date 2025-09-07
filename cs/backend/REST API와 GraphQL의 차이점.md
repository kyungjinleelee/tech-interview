# REST API와 GraphQL의 차이점

## 1. 정의와 접근 방식  
- **REST API**
  - 리소스(데이터 객체)를 URL 단위로 구분
  - HTTP 메서드(GET, POST, PUT, DELETE 등)로 리소스 조작  
- **GraphQL**
  - 쿼리 언어 및 런타임
  - 클라이언트가 필요한 데이터 구조를 직접 요청, 서버는 한 번에 응답

## 2. 데이터 요청과 응답
- **REST**
  - 엔드포인트별 고정 데이터 구조
  - 불필요한(over-fetching) 또는 부족한(under-fetching) 데이터가 올 수 있음  
- **GraphQL**
  - 단일 엔드포인트(graphql/api)
  - 원하는 데이터만 선택적으로 요청 가능 (over/under-fetching 방지)

## 3. 버전 관리
- **REST:** 새로운 버전 URL 분기(v1, v2 등) 자주 발생  
- **GraphQL:** 스키마 진화 방식, 버전 관리 필요성↓  
 
## 4. 실사용 예
- **REST:** https://api.example.com/users/1  
- **GraphQL:** 
    ```graphql
    query {
      user(id: 1) {
        name
        email
      }
    }
    ```

## 5. 장/단점 요약

| 구분     | REST API           | GraphQL                 |
|----------|--------------------|-------------------------|
| 학습 곡선 | 낮음               | 약간 높음               |
| 유연성   | 낮음(엔드포인트 고정)| 높음(필드 단위 선택)   |
| 확장성   | 엔드포인트/버전 관리 필요 | 스키마 진화로 관리 수월 |
| 대표 서비스 | Twitter, Github 등  | Facebook, Github 등   |

---

## 참고자료
- [REST 공식 문서](https://restfulapi.net/)
- [GraphQL 공식 문서](https://graphql.org/)
