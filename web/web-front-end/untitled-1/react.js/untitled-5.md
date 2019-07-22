---
description: React의 event handling 방식에 대해 정리한다.
---

# 주요 개념 :: 05. Event Handling

React element에서 이벤트를 처리하는 방식은 DOM element에서 이벤트를 처리하는 방식과 비슷하다.

몇가지 문법적인 차이는 아래와 같다.

* React의 이벤트는 소문자 대신 camelCase를 사용한다.
* JSX를 사용하여 문자열이 아닌 함수로 event handler를 전달한다.

 **Event handler 등록**

{% code-tabs %}
{% code-tabs-item title="HTML Example" %}
```jsx
<button onclick="activateLasers()">
	Activate Lasers
</button>
```
{% endcode-tabs-item %}
{% endcode-tabs %}

{% code-tabs %}
{% code-tabs-item title="React Example" %}
```jsx
<button onClick={activateLasers}>
	Activate Lasers
</button>
```
{% endcode-tabs-item %}
{% endcode-tabs %}

 **이벤트 전파 방지**

{% code-tabs %}
{% code-tabs-item title="HTML Example" %}
```jsx
<a href="#" onclick="console.log('The link was clicked.'); return false">Click Me!</a>
```
{% endcode-tabs-item %}
{% endcode-tabs %}

{% code-tabs %}
{% code-tabs-item title="React Example" %}
```jsx
function ActionLink(){
	function handleClick(e){
		e.preventDefault();
		console.log('The link was Clicked');
	}
	
	return (
		<a href="#" onClick={handleClick}}>Click Me!</a>
	);
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

React에서는 return false를 해도 이벤트 전파를 막을 수 없다. 반드시 preventDefault를 명시적으로 호출해야 한다.

 **ES6 Class**

{% code-tabs %}
{% code-tabs-item title="ES6 Class Example" %}
```jsx
class Toggle extends React.Component {
	constructor(props){
		super(props);
		this.state = {isToggleOn: true};


		// call back에서 this가 작동하려면, 바인딩을 해주어야 한다.
		this.handleClick = this.handleClick.bind(this);
	}


	handleClick(){
		this.setStatus(state => ({
			isToggleOn: !state.isToggleOn
		}));
	}


	render(){
		return (
			<button onClick={this.handleClick}>
				{this.state.isToggleOn? "ON" : "OFF"}
			</button>
		);
	}
}


ReactDOM.render(
	<Toggle />,
	document.getElementById('root')
);
```
{% endcode-tabs-item %}
{% endcode-tabs %}

JSX callback에서 this의 의미에 대해 주의해야한다.

Javascript class Method는 기본적으로 바인딩 되어있지 않다.

this.handleClick을 바인딩 하지 않고 onClick에 전달하면, 실제 함수가 호출될 때 this는 undefined가 된다.

일반적으로, onClick={this.handleClick}과 같이 뒤에 \(\) 를 사용하지 않고 메서드를 참조하는 경우 해당 메서드를 바인딩 해야 한다.

**bind를 사용하지 않는 예 2가지.**

&gt;&gt; public class field 문법

// handleClick 이름을 함수를 참조하도록 한다. → 익명함수를 사용하여 해당 내용을 바인딩하는 처리.

```jsx
class LoggingButton extends React.Component  {
	// 이 문법은 `this`가 handleClick내에서 바인딩되도록 한다.
	// 실험적인 문법..
	handleClick = () => {
		console.log('this is : ', this);
	}


	render(){
		return (
			<button onClick={this.handleClick}>
				Click Me
			</button>
		)
	}
}
```

```jsx
class LoggingButton extends React.Component {
	handleClick(){
		console.log('this is : ', this);
	}


	render(){
		// 이 문법은 `this`가 handleClick 내에서 바인딩 되도록 한다.
		<button onClick={(e) => this.handleClick(e)}>
			Click me
		</button>
	}
}
/*
	주의점, LoggingButton이 렌더링될 때마다 다른 콜백이 생성된다는 점이다.
	만약 이 콜백이 하위 컴포넌트의 props로 전달되는 경우, 그 컴포넌트들은 추가로 다시 렌더링을 수행해야 한다. <-- 상위의 콜백이 바뀌어서.. 하위도 다시 렌더링해야한다.
    성능문제를 피하고자, 생성자 안에서 바인딩하거나 클래스 필드문법을 사용하는 것을 권장한다.
*/
```

**이벤트 핸들러에 인자 전달하기**

loop 내부에서는 event handler에 추가적인 매개변수를 전달하는 것이 일반적이다.

예를 들어, id가 행의 ID일 경우 아래 코드가 모두 작동한다.

```jsx
<button onClick={(e) => this.deleteRow(id, e)}>Delete Row</button>
<button onClick={this.deleteRow.bind(this, id)}>Delete Row</button>
```

두가지 예 모두 동일한 결과를 낸다.

두 경우 모두 React 이벤트를 나타내는 e 인자가 ID 뒤에 전달된다.

화살표 함수를 사용하면, 명시적으로 인자를 전달해야 하지만, bind를 사용하는 경우 추가 인자가 자동으로 전달된다.

