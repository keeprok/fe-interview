# 🌐 브라우저

## 목차
- [브라우저 동작 원리](#브라우저-동작-원리)
- [CSR과 SSR의 차이](#csr과-ssr의-차이)
- [쿠키, 세션, 로컬스토리지의 차이](#쿠키-세션-로컬스토리지의-차이)
- [HTML 렌더링 중 JavaScript 실행 시 렌더링이 멈추는 이유](#html-렌더링-중-javascript-실행-시-렌더링이-멈추는-이유)
- [Virtual DOM](#virtual-dom)

---

## 브라우저 동작 원리

> URL 입력부터 화면 렌더링까지의 전체 과정을 설명해주세요.

<details>
<summary>답변 보기</summary>

1. **URL 파싱** → 프로토콜, 도메인, 경로 분리
2. **DNS 조회** → 도메인을 IP 주소로 변환 (캐시 확인 후 없으면 DNS 서버 질의)
3. **TCP 연결** → 3-way handshake (HTTPS의 경우 TLS handshake 추가)
4. **HTTP 요청** → 서버에 GET 요청 전송
5. **서버 응답** → HTML 문서 수신
6. **HTML 파싱 & DOM 생성** → HTML 파서가 토큰화 → DOM Tree 생성
7. **CSS 파싱 & CSSOM 생성** → CSS 파서가 CSSOM Tree 생성
8. **JavaScript 실행** → `<script>` 태그를 만나면 파싱 중단 후 JS 실행
9. **Render Tree 생성** → DOM + CSSOM 합쳐서 실제 화면에 표시될 노드만 포함
10. **Layout(Reflow)** → 각 요소의 위치와 크기 계산
11. **Paint** → 픽셀로 화면에 그리기
12. **Composite** → 레이어를 합쳐서 최종 화면 출력

</details>

---

## CSR과 SSR의 차이

> CSR(Client Side Rendering)과 SSR(Server Side Rendering)의 차이점과 각각의 장단점을 설명해주세요.

<details>
<summary>답변 보기</summary>

| 구분 | CSR | SSR |
|------|-----|-----|
| 렌더링 위치 | 브라우저(클라이언트) | 서버 |
| 초기 로딩 | 느림 (JS 번들 다운로드 필요) | 빠름 (완성된 HTML 전달) |
| SEO | 불리 (초기 HTML이 비어있음) | 유리 (완성된 HTML 크롤링 가능) |
| 서버 부하 | 낮음 | 높음 |
| 이후 페이지 전환 | 빠름 | 느림 |
| 예시 | React SPA | Next.js, Nuxt.js |

**CSR 장점:** 페이지 이동 시 빠른 인터랙션, 서버 부하 적음
**SSR 장점:** SEO 최적화, 초기 로딩 속도 빠름, 소셜 미리보기 지원

</details>

---

## 쿠키, 세션, 로컬스토리지의 차이

> 브라우저 저장소(쿠키, 세션 스토리지, 로컬 스토리지)의 차이점을 설명해주세요.

<details>
<summary>답변 보기</summary>

| 구분 | 쿠키(Cookie) | 세션 스토리지 | 로컬 스토리지 |
|------|-------------|-------------|-------------|
| 만료 시간 | 설정 가능 | 탭/브라우저 닫으면 삭제 | 명시적 삭제 전까지 유지 |
| 용량 | 약 4KB | 약 5MB | 약 5~10MB |
| 서버 전송 | HTTP 요청 시 자동 전송 | 전송 안됨 | 전송 안됨 |
| 접근 범위 | 도메인 기반 | 같은 탭 | 같은 도메인 |
| 사용 사례 | 인증 토큰, 사용자 설정 | 임시 데이터, 폼 데이터 | 사용자 설정, 캐시 |

</details>

---

## HTML 렌더링 중 JavaScript 실행 시 렌더링이 멈추는 이유

> HTML 렌더링 도중 JavaScript가 실행되면 렌더링이 멈추는 이유를 설명해주세요.

<details>
<summary>답변 보기</summary>

JavaScript는 **DOM을 직접 조작**할 수 있기 때문입니다.

- `<script>` 태그를 만나면 HTML 파서가 **즉시 중단**됨
- JS가 `document.write()` 등으로 DOM을 변경할 수 있기 때문에, 파서가 계속 실행되면 충돌 가능성이 있음
- JS 실행이 완료된 후에야 다시 파싱 재개

**해결 방법:**
- `defer` 속성: HTML 파싱 완료 후 JS 실행 (순서 보장)
- `async` 속성: 다운로드 완료 즉시 실행 (순서 미보장)
- `<script>` 태그를 `</body>` 직전에 배치

</details>

---

## Virtual DOM

> Virtual DOM이 무엇이고, 이를 사용하는 이유와 장점은 무엇인가요?

<details>
<summary>답변 보기</summary>

**Virtual DOM**은 실제 DOM의 가벼운 JavaScript 복사본입니다.

**동작 원리:**
1. 상태(state) 변경 시 새로운 Virtual DOM 생성
2. 이전 Virtual DOM과 새 Virtual DOM을 **Diffing 알고리즘**으로 비교
3. 변경된 부분만 실제 DOM에 업데이트 (**Reconciliation**)

**사용 이유:**
- 실제 DOM 조작은 비용이 크기 때문에, 변경사항을 모아서 한 번에 처리
- 직접 DOM을 조작하지 않아도 되므로 **선언적 UI 작성** 가능

**장점:**
- 불필요한 DOM 조작 최소화 → 성능 향상
- 개발자가 DOM 최적화를 직접 신경쓰지 않아도 됨

</details>
