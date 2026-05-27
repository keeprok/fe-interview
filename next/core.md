# ▲ Next.js

## 목차
- [Next.js를 사용하는 이유](#nextjs를-사용하는-이유)
- [SSR, SSG, ISR의 차이](#ssr-ssg-isr의-차이)
- [Hydration이란](#hydration이란)
- [App Router vs Pages Router](#app-router-vs-pages-router)

---

## Next.js를 사용하는 이유

> Next.js를 사용하는 이유와 이점을 설명해주세요.

<details>
<summary>답변 보기</summary>

**Next.js를 선택하는 이유:**
1. **SEO**: SSR/SSG로 완성된 HTML 제공 → 검색 엔진 크롤링 가능
2. **초기 로딩 속도**: 서버에서 렌더링된 HTML을 바로 전달
3. **파일 기반 라우팅**: `pages/` 또는 `app/` 폴더 구조가 곧 라우트
4. **API Routes**: 백엔드 API를 같은 프로젝트에서 관리
5. **이미지 최적화**: `next/image`로 자동 최적화
6. **코드 스플리팅**: 페이지 단위 자동 분리

</details>

---

## SSR, SSG, ISR의 차이

> Next.js의 렌더링 방식(SSR, SSG, ISR)의 차이를 설명해주세요.

<details>
<summary>답변 보기</summary>

| 방식 | 렌더링 시점 | 사용 사례 |
|------|-----------|----------|
| **SSG** (Static Site Generation) | 빌드 시간 | 블로그, 마케팅 페이지 |
| **SSR** (Server Side Rendering) | 요청마다 | 실시간 데이터, 사용자별 다른 컨텐츠 |
| **ISR** (Incremental Static Regeneration) | 빌드 + 주기적 재생성 | 자주 변하지 않는 데이터 |
| **CSR** (Client Side Rendering) | 브라우저 | 대시보드, 인증 필요 페이지 |

**App Router에서:**
```ts
// SSG (기본값 - force-cache)
const data = await fetch('https://api.example.com/posts');

// SSR
const data = await fetch('https://api.example.com/posts', {
  cache: 'no-store'
});

// ISR
const data = await fetch('https://api.example.com/posts', {
  next: { revalidate: 60 } // 60초마다 재검증
});
```

</details>

---

## Hydration이란

> Next.js에서 Hydration이란 무엇이고, SSR과 어떤 관계가 있나요?

<details>
<summary>답변 보기</summary>

**Hydration**은 서버에서 렌더링된 **정적 HTML에 JavaScript 이벤트 핸들러를 붙여 인터랙티브하게 만드는 과정**입니다.

**과정:**
1. 서버: 컴포넌트를 HTML 문자열로 렌더링
2. 브라우저: HTML 즉시 표시 (사용자가 빠르게 내용 볼 수 있음)
3. 브라우저: React JS 번들 로드
4. React: 서버 HTML과 Virtual DOM을 매칭하며 이벤트 핸들러 추가 → **Hydration**
5. 완성된 인터랙티브 페이지

**Hydration Mismatch:**
- 서버와 클라이언트 HTML이 다르면 경고/오류 발생
- `Math.random()`, `Date.now()`, `useId` 오용 등이 원인

**React 18 Selective Hydration:**
- `Suspense`를 사용해 컴포넌트 단위로 점진적 hydration 가능

</details>

---

## App Router vs Pages Router

> Next.js App Router와 Pages Router의 차이점을 설명해주세요.

<details>
<summary>답변 보기</summary>

| 구분 | Pages Router | App Router (Next.js 13+) |
|------|-------------|------------------------|
| 폴더 | `pages/` | `app/` |
| 기본 렌더링 | CSR | **Server Component** |
| 데이터 패칭 | `getServerSideProps`, `getStaticProps` | `fetch` with cache options |
| 레이아웃 | `_app.js` | `layout.tsx` (중첩 가능) |
| 로딩 UI | 별도 구현 | `loading.tsx` 자동 적용 |
| 에러 처리 | `_error.js` | `error.tsx` (경계 설정) |

**Server Component vs Client Component:**
```tsx
// Server Component (기본): 서버에서만 실행, JS 번들에 미포함
async function ServerComp() {
  const data = await fetch('...'); // 서버에서 직접 패칭
  return <div>{data}</div>;
}

// Client Component: 'use client' 디렉티브 필요
'use client';
function ClientComp() {
  const [state, setState] = useState(0); // 클라이언트 훅 사용 가능
  return <button onClick={() => setState(s => s + 1)}>{state}</button>;
}
```

</details>
