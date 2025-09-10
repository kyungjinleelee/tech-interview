# HTTP 상태코드의 종류와 의미

HTTP 상태 코드는 클라이언트의 요청에 대한 서버의 응답 결과를 숫자로 표현한 것입니다.  
대표적인 상태코드는 다음과 같이 분류됩니다.

---

## 1xx: Informational (정보)
- **100 Continue**: 서버가 요청의 일부를 받았으며, 나머지 요청을 계속 해도 됨

## 2xx: Success (성공)
- **200 OK**: 요청 성공, 일반적인 응답
- **201 Created**: 성공적으로 리소스 생성
- **204 No Content**: 요청 성공, 응답 본문 없음

## 3xx: Redirection (리다이렉션)
- **301 Moved Permanently**: 리소스가 영구적으로 이동됨
- **302 Found**: 리소스가 임시로 이동

## 4xx: Client Error (클라이언트 에러)
- **400 Bad Request**: 잘못된 요청(문법 등)
- **401 Unauthorized**: 인증 필요
- **403 Forbidden**: 접근 권한 없음
- **404 Not Found**: 요청한 리소스를 찾을 수 없음

## 5xx: Server Error (서버 에러)
- **500 Internal Server Error**: 서버 내부 오류
- **502 Bad Gateway**: 게이트웨이/프록시 서버에서 잘못된 응답 수신
- **503 Service Unavailable**: 서버가 일시적으로 사용 불가

---

## 실무 예시

- API 개발 시, 클라이언트에게 명확한 상태코드를 반환해 의도를 전달
- 예: 사용자 인증 실패 시 401, 잘못된 요청 데이터면 400
- 204 No Content는 성공했지만 응답 바디가 필요없을 때 사용

---

## 참고자료
- [MDN HTTP 상태코드](https://developer.mozilla.org/ko/docs/Web/HTTP/Status)
- [RFC 7231: HTTP/1.1](https://datatracker.ietf.org/doc/html/rfc7231)
