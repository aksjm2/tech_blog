---
description: React DOM의 각 요소(Element)에 대한 Rendering에 대해 정리한다.
---

# 주요 개념 :: 02. Element Rendering

**Element는 React app의 최소 단위이다.**

```jsx
const element = <h1>hello, world</h1>;
```

브라우저 DOM element와는 달리 React element는 일반 객체이며\(plain object\) 쉽게 생성할 수 있다.

React DOM은 React element와 일치하도록 DOM을 update한다.

**DOM에 element rendering.**

HTML 파일 어딘가 &lt;div&gt; tag가 존재한다고 가정.

```jsx
<div id="root"></div>
```

이 안에 들어가는 모든 엘리먼트를 React DOM에서 관리하기 때문에 이것을 루트 DOM 노드라고 한다.

React로 구현된 application은 일반적으로 하나의 root DOM노드가 존재한다.

React를 기존 app에 통합하려는 경우 원하는 만큼 독립된 root DOM 노드가 있을 수 있다.

React Element를 루트 DOM 노드에 렌더링하려면, 둘 다 ReactDOM.render\(\)로 전달하면 된다.

```jsx
const element = <h1>Hello, World!</h1>;
ReactDOM.render(element, document.getElementById('root'));
```

**렌더링 된 element update.**

React Element는 불변객체이다.

element를 생성한 이후에는 해당 element의 자식이나 속성을 변경할 수 없다.

element는 영화에서 하나의 프레임과 같이 특정 시점의 UI를 보여준다.

01~03 까지 소개한 내용을 바탕으로 하면 UI를 업데이트하는 유일한 방법은 새로운 element를 만들고 이를 ReactDOM.render\(\)에 전달하는 것이다.

```jsx
function tick(){
	const element = (
		<div>
			<h1>Hello, world!</h1>
			<h2>It is {new Date().toLocaleTimeString()}.</h2>
		</div>
	);
	ReactDOM.render(element, document.getElementById('root'));
}


setInterval(tick, 1000);
```

<table>
  <thead>
    <tr>
      <th style="text-align:left">
        <p>&#xC2E4;&#xC81C; &#xB300;&#xBD80;&#xBD84;&#xC758; React App&#xC740; ReactDOM.render()&#xB97C;
          &#xD55C;&#xBC88;&#xB9CC; &#xD638;&#xCD9C;&#xD55C;&#xB2E4;.</p>
        <p>&#xCD94;&#xD6C4; &#xCEF4;&#xD3EC;&#xB10C;&#xD2B8;&#xC5D0;&#xC11C; &#xBCC0;&#xACBD;&#xB41C;
          &#xB0B4;&#xC6A9;&#xB9CC; update&#xD558;&#xB294; &#xBD80;&#xBD84;&#xC744;
          &#xB2E4;&#xB8F0; &#xC608;&#xC815;.</p>
        <p>&#xC0C1;&#xB2E8; &#xC608;&#xC81C;&#xCF54;&#xB4DC;&#xB294; &#xC804;&#xCCB4;
          DOM&#xC744; &#xB2E4;&#xC2DC; rendering&#xD558;&#xB294; &#xCF54;&#xB4DC;&#xAE30;
          &#xB54C;&#xBB38;&#xC5D0;, &#xBE44;&#xCD94;.</p>
      </th>
    </tr>
  </thead>
  <tbody></tbody>
</table>