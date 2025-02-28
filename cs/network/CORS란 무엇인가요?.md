## CORS란 무엇인가요?
- CORS(Cross Origin Resource Sharing)는 **웹 브라우저가 다른 도메인(origin)의 리소스를 요청할 때 보안 정책을 적용**하는 방식입니다.
- 즉, 브라우저가 "다른 도메인"의 리소스를 요청할 수 있도록 허용하거나 차단하는 메커니즘입니다.
- 리소스를 주고받는 두 곳의 출처가 다르면 출처가 교차한다고 합니다.
- 이때 출처는 URL뿐만 아니라 프로토콜과 포트까지 포함됩니다.
- 만약 클라이언트의 출처가 허용되지 않았다면 CORS 에러가 발생할 수 있습니다.

--------------------------------

## CORS는 왜 필요한가요?
- 과거에는 **크로스 사이트 요청 위조**(CSRF, Cross-Site Request Forgery) 문제가 있었습니다. 피해자의 브라우저에서 다른 애플리케이션으로 가짜 클라이언트 요청을 전송하는 공격입니다.
- CSRF를 예방하기 위해 브라우저는 **동일 출처 정책**(SOP, same-origin policy)을 구현했습니다. SOP가 구현된 브라우저는 클라이언트와 동일한 출처의 리소스로만 요청을 보낼 수 있습니다.
  - 즉, 자바스크립트가 다른 도메인의 리소스에 접근하는 것을 제한합니다.
  
- 하지만 API 서버와 클라이언트가 다른 도메인에 있을 경우(예: frontend.com ↔ api.backend.com), 이 정책이 너무 강력하여 데이터를 주고받을 수 없게 됩니다.

- CORS는 **SOP의 한계를 해결하면서도 보안을 유지**하기 위해 사용됩니다.
  - 즉, 특정 도메인에서만 데이터를 요청할 수 있도록 허용하는 방식입니다.

-----------------------------------

## CORS는 어떻게 작동할까요? 🤔
- 브라우저가 요청 메시지에 Origin 헤더와 응답 메시지의 Access-Control-Allow-Origin 헤더를 비교해서 CORS를 위반하는지 확인합니다. 이때, Origin에는 현재 요청하는 클라이언트의 출처(프로토콜, 도메인, 포트)가, Access-Control-Allow-Origin은 리소스 요청을 허용하는 출처가 작성됩니다.

- 이렇게 단순하게 요청하는 것을 **Simple Request(단순 요청)** 라고 합니다.
  - Simple Request은 요청 메서드(GET, POST, HEAD), 수동으로 설정한 요청 헤더(Accept, Accept-Language, Content-Language, Content-Type, Range), Content-Type 헤더(application/x-www-form-urlencoded, multipart/form-data, text/plain)인 경우에만 해당합니다.

- 브라우저가 사전 요청을 보내는 경우도 있습니다. 이때 사전 요청을 **Preflight Request** 라고 합니다. 브라우저가 본 요청을 보내기 이전, Preflight Request를 OPTIONS 메서드로 요청을 보내어 실제 요청이 안전한지 확인합니다.

  - Preflight Request는 추가로 Access-Control-Request-Method로 실 요청 메서드와, Access-Control-Request-Headers 헤더에 실 요청의 추가 헤더 목록을 담아서 보내야 합니다.

  - 이에 대한 응답은 대응되는 Access-Control-Allow-Methods와 Access-Control-Headers를 보내야 하고, Preflight Request로 인한 추가 요청을 줄이기 위해 캐시 기간을 Access-Control-Max-Age에 담아서 보내야 합니다.

- 또한 인증된 요청을 사용하는 방식도 있는데요. 이를 **Credential Request**라고 합니다. 쿠키나 토큰과 같은 인증 정보를 포함한 요청은 더욱 안전하게 처리되어야 합니다. 이때 Credential Request를 수행합니다.

  - Credential Request를 요청하는 경우에는 서버에서는 Access-Control-Allow-Credentials를 true로 설정해야 하며 Access-Control-Allow-Origin에 와일드카드를 사용하지 못합니다.

------------------------

## 결론
- CORS는 브라우저의 보안 정책(SOP)을 우회하면서도, 안전한 도메인 간 데이터 공유를 가능하게 하는 메커니즘
- 서버에서 Access-Control-Allow-Origin 헤더를 설정하여 특정 도메인만 접근 가능하도록 허용
- 단순 요청(Simple Request)은 바로 요청되지만, 사전 요청(Preflight Request)이 필요한 경우 먼저 OPTIONS 요청을 보냄
- 백엔드에서 CORS를 적절히 설정하지 않으면 API 요청이 차단될 수 있음

### 참고 및 추가 자료
- [토스페이먼츠 - 결제창에서 CORS 대응하기](https://docs.tosspayments.com/blog/payment-window-cors-error#%EA%B2%B0%EC%A0%9C%EC%B0%BD%EC%97%90%EC%84%9C-cors-%EB%8C%80%EC%9D%91%ED%95%98%EA%B8%B0)
- [MDN - 교차 출처 리소스 공유 (CORS)](https://developer.mozilla.org/ko/docs/Web/HTTP/CORS)
- [테코블 - CORS란?](https://tecoble.techcourse.co.kr/post/2020-07-18-cors/)
