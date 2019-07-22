---
description: 'React.js의 컴포넌트에 대한 사용법을 정리하며, 각 컴포넌트에 전달하는 Props에 대해 설명한다.'
---

# 주요 개념 :: 03. Components and Props

컴포넌트를 통해 UI를 재사용 가능한 개별적인 여러 조각으로 나누고,

각 조각을 개별적으로 살펴볼 수 있다. – 넘나 CBD 스럽고 –

컴포넌트 개념을 소개한다.

상세 내용 참고 : [React.Component](https://ko.reactjs.org/docs/react-component.html)

**함수 컴포넌트와 클래스 컴포넌트**

컴포넌트를 정의하는 가장 간단한 방법은 Javascript 함수를 작성하는 것이다.

```jsx
function welcome(props){
	return <h1>Hello, {props.name}</h1>
}
```

이 함수는 데이터를 가진 하나의 "props"\(props는 속성을 나타내는 데이터이다.\)

객체 인자를 받고, React element를 반환하므로 유효한 React Component이다.

이러한 컴포넌트는 Javascript 함수이기 때문에, 말 그대로 \[함수 컴포넌트\] 라 한다.

ES6의 class를 활용하여 컴포넌트를 정의할 수 있다.

```jsx
class Welcome extends React.Component{
	render(){
		return <h1>Hello, {props.name}</h1>
	}
}
```

React의 관점에서 두 컴포넌트는 동일하다.

**컴포넌트 Rendering**

이전까지 React element를 DOM tag로 나타냈다.

```jsx
const element = <div />;
```

React element는 사용자 정의 컴포넌트로도 나타낼 수 있다.

```jsx
const element = <Welcome name ="Sara" />;
```

React 가 사용자 정의 컴포넌트로 작성한 element를 발견하면 JSX attribute를 해당 component의 단일 객체로 전달한다.

이 객체를 "props"라 한다.

```jsx
function Welcome(props) {
	return <h1>Hello, {props.name}</h1>;
}


const element = <Welcome name="Sara" />;
ReactDOM.render(
	element,
	document.getElementById('root')
);
```

<table>
  <thead>
    <tr>
      <th style="text-align:left">
        <p>&#xCEF4;&#xD3EC;&#xB10C;&#xD2B8;&#xC758; &#xC774;&#xB984;&#xC740; &#xD56D;&#xC0C1;
          &#xB300;&#xBB38;&#xC790;&#xB85C; &#xC2DC;&#xC791;&#xD55C;&#xB2E4;.</p>
        <p>React&#xB294; &#xC18C;&#xBB38;&#xC790;&#xB85C; &#xC2DC;&#xC791;&#xD558;&#xB294;
          &#xCEF4;&#xD3EC;&#xB10C;&#xD2B8;&#xB97C; DOM tag&#xB85C; &#xCC98;&#xB9AC;&#xD55C;&#xB2E4;.</p>
        <p>e.g., &lt;div /&gt; &#x2192; HTML div tag | &lt;Welcome /&gt; &#x2192;
          React Component name Welcome &#x2190; scope&#xB0B4;&#xC5D0; &#xC788;&#xC5B4;&#xC57C;
          &#xD55C;&#xB2E4;.</p>
      </th>
    </tr>
  </thead>
  <tbody></tbody>
</table>**컴포넌트 합성**

컴포넌트는 자신의 출력에 다른 컴포넌트를 참조할 수 있다.

이는 모든 세부 단계에서 동일한 추상 컴포넌트를 사용할 수 있음을 의미한다.

React app에서는 button, form, dialog, screen 등의 모든것들이 흔히 컴포넌트로 표현된다.

```jsx
function Welcome(props){
	return <h1>Hello, {props.name}</h1>
}


function App(){
	return (
		<div>
			<Welcome name="Sara" />
			<Welcome name="Cahal" />
			<Welcome name="Edite" />
		</div>
	);
}


ReactDOM.render(
	<App />,
	document.getElementById('root')
);
```

일반적으로 새 React Application은 최 상위에 단일 App component를 가지고 있다.

기존 Application에 React를 통합하는 경우에는 Button과 같은 작은 컴포넌트부터 시작해서 뷰 계층 상단으로 올라가면서 점진적으로 진행해야 한다.\(Bottom-up 방식으로\)

**컴포넌트 추출**

복잡한 단위의 컴포넌트를 작은 단위로 쪼개어, 가독성을 높일 수 있다.

**props는 읽기 전용이다.**

함수 컴포넌트나, 클래스 컴포넌트 모두 컴포넌트 자체의 props를 수정해서는 안된다.

```jsx
// 순수함수 ; 입력밧을 바꾸지 않고 항상 동일한 입력값에 대해 동일한 결과를 반환한다.
function sum(a,b) {
	return a + b;
}
```

```jsx
// 순수함수가 아닌경우.
function withdraw(account, amount){
	account.total -= amount;
}
```

React는 유연하지만, 엄격한 규칙이 하나 있다.

모든 React component는 자신의 props를 다룰때, 반드시 순수함수처럼 동작해야 한다.

