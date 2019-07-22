# 주요 개념 :: 10. Composition vs Inheritance

React는 강력한 합성모델을 가지고있다. 상속 대신 합성을 사용하여 컴포넌트간에 코드를 재사용 하는것이 좋다.

#### 컴포넌트에서 다른 컴포넌트 담기

어떤 컴포넌트들은 어떤 자식 Element가 들어올 지 미리 예상할 수 없는 경우가 있다.

범용적인 box 역할을 하는 Sidebar, Dialog 와 같은 컴포넌트에서 특히 자주 볼 수 있다.

이러한 컴포넌트에서는 특수한 children prop을 사용하여 자식 element를 출력에 그대로 전달하는 것이 좋다.

```jsx
function FancyBorder(props) {
    return (
        <div className={'FancyBorder FancyBorder-' + props.color } >
            {props.children}
        </div>
    );
}
```

이러한 방식으로 다른 컴포넌트에서 JSX를 중첩하여 임의의 자식을 전달할 수 있다.

```jsx
function WelcomeDialog(){
    return (
        <FancyBorder color="blue">
            <h1 className="Dialog-title">
                Welcome
            </h1>
            <p className="Dialog-message">
                Thank you for visiting our spacecraft!
            </p>
        </FancyBorder>
    );
}
```

&lt;FancyBorder&gt; JSX 태그 안에 있는 것들이 FancyBorder 컴포넌트의 children prop로 전달된다.

FancyBorder는 {props.children}을 &lt;div&gt;안에 렌더링 하므로 전달된 element들이 최종 출력된다.

흔하지 않지만 종종 컴포넌트에 여러개의 "구멍"이 필요할 수도 있다.

이런경우에는 children 대신 자신만의 고유한 방식을 적용할 수 있다.

```jsx
function SplitPane(props){
    return(
        <div className="SplitPane">
            <div className="SplitPane-left">
                {props.left}
            </div>
            <div className="SplitPane-right">
                {props.right}
            </div>
        </div>
    );
}

function App() {
    return (
        <SplitPane
            left={
                <Contacts />
            }
            right={
                <Chat />
            }
        />
    );
}
```

&lt;Contact /&gt;와 &lt;Chat /&gt; 같은 React Element는 단지 객체이기 때문에 다른 데이터처럼 prop으로 전달할 수 있다.

이러한 접근은 다른 라이브러리의 슬롯\(slots\)과 비슷해보이지만 React에서 prop로 전달할 수 있는 것에는 제한이 없다.

#### 특수화

때로 어떤 컴포넌트의 특수한 경우인 컴포넌트를 고려해야 하는 경우가 있다.

예를 들어, WelcomeDialog는 Dialog의 특수한 경우라고 할 수 있다.

React에서는 이 역시 합성을 통해 해결한다.

더 구체적인 컴폰너트가 일반적인 컴포넌트를 렌더링하고, props를 통해 내용을 구성한다.

```jsx
function Dialog(props){
    return(
        <FancyBorder color="blue">
            <h1 className="Dialog-title">
                {props.title}
            </h1>
            <p className="Dialog-message">
                {props.message}
            </p>
        </FancyBorder>
    );
}

function WelcomeDialog(){
    return (
        <Dialog
            title="Welcome"
            message="Thank you for visiting our spacecraft!" />
    );
}
```

합성은 클래스로 정의된 컴포넌트에서도 동일하게 적용된다.

```jsx
function Dialog(props){
    return(
        <FancyBorder color="blue">
            <h1 className="Dialog-title">
                {props.title}
            </h1>
            <p className="Dialog-message">
                {props.message}
            </p>
            {props.children}
        </FancyBorder>
    );
}

class SignUpDialog extends React.Component {
    constructor(props){
        super(props);
        this.handleChange = this.handleChange.bind(this);
        this.handleSignUp = this.handleSignUp.bind(this);
        this.state = {login : ''};
    }

    render() {
         return (
            <Dialog title="Mars Exploration Program"
                message="How should we refer to you?" >
                <input value={this.state.login} onChange={this.handleChange} />

                <button onClick={this.handleSignUp}>
                    Sign Me Up!
                </button>
            </Dialog>
         );
    }

    handleChange(e){
        this.setState({login: e.target.value});
    }

    handleSignUp(){
        alert(`Welcome aboard, ${this.state.login}!`);
    }
}
```

#### 상속은?

Facebook에서는 수천개의 React 컴포넌트를 사용하지만, 컴포넌트를 상속 계층구조로 작성을 권장할 만한 사례가 없다.

props와 합성은 명시적이고 안전한 방법으로 컴포넌트의 모양과 동작을 커스터마이징 하는데 필요한 모든 유연성을 제공한다.

컴포넌트가 원시타입의 값, React Element 혹은 함수 등 어떠한 props도 받을 수 있다는 것을 기억해라.

UI가 아닌 기능을 여러 컴포넌트에서 재사용하기를 원한다면, 별도의 JavaScript 모듈로 분리하는것이 좋다.

컴포넌트에서 해당 함수, 객체, 클래스 등을 import 하여 사용할 수 있다. – 상속 받을 필요 없이..

