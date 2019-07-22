# 주요 개념 :: 07. List & Key

#### Background

Javascript에서 List를 어떻게 처리하는지 확인.

```javascript
const numbers = [1, 2, 3, 4, 5];
const doubled = number.map((number) => number * 2);
console.log(doubled);
```

 → result : \[2,4,6,8,10\]

React에서 배열을 element list로 만드는 방식은 이와 거의 동일하다.

#### Multi component rendering

Element의 모음을 만들고, 중괄호 {} 를 사용하여 JSX에 포함시킬 수 있다.

javascript map\(\) 함수를 사용하여 numbers 배열을 반복실행한다.

각 항목에 대해 &lt;li&gt; element를 반환하고, element 배열의 결과를 listItems에 저장한다.

```jsx
const numbers= [1,2,3,4,5];
const listItems = number.map((number) => 
	<li>{number}</li>
);
// listItems 배열을 ul element에 포함시키고, DOM에 렌더링 한다.
ReactDOM.render(
	<ul>{listItems}</ul>,
	document.getElementById('root')
);
```

#### Default Component List

일반적으로 컴포넌트 안에서 리스트를 렌더링한다.

위 예제에서 numbers 배열을 받아, 순서없는 element list를 출력하는 컴포넌트로 리펙토링 한다.

```jsx
function NumberList(props) {
	const numbers = props.numbers;
	const listItems = numbers.map((number) => 
		<li>{number}</li>
	);


	return (
		<ul>{listItems}</ul>
	);
}


const numbers = [1,2,3,4,5];
ReactDOM.render(
	<NumberList numbers={numbers} />,
	document.getElementById('root');
);
```

-. 위 코드를 실행하면 리스트의 각 항목에 key를 넣어야 한다는 경고가 표시된다.

-. "Key"는 Element 리스트를 만들 때 포함해야 하는 특수한 문자열 attribute이다.

```jsx
function NumberList(props) {
	const numbers = props.numbers;
	const listItems = numbers.map((number) => 
		<li key={number.toString()}> // Key 누락문제 해결.
			{number}
		</li>
	);
	return (
		<ul>{listItems}</ul>
	);
}


const numbers = [1,2,3,4,5];
ReactDOM.render(
	<NumberList numbers={numbers} />,
	document.getElementById('root');
);
```

#### key

Key는 React가 어떤 항목을 변경, 추가 또는 삭제할지 식별하는것을 돕는다.

Key는 element에 안정적인 고유성을 부여하기 위해 배열 내부의 element에 지정해야 한다.

Key를 선택하는 가장 좋은 방법은 리스트의 다른 항목들 사이에서 해당 항목을 고유하게 식별할 수 있는 문자열을 사용하는 것이다.

대부분의 경우 데이터의 ID를 Key로 잡는다.

```jsx
const todoItems = todos.map((todo) =>
	<li key={todo.id}>
		{todo.text}
	</li>
);
```

렌더링 한 항목에 대한 안정적인 ID가 없다면 최후의 수단으로 항목의 인덱스를 Key로 사용할 수 있다.

```jsx
const todoItems = todos.map((todo) =>
	<li key={index}>
		{todo.text}
	</li>
);
```

<table>
  <thead>
    <tr>
      <th style="text-align:left">
        <p>&#xD56D;&#xBAA9;&#xC758; &#xC21C;&#xC11C;&#xAC00; &#xBC14;&#xB014; &#xC218;
          &#xC788;&#xB294; &#xACBD;&#xC6B0; Key&#xC5D0; index&#xB97C; &#xC8FC;&#xB294;&#xAC83;&#xC744;
          &#xAD8C;&#xC7A5;&#xD558;&#xC9C0; &#xC54A;&#xB294;&#xB2E4;.</p>
        <p>&#xC774;&#xB85C;&#xC778;&#xD574; &#xC131;&#xB2A5;&#xC774; &#xC800;&#xD558;&#xB418;&#xAC70;&#xB098;,
          &#xCEF4;&#xD3EC;&#xB10C;&#xD2B8;&#xC758; state&#xC640; &#xAD00;&#xB828;&#xB41C;
          &#xBB38;&#xC81C;&#xAC00; &#xBC1C;&#xC0DD;&#xD560; &#xC218; &#xC788;&#xB2E4;.</p>
        <p>&#xB9CC;&#xC57D; &#xB9AC;&#xC2A4;&#xD2B8; &#xD56D;&#xBAA9;&#xC5D0; &#xBA85;&#xC2DC;&#xC801;&#xC73C;&#xB85C;
          key&#xB97C; &#xC9C0;&#xC815;&#xD558;&#xC9C0; &#xC54A;&#xC73C;&#xBA74; React&#xB294;
          &#xAE30;&#xBCF8;&#xC801;&#xC73C;&#xB85C; index&#xB97C; key&#xB85C; &#xC0AC;&#xC6A9;&#xD55C;&#xB2E4;.</p>
      </th>
    </tr>
  </thead>
  <tbody></tbody>
</table>#### key로 component 추출하기

Key는 주변 배열의 context에서만 의미가 있다.

예를 들면, ListItem 컴포넌트를 추출할 경우, ListItem 안에 있는 &lt;li&gt; element가 아니라 배열의 &lt;ListItem /&gt; element가 key를 가져야 한다.

{% code-tabs %}
{% code-tabs-item title="Wrong Example" %}
```jsx
function ListItem(props) {
	const value = props.value;
	return (
		// 여기에 key를 지정하는 것이 아니라.
		<li key={value.toString()}>
			{value}
		</li>
	);
}


function NumberList(props) {
	const numbers = props.numbers;
	const listItems = number.map((number) =>
		// 여기에 Key를 지정해야 한다.
		<ListItem value={number} />	
	);


	return (
		<ul>
			{listItems}
		</ul>
	);
}


const numbers = [1,2,3,4,5]
ReactDOM.render(
	<NumberList numbers={numbers} />,
	document.getElementById('root')
);
```
{% endcode-tabs-item %}
{% endcode-tabs %}

{% code-tabs %}
{% code-tabs-item title="Right Example" %}
```jsx
function ListItem(props) {
	// key를 지정할 필요가 없다.
	return <li>{props.value}</li>;
}


function NumberList(props) {
	const numbers = props.numbers;
	const listItems = number.map((number) =>
		<ListItem key={number.toString()} value={number} /> // 여기에 지정한다. 실제 배열 객체에 들어가는 단위가 뭔지 잘 생각해봐야 한다.
	);

	return (
		<ul>
			{listItems}
		</ul>
	);
}


const numbers = [1,2,3,4,5]
ReactDOM.render(
	<NumberList numbers={numbers} />,
	document.getElementById('root')
);
```
{% endcode-tabs-item %}
{% endcode-tabs %}

| 공식 가이드에 경험상 map\(\) 함수 내부에 있는 element에 key를 넣어주는 것이 좋다고 한다. |
| :--- |


#### Key는 형제 사이에서만 고유한 값이어야 한다.

Key는 배열 안의 형제 사이에서 고유해야 하고, 전체 범위에서 고유할 필요는 없다.

두 개의 배열을 만들때 동일한 Key를 사용할 수 있다.

```jsx
function Blog(props) {
	const sidebar = (
		<ul>
			{props.posts.map((post) =>
				<li key={post.id}>
					{post.title}
				</li>
			)}
		</ul>
	);


	const content = props.posts.map((post) => 
		<div key={post.id}>
			<h3>{post.title}</h3>
			<p>{poist.content}</p>
		</div>
	);


	return (
		<div>
			{sidebar}
			<hr />
			{content}
		</div>
	);
}


const posts = [
  {id: 1, title: 'Hello World', content: 'Welcome to learning React!'},
  {id: 2, title: 'Installation', content: 'You can install React from npm.'}
];
ReactDOM.render(
  <Blog posts={posts} />,
  document.getElementById('root')
);
```

React에서 Key는 힌트를 제공하지만, 컴포넌트로 전달하지 않는다.

컴포넌트에서 Key와 동일한 값이 필요하면 다른 이름의 prop로 명시적인 전달이 가능하다.

```jsx
const content = posts.map((post) => 
	<Post key={post.id}
		  id={post.id}
		  title={post.title} />
);
```

#### JSX에 map\(\) 포함시키기

JSX를 사용하면 중괄호 안에 모든 표현식을 포함시킬 수 있으므로, map\(\) 함수의 결과를 인라인으로 처리할 수 있다.

```jsx
function NumberList(props) {
	const numbers = props.numbers;
	const listItems = numbers.map((number) =>
		<ListItem key={number.toString()}
				  value={number} />
	);
	return (
		<ul>
			{listItems}
		</ul>
	);
}


// Inline으로 처리
function NumberList(props) {
  const numbers = props.numbers;
  return (
    <ul>
      {numbers.map((number) =>
        <ListItem key={number.toString()}
                  value={number} />
      )}
    </ul>
  );
}
```

<table>
  <thead>
    <tr>
      <th style="text-align:left">
        <p>inline &#xCC98;&#xB9AC;&#xB97C; &#xC0AC;&#xC6A9;&#xD558;&#xBA74; &#xCF54;&#xB4DC;&#xAC00;
          &#xAE54;&#xB054;&#xD574;&#xC9C0;&#xC9C0;&#xB9CC;, &#xAC00;&#xB3C5;&#xC131;&#xC774;
          &#xC88B;&#xC9C0; &#xC54A;&#xC544;&#xC9C4;&#xB2E4;.</p>
        <p>Javascript&#xC640; &#xB9C8;&#xCC2C;&#xAC00;&#xC9C0;&#xB85C;, &#xAC00;&#xB3C5;&#xC131;&#xC744;
          &#xC704;&#xD574; &#xBCC0;&#xC218;&#xB85C; &#xCD94;&#xCD9C;&#xD574;&#xC57C;
          &#xD560;&#xC9C0; Inline&#xC73C;&#xB85C; &#xB123;&#xC5B4;&#xC57C; &#xD560;&#xC9C0;&#xB294;
          &#xAC1C;&#xBC1C;&#xC790;&#xAC00; &#xC9C1;&#xC811; &#xD310;&#xB2E8;&#xD574;&#xC57C;
          &#xD55C;&#xB2E4;.</p>
        <p>map() &#xD568;&#xC218;&#xAC00; &#xB108;&#xBB34; &#xC911;&#xCCA9;&#xB41C;&#xB2E4;&#xBA74;
          &#xCEF4;&#xD3EC;&#xB10C;&#xD2B8;&#xB85C; &#xCD94;&#xCD9C;&#xD558;&#xB294;
          &#xAC83;&#xC774; &#xC88B;&#xB2E4;.</p>
      </th>
    </tr>
  </thead>
  <tbody></tbody>
</table>