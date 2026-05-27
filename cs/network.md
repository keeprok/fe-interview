# 🌐 네트워크

## 목차
- [HTTP 메소드 종류와 GET, POST 차이](#http-메소드-종류와-get-post-차이)
- [HTTP와 HTTPS의 차이](#http와-https의-차이)
- [CORS](#cors)
- [TCP와 UDP](#tcp와-udp)
- [RESTful API](#restful-api)
- [AJAX](#ajax)
- [멀티스레딩의 장단점](#멀티스레딩의-장단점)

---

## HTTP 메소드 종류와 GET, POST 차이

> HTTP 메소드의 종류와 GET과 POST의 차이를 설명해주세요.

<details>
<summary>답변 보기</summary>

**HTTP 메소드 종류:**
| 메소드 | 설명 |
|--------|------|
| GET | 리소스 조회 |
| POST | 리소스 생성 |
| PUT | 리소스 전체 수정 |
| PATCH | 리소스 일부 수정 |
| DELETE | 리소스 삭제 |
| HEAD | GET과 동일하나 Body 없이 헤더만 반환 |
| OPTIONS | 허용된 메소드 확인 (CORS Preflight) |

**GET vs POST:**
| 구분 | GET | POST |
|------|-----|------|
| 데이터 위치 | URL 쿼리스트링 | Request Body |
| 보안 | 낮음 (URL 노출) | 상대적으로 높음 |
| 캐싱 | 가능 | 불가 |
| 멱등성 | 있음 | 없음 |
| 데이터 길이 | 제한 있음 | 제한 없음 |
| 사용 사례 | 데이터 조회 | 데이터 생성/전송 |

</details>

---

## HTTP와 HTTPS의 차이

> HTTP와 HTTPS의 차이점을 설명해주세요.

<details>
<summary>답변 보기</summary>

- **HTTP**: HyperText Transfer Protocol, 데이터를 평문(암호화 없이) 전송
- **HTTPS**: HTTP + **TLS/SSL** 암호화 계층 추가

**HTTPS의 장점:**
1. **암호화**: 데이터가 암호화되어 중간자 공격(MITM) 방지
2. **인증**: 서버 신뢰성 보장 (CA 인증서)
3. **무결성**: 데이터 변조 방지
4. **SEO**: Google은 HTTPS 사이트를 검색 우선순위에서 우대

**TLS Handshake 과정:**
1. 클라이언트 → 서버: ClientHello (지원 암호화 방식 목록)
2. 서버 → 클라이언트: ServerHello + 인증서
3. 클라이언트: 인증서 검증, 세션 키 생성
4. 암호화 통신 시작

</details>

---

## CORS

> CORS는 무엇이고 어떻게 해결할 수 있나요?

<details>
<summary>답변 보기</summary>

**CORS(Cross-Origin Resource Sharing)**는 브라우저의 보안 정책으로, **다른 출처(Origin)의 리소스 요청을 제한**합니다.

- **Origin** = 프로토콜 + 도메인 + 포트
- `http://localhost:3000`과 `http://localhost:8080`은 **다른 Origin**

**발생 이유:** 악의적인 사이트가 사용자 정보를 훔치는 **CSRF 공격** 방지

**해결 방법:**
1. **서버에서 CORS 헤더 설정** (가장 권장)
   ```
   Access-Control-Allow-Origin: https://example.com
   ```
2. **Proxy 서버 사용** (개발환경에서 webpack-dev-server proxy 설정)
3. **JSONP** (GET 요청만 가능, 구식 방법)

**Preflight 요청:** POST, PUT 등 단순 요청이 아닌 경우 실제 요청 전에 OPTIONS 메소드로 사전 확인

</details>

---

## TCP와 UDP

> 네트워크에서 TCP와 UDP의 차이점과 각각 사용되는 상황을 설명해주세요.

<details>
<summary>답변 보기</summary>

| 구분 | TCP | UDP |
|------|-----|-----|
| 연결 방식 | 연결 지향 (3-way handshake) | 비연결 지향 |
| 신뢰성 | 높음 (패킷 손실 시 재전송) | 낮음 (손실 허용) |
| 순서 보장 | 있음 | 없음 |
| 속도 | 느림 | 빠름 |
| 혼잡 제어 | 있음 | 없음 |
| 사용 사례 | HTTP, 이메일, 파일 전송 | 스트리밍, DNS, 게임, VoIP |

**TCP 사용:** 데이터 무결성이 중요한 경우 (웹 요청, 파일 다운로드)
**UDP 사용:** 속도가 중요하고 약간의 손실이 허용되는 경우 (실시간 영상/음성)

</details>

---

## RESTful API

> RESTful API란 무엇인지 설명해주세요.

<details>
<summary>답변 보기</summary>

**REST(Representational State Transfer)**는 HTTP를 기반으로 한 **아키텍처 스타일**입니다.

**REST 6가지 원칙:**
1. **클라이언트-서버 분리**: UI와 데이터 처리 분리
2. **무상태(Stateless)**: 서버가 클라이언트 상태를 저장하지 않음
3. **캐시 가능(Cacheable)**: 응답을 캐싱할 수 있어야 함
4. **계층화 시스템**: 중간 서버를 클라이언트가 인식할 필요 없음
5. **인터페이스 일관성**: URI로 리소스 식별, HTTP 메소드로 행위 표현
6. **Code on Demand** (선택): 서버가 실행 가능한 코드 전달 가능

**RESTful URI 설계:**
```
GET    /users          → 전체 사용자 조회
GET    /users/{id}     → 특정 사용자 조회
POST   /users          → 사용자 생성
PUT    /users/{id}     → 사용자 전체 수정
PATCH  /users/{id}     → 사용자 일부 수정
DELETE /users/{id}     → 사용자 삭제
```

</details>

---

## AJAX

> AJAX란 무엇이며 어떻게 동작하나요?

<details>
<summary>답변 보기</summary>

**AJAX(Asynchronous JavaScript And XML)**는 페이지 새로고침 없이 **비동기적으로 서버와 데이터를 주고받는 기술**입니다.

**동작 원리:**
1. 사용자 이벤트 발생
2. `XMLHttpRequest` 또는 `fetch` API로 비동기 HTTP 요청
3. 서버에서 응답 (주로 JSON 형식)
4. JavaScript로 DOM 업데이트

**장점:** 페이지 전체를 다시 로드하지 않아 빠른 UX, 서버 부하 감소

**예시:**
```js
// fetch API
const data = await fetch('/api/users').then(res => res.json());
```

</details>

---

## 멀티스레딩의 장단점

> 멀티스레딩(Multi-threading)의 장점과 단점을 설명해주세요.

<details>
<summary>답변 보기</summary>

**장점:**
- CPU 자원을 효율적으로 활용 가능
- 동시에 여러 작업 처리 가능 → 응답 속도 향상
- 같은 프로세스 내 메모리 공유로 IPC 비용 절감

**단점:**
- **경쟁 조건(Race Condition)**: 여러 스레드가 동시에 같은 자원 접근 시 오류
- **교착 상태(Deadlock)**: 스레드들이 서로 자원을 기다리며 무한 대기
- 디버깅 복잡도 증가
- 동기화 비용 발생

</details>
