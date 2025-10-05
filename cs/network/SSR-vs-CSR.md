# 서버 사이드 렌더링(SSR)과 클라이언트 사이드 렌더링(CSR) 차이

## 개념
- **SSR(Server Side Rendering)**
  - 사용자의 요청이 들어오면 서버에서 웹 페이지를 완성된 HTML 형태로 렌더링해서 클라이언트(브라우저)로 전달한다.
  - 사용자는 즉시 내용을 볼 수 있다.

- **CSR(Client Side Rendering)**
  - 서버에서는 최소한의 HTML, CSS, JS 파일만 전달한다.
  - 브라우저가 JS를 실행해서 실제로 페이지를 완성하고 렌더링한다.

## 장단점 비교

| SSR                                        | CSR                                         |
|---------------------------------------------|---------------------------------------------|
| 첫 페이지 로딩 속도 빠름                    | 초기 로딩(최초 요청)엔 느릴 수 있음         |
| SEO에 유리함 (검색 엔진이 HTML을 바로 읽음) | SPA 등 동적 UI에 더 적합                    |
| 서버 부하가 커질 수 있음                    | 사용자와 상호작용이 많은 앱에 적합           |
| 매 요청마다 서버에서 렌더링                  | 한 번 JS 받아오면 이후엔 빠른 전환 가능     |

## 활용 예시
- SSR: 뉴스 사이트, 블로그 등 검색 엔진 노출이 중요한 서비스
- CSR: 대시보드, 인터랙티브한 웹앱, SPA(Single Page Application)

---

### 참고 자료
- [MDN - Server-side rendering](https://developer.mozilla.org/en-US/docs/Glossary/Server-side_rendering)
- [MDN - Client-side rendering](https://developer.mozilla.org/en-US/docs/Glossary/Client-side_rendering)
- [Velog - SSR vs CSR 차이](https://velog.io/@velopert/SSR-vs-CSR)
