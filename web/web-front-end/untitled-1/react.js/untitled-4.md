# 주요 개념 :: 06. Conditional Rendering

React에서 조건부 렌더링은 javascript의 조건처리와 같이 동작합니다.

if나 삼항연사자와 같은 javascript 연산자를 현재 상태를 나타내는 element를 만드는데 사용합니다. → 현재 상태에 맞는 컴포넌트를 rendering 한다.

```jsx
function UserGreeting(props){
	return <h1>Welcome back!</h1>;
}


function GuestGreeting(props){
	return <h1>Please sign up.</h1>;
}


function Greeting(props){
	const isLoggedIn = props.isLoggedIn;
	if (isLoggedIn){
		return <UserGreeting />;
	}
	return <GuestGreeting />;
}


ReactDOM.render(
	<Greeting isLoggedIn={false} />,
	document.getElementById('root')
);
```

**Element 변수**

element를 저장하기 위해 변수를 사용할 수 있다.

출력의 다른부분은 변하지 않은채, 컴포넌트의 일부를 조건부로 렌더링 할 수 있다.

로그아웃과 로그인 버튼을 나타내는 두 컴포넌트가 있다고 가정,

```jsx
function LoginButton(props){
	retrun (
		<button onClick={props.onClick}>
			Login
		</button>
	);
}


function LogoutButton(props){
	return (
		<button onClick={props.onClick}>
			Logout
		</button>
	);
}


class LoginControl extends React.Component {
	constructor(props){
		super(props);
		this.handleLoginClick = this.handleLoginClick.bind(this);
		this.handleLogoutClick = this.handleLogoutClick.bind(this);
		this.state = {isLoggedIn : false};
	}


	handleLoginClick(){
		this.setState({isLoggedIn : true});
	}


	handleLogoutClick(){
		this.setState({isLoggedIn : false});
	}


	render() {
		const isLoggedIn = this.state.isLoggedIn;
		let button;


		if(isLoggedIn){
			button = <LogoutButton onClick={this.handleLogoutClick} />;
		}else{
			button = <LoginButton onClick={this.handleLoginClick} />;
		}


		return (
			<div>
				<Greeting isLoggedIn={isLoggedIn} />
				{button}
			</div>
		);
	}
}
ReactDOM.render(
	<LoginControl />,
	document.getElementById('root')
);
```

**논리 && 연산자로 If를 인라인으로 표시**

변수를 선언하고 if를 사용해서 조건부로 렌더링 하는 것은 좋은 방법이다.

하지만, 더 짧은 구문으로 처리할 수 있다.

여러 조건을 JSX 안에서 인라인으로 처리하는 방법 소개.

```jsx
function Mailbox(props) {
	const unreadMessages = props.unreadMessages;
	return (
		<div>
			<h1>Hello !</h1>
			{ unreadMessages.length > 0 &&
				<h2>
					You Have {unreadMessages.length} unread messages.		
				</h2>
			}
		</div>
	);
}


const messages = ['React', 'Re: React', 'Re:Re: React'];
ReactDOM.render(
	<Mailbox unreadMessages={messages} />,
	document.getElementById('root')
);
```

<table>
  <thead>
    <tr>
      <th style="text-align:left">
        <p>Javascript&#xC5D0;&#xC11C; true &amp;&amp; expression&#xC740; expression&#xC73C;&#xB85C;
          &#xD3C9;&#xAC00;&#xB418;&#xACE0;,</p>
        <p>false &amp;&amp; expression&#xC740; false&#xC774;&#xB2E4;.</p>
        <p>&#xB530;&#xB77C;&#xC11C; &amp;&amp; &#xB4A4; element&#xB294; &#xC870;&#xAC74;&#xC774;
          true&#xC77C;&#xB54C;&#xB9CC; &#xCD9C;&#xB825;&#xB41C;&#xB2E4;.</p>
      </th>
    </tr>
  </thead>
  <tbody></tbody>
</table>**조건부 연산자로 If-Else 구문 인라인으로 표현**

Element를 조건부로 렌더링하는 다른 방법은 삼항연산자를 사용하는것이다.

```jsx
render(){
	const isLoggedIn = this.state.isLoggedIn;
	return (
		<div>
			The user is <b>{isLoggedIn ? 'currently' : 'not'}</b> logged in.
		</div>
	);
}
```

가독성은 떨어지지만, 더 큰 표현식에도 해당 구문을 사용할 수 있다.

```jsx
render() {
	const isLoggedIn = this.state.isLoggedIn;
	return (
		<div>
			{ isLoggedIn ? (
				<LogoutButton onClick={this.handleLogoutClick} />
			) : (
				<LoginButton onClick={this.handleLoginClick} />
			)}
		</div>
	);
}
```

**컴포넌트가 렌더링하는 것을 막기.**

가끔 다른 컴포넌트에 의해 렌더링 될 때 컴포넌트 자체를 숨기고 싶을 때가 있을 수 있다.

이때는 렌더링 결과를 출력하는 대신 null을 반환하면 해결할 수 있다.

```jsx
function WarningBanner(props){
	if(!props.warn){
		return null;
	}


	return (
		<div className="warning">
			Warning!
		</div>
	);
}


class Page extends React.Component {
	constructor(props){
		super(props);
		this.state = {showWarning: true};
		this.handleToggleClick = this.handleToggleClick.bind(this);
	}


	handleToggleClick() {
		this.setState(state => ({
			showWarning : !state.showWarning
		}));
	}


	render(){
		return(
			<div>
				<WarningBanner warn={this.state.showWarning} />
				<button onClick={this.handleToggleClick}>
					{this.state.showWarning? 'Hide' : 'Show'}
				</button>
			</div>
		);
	}
}


ReactDOM.render(
	<Page />,
	document.getElementById('root')
);
```

컴포넌트의 render method로 부터 null을 반환하는 것은 생명주기 method 호출에 영향을 주지 않는다.

