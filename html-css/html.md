# 📄 HTML

## 목차
- [Attribute와 Property의 차이](#attribute와-property의-차이)
- [event.target과 event.currentTarget의 차이](#eventtarget과-eventcurrenttarget의-차이)
- [버블링과 캡처링](#버블링과-캡처링)

---

## Attribute와 Property의 차이

> Attribute와 Property의 차이점을 설명해주세요.

<details>
<summary>답변 보기</summary>

- **Attribute**: HTML 문서에 정의된 정적인 값, `getAttribute()`로 접근
- **Property**: DOM 객체에 존재하는 동적인 값, JavaScript로 직접 접근

```html
<input type="text" value="초기값" />
```

```js
const input = document.querySelector('input');

// Attribute: HTML에 정의된 초기값
input.getAttribute('value'); // "초기값" (변경 안됨)

// Property: 현재 DOM 상태
input.value; // 사용자가 변경하면 업데이트됨
```

**핵심 차이:** Attribute는 초기값, Property는 현재 상태

</details>

---

## event.target과 event.currentTarget의 차이

> event.target과 event.currentTarget의 차이를 설명해주세요.

<details>
<summary>답변 보기</summary>

- **`event.target`**: 실제로 이벤트가 발생한 요소 (클릭한 바로 그 요소)
- **`event.currentTarget`**: 이벤트 핸들러가 등록된 요소 (현재 이벤트를 처리 중인 요소)

```html
<div id="parent">
  <button id="child">클릭</button>
</div>
```

```js
document.getElementById('parent').addEventListener('click', (e) => {
  console.log(e.target);        // <button id="child"> (실제 클릭된 요소)
  console.log(e.currentTarget); // <div id="parent"> (핸들러가 등록된 요소)
});
```

**이벤트 위임 패턴**에서 `e.target`을 활용해 어떤 자식이 클릭됐는지 판별합니다.

</details>

---

## 버블링과 캡처링

> 이벤트 버블링과 캡처링에 대해 설명해주세요.

<details>
<summary>답변 보기</summary>

**이벤트 전파 3단계:**
1. **캡처링(Capturing)**: 이벤트가 최상위(window)에서 타겟 요소로 내려오는 단계
2. **타겟(Target)**: 이벤트가 실제 발생한 요소에 도달
3. **버블링(Bubbling)**: 이벤트가 타겟에서 최상위로 올라가는 단계

```js
// 기본은 버블링 단계에서 실행
element.addEventListener('click', handler);

// 캡처링 단계에서 실행하려면
element.addEventListener('click', handler, true);

// 버블링 중단
event.stopPropagation();
```

**이벤트 위임(Event Delegation):** 버블링을 이용해 부모 요소에 이벤트 핸들러를 달아 자식 요소들의 이벤트를 처리하는 패턴. 동적으로 추가되는 요소에 유용하고 메모리 효율적입니다.

</details>
