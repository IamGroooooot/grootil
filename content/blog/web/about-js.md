---
title: 'about js'
date: 2020-03-11 13:03:05
category: web
thumbnail: './images/book1.png'
---
[Event interface](https://dom.spec.whatwg.org/#interface-event)

`click`이라는 액션 예

```js
element.addEventListener('click', () => {
  console.log('clicked')
})
```

위 예제 코드에선 `element`라는 DOM Element에 `addEventListener`을 통해 `click` 액션에 핸들러를 추가

```js
element.click()
```

```js
const boom = 'boom'

console.log(boom) // boom!!!
```

```html
<p>Message from my computer:</p>
<p><samp>File not found.<br>Press F1 to continue</samp></p>
```

```js
aTag.addEventListener('click', e => {
  e.preventDefault()
})
```

```js
element.addEventListener('click', e => {
  e.stopPropagation()
})
```

### Case 1

```html
<table id="semantic-table" role="table" aria-label="Semantic Elements" aria-describedby="semantic_elements_table_desc" aria-rowcount="100" aria-colcount="2">
  <caption id="semantic_elements_table_desc">Semantic Elements to use instead of ARIA's roles</caption>
  <thead role="rowgroup">
    <tr role="row">
      <th role="columnheader" aria-sort="none" aria-rowindex="1">ARIA Role</th>
      <th role="columnheader" aria-sort="none" aria-rowindex="1">Semantic Element</th>
    </tr>
  </thead>
  <tbody role="rowgroup">
    <tr role="row">
      <td role="cell" aria-rowindex="11">header</td>
      <td role="cell" aria-rowindex="11">h1</td>
    </tr>
    <tr role="row">
      <td role="cell" aria-rowindex="16">header</td>
      <td role="cell" aria-rowindex="16">h6</td>
    </tr>
    <tr role="row">
      <td role="cell" aria-rowindex="18">rowgroup</td>
      <td role="cell" aria-rowindex="18">thead</td>
    </tr>
    <tr role="row">
      <td role="cell" aria-rowindex="24">term</td>
      <td role="cell" aria-rowindex="24">dt</td>
    </tr>
  </tbody>
</table>
```

```js
let el = document.getElementById('semantic-table');
console.log(el.ariaColCount); // 2
el.ariaColCount = "3"
console.log(el.ariaColCount); // 3
```

## 마무리

> 이 포스팅은 아래 글을 참조했습니다.

- [https://developer.mozilla.org/en-US/docs/Web/API/Element/ariaColCount](https://developer.mozilla.org/en-US/docs/Web/API/Element/ariaColCount)
