# ♿ 헤드리스 디자인 시스템 - 접근성 (a11y)

> 우선순위: ★★★ (다른 지원자와 확실히 차별화되는 영역)

## 목차
- [aria-disabled vs HTML disabled](#aria-disabled-vs-html-disabled)
- [Focus Trap](#focus-trap)
- [WAI-ARIA Role](#wai-aria-role)
- [aria-labelledby와 aria-label의 차이](#aria-labelledby와-aria-label의-차이)
- [aria-haspopup과 aria-expanded](#aria-haspopup과-aria-expanded)
- [Roving tabindex 패턴](#roving-tabindex-패턴)

---

## aria-disabled vs HTML disabled

> ★★★ aria-disabled와 HTML의 disabled 속성의 차이점은 무엇인가요?

<details>
<summary>답변 보기</summary>

| 구분 | `disabled` | `aria-disabled="true"` |
|------|-----------|----------------------|
| 포커스 | **완전히 제거** (탭으로 접근 불가) | 포커스 **유지** |
| 클릭 이벤트 | 발생하지 않음 | 발생함 (직접 막아야 함) |
| 스크린 리더 | "비활성화됨" 읽음 | "비활성화됨" 읽음 |
| 폼 제출 | 값 포함 안됨 | 값 포함됨 |

**언제 `aria-disabled`를 사용해야 하나요?**
- 버튼이 현재 사용 불가능하지만, 사용자가 **해당 버튼을 인식하고 이유를 알 수 있어야 할 때**
- 툴팁으로 비활성화 이유를 설명하는 경우 (포커스가 있어야 툴팁이 표시됨)
- 키보드 사용자가 비활성 상태임을 탐색 흐름에서 인지할 수 있어야 할 때

```tsx
// aria-disabled 사용 시 이벤트 직접 차단 필요
<button
  aria-disabled={isDisabled}
  onClick={(e) => {
    if (isDisabled) {
      e.preventDefault();
      return;
    }
    handleClick();
  }}
>
  제출
</button>
```

</details>

---

## Focus Trap

> ★★★ Focus Trap이란 무엇이고 왜 필요한가요? (Dialog에 구현됨)

<details>
<summary>답변 보기</summary>

**Focus Trap**은 다이얼로그가 열려 있는 동안 **키보드 포커스가 다이얼로그 내부에서만 순환**하도록 제한하는 기술입니다.

**왜 필요한가?**
- 모달이 열렸을 때 Tab 키로 모달 밖의 요소에 포커스가 가면 시각 장애인 사용자는 현재 컨텍스트를 잃게 됨
- WAI-ARIA Dialog 패턴의 필수 요건

**구현 원리:**
```tsx
useEffect(() => {
  if (!open) return;

  // 포커스 가능한 모든 요소 수집
  const focusableElements = dialogRef.current?.querySelectorAll(
    'a, button, input, textarea, select, [tabindex]:not([tabindex="-1"])'
  );
  const firstElement = focusableElements?.[0] as HTMLElement;
  const lastElement = focusableElements?.[focusableElements.length - 1] as HTMLElement;

  function handleKeyDown(e: KeyboardEvent) {
    if (e.key !== 'Tab') return;

    if (e.shiftKey) {
      // Shift+Tab: 첫 번째 요소에서 마지막으로 순환
      if (document.activeElement === firstElement) {
        e.preventDefault();
        lastElement.focus();
      }
    } else {
      // Tab: 마지막 요소에서 첫 번째로 순환
      if (document.activeElement === lastElement) {
        e.preventDefault();
        firstElement.focus();
      }
    }
  }

  document.addEventListener('keydown', handleKeyDown);
  firstElement?.focus(); // 다이얼로그 열리면 첫 번째 요소에 포커스

  return () => document.removeEventListener('keydown', handleKeyDown);
}, [open]);
```

**추가 요건:**
- `Escape` 키로 다이얼로그 닫기
- 다이얼로그 닫힐 때 이전 포커스 위치로 복원

</details>

---

## WAI-ARIA Role

> WAI-ARIA role="dialog", role="listbox", role="option"은 각각 언제 사용하나요?

<details>
<summary>답변 보기</summary>

**role="dialog":**
- 사용자 인터랙션이 필요한 팝업 창
- `aria-labelledby` 또는 `aria-label`로 제목 연결 필수
- `aria-modal="true"` 설정 권장 (스크린 리더에게 모달임을 알림)

```html
<div role="dialog" aria-modal="true" aria-labelledby="dialog-title">
  <h2 id="dialog-title">확인</h2>
  <p>정말 삭제하시겠습니까?</p>
</div>
```

**role="listbox"와 role="option":**
- Select 컴포넌트의 드롭다운 목록
- `aria-selected`로 선택 상태 표시
- `aria-activedescendant`로 현재 포커스된 옵션 참조

```html
<ul role="listbox" aria-labelledby="select-label">
  <li role="option" aria-selected="true" id="opt-1">서울</li>
  <li role="option" aria-selected="false" id="opt-2">부산</li>
</ul>
```

</details>

---

## aria-labelledby와 aria-label의 차이

> aria-labelledby와 aria-label의 차이점은 무엇인가요?

<details>
<summary>답변 보기</summary>

| 구분 | aria-label | aria-labelledby |
|------|-----------|----------------|
| 값 | 직접 텍스트 문자열 | 다른 요소의 ID 참조 |
| 사용 시점 | 화면에 보이는 레이블이 없을 때 | 화면에 이미 레이블 텍스트가 있을 때 |
| 우선순위 | 낮음 | 높음 (aria-labelledby가 우선) |

```html
<!-- aria-label: 아이콘만 있는 버튼 -->
<button aria-label="검색">
  <SearchIcon />
</button>

<!-- aria-labelledby: 화면의 텍스트를 레이블로 참조 -->
<h2 id="modal-title">설정 변경</h2>
<div role="dialog" aria-labelledby="modal-title">
  ...
</div>
```

</details>

---

## aria-haspopup과 aria-expanded

> aria-haspopup과 aria-expanded는 각각 어떤 상황에서 사용하나요?

<details>
<summary>답변 보기</summary>

**aria-haspopup:**
- 해당 요소가 팝업을 열 수 있음을 스크린 리더에 알림
- 값: `"true"`, `"menu"`, `"listbox"`, `"tree"`, `"grid"`, `"dialog"`

**aria-expanded:**
- 팝업/드롭다운의 현재 열림/닫힘 상태를 표시

```tsx
// Select Trigger 예시
<button
  aria-haspopup="listbox"
  aria-expanded={isOpen}
  aria-controls="select-listbox"
>
  {selectedValue || '선택하세요'}
</button>

<ul
  id="select-listbox"
  role="listbox"
  hidden={!isOpen}
>
  ...
</ul>
```

스크린 리더가 읽을 때: **"선택하세요, 콤보박스, 닫혀있음"** (또는 "열려있음")

</details>

---

## Roving tabindex 패턴

> ★★☆ Roving tabindex 패턴이 무엇인가요? (Select에 구현됨)

<details>
<summary>답변 보기</summary>

**Roving tabindex**는 컴포넌트 그룹(라디오 버튼, 탭, 리스트박스 등)에서 **Tab 키는 그룹 전체를 단위로, 화살표 키는 그룹 내부를 이동**하는 패턴입니다.

**왜 필요한가?**
- 옵션이 100개인 Select에서 Tab으로 하나씩 이동하면 매우 불편
- 사용자가 Tab으로 Select에 진입 후, 방향키로 옵션 탐색하는 게 자연스러움

**구현:**
```tsx
function SelectOption({ index, isActive, onSelect, children }) {
  return (
    <li
      role="option"
      // 활성화된 옵션만 tabindex="0", 나머지는 tabindex="-1"
      tabIndex={isActive ? 0 : -1}
      aria-selected={isActive}
      onClick={onSelect}
    >
      {children}
    </li>
  );
}

// 부모에서 키보드 핸들링
function handleKeyDown(e: KeyboardEvent) {
  if (e.key === 'ArrowDown') {
    const nextIndex = (currentIndex + 1) % options.length;
    setCurrentIndex(nextIndex);
    // 다음 옵션 DOM에 포커스
    optionRefs.current[nextIndex]?.focus();
  }
}
```

</details>
