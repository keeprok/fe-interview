# 🎨 CSS

## 목차
- [CSS 선택자 우선순위](#css-선택자-우선순위)
- [반응형 웹의 3요소](#반응형-웹의-3요소)
- [Flex와 Grid 차이](#flex와-grid-차이)
- [UI/UX](#uiux)
- [CSS Cascading](#css-cascading)
- [CSS-in-JS](#css-in-js)

---

## CSS 선택자 우선순위

> CSS 선택자 우선순위를 설명해주세요.

<details>
<summary>답변 보기</summary>

우선순위가 높은 순서:
1. `!important` (사용 지양)
2. 인라인 스타일 (`style="..."`)
3. ID 선택자 (`#id`) → 0,1,0,0
4. 클래스, 속성, 가상 클래스 (`.class`, `[attr]`, `:hover`) → 0,0,1,0
5. 태그, 가상 요소 (`div`, `::before`) → 0,0,0,1
6. 전체 선택자 (`*`) → 0,0,0,0

**명시도(Specificity)**: 선택자 종류를 (인라인, ID, 클래스, 태그) 네 자리 숫자로 계산

```css
#nav .item a:hover { } /* 0,1,1,1 */
.item a            { } /* 0,0,1,1 */
```
같은 우선순위라면 **나중에 선언된 스타일**이 적용됩니다.

</details>

---

## 반응형 웹의 3요소

> 반응형 웹의 3가지 핵심 요소를 설명해주세요.

<details>
<summary>답변 보기</summary>

1. **유동적인 그리드(Fluid Grid)**: 고정 px 대신 %, em 등 상대 단위 사용
2. **유연한 이미지(Flexible Images)**: `max-width: 100%`로 이미지가 컨테이너를 넘지 않도록
3. **미디어 쿼리(Media Query)**: 뷰포트 크기에 따라 다른 스타일 적용

```css
@media (max-width: 768px) {
  .container {
    flex-direction: column;
  }
}
```

</details>

---

## Flex와 Grid 차이

> Flexbox와 CSS Grid의 차이점을 설명해주세요.

<details>
<summary>답변 보기</summary>

| 구분 | Flexbox | Grid |
|------|---------|------|
| 차원 | **1차원** (행 또는 열 하나) | **2차원** (행과 열 동시) |
| 사용 목적 | 아이템 정렬, 배포 | 전체 레이아웃 설계 |
| 콘텐츠 기반 | 콘텐츠 크기에 따라 유연 | 그리드 셀에 맞게 배치 |

**언제 사용?**
- **Flexbox**: 네비게이션 바, 버튼 그룹, 카드 목록
- **Grid**: 페이지 전체 레이아웃, 갤러리, 복잡한 2D 배치

</details>

---

## UI/UX

> UI와 UX의 차이점을 설명해주세요.

<details>
<summary>답변 보기</summary>

- **UI (User Interface)**: 사용자가 실제로 보는 화면 - 색상, 폰트, 버튼 디자인 등 시각적 요소
- **UX (User Experience)**: 사용자가 제품을 사용하며 느끼는 전반적인 경험 - 사용성, 흐름, 편의성

**좋은 UX를 위한 원칙:**
- 일관성 (Consistency)
- 피드백 (Feedback)
- 접근성 (Accessibility)
- 오류 방지와 복구 (Error Prevention & Recovery)

</details>

---

## CSS Cascading

> CSS의 Cascading에 대해 설명해주세요.

<details>
<summary>답변 보기</summary>

**Cascading**은 CSS에서 여러 스타일이 충돌할 때 **어떤 스타일을 적용할지 결정하는 규칙**입니다.

우선순위 결정 순서:
1. **출처(Origin)**: 브라우저 기본 < 개발자 스타일 < 사용자 스타일 < `!important`
2. **명시도(Specificity)**: 선택자의 구체성
3. **순서(Order)**: 나중에 선언된 스타일이 우선

</details>

---

## CSS-in-JS

> CSS-in-JS의 특징과 장단점을 설명해주세요.

<details>
<summary>답변 보기</summary>

**CSS-in-JS**는 JavaScript 파일 안에서 CSS를 작성하는 방식입니다. (styled-components, emotion 등)

**장점:**
- 컴포넌트 단위로 스타일 캡슐화 → 클래스명 충돌 없음
- JavaScript 변수/로직 사용 가능 (동적 스타일)
- 사용하지 않는 스타일 자동 제거

**단점:**
- 런타임에 스타일 생성 → 초기 렌더링 성능 저하
- 번들 크기 증가
- SSR 시 추가 설정 필요

**Zero-runtime CSS-in-JS** (vanilla-extract, Linaria): 빌드 타임에 CSS 생성하여 성능 문제 해결

</details>
