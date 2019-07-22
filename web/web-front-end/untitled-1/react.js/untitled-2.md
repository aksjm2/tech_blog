# 주요 개념 :: 08. Form

#### 제어 컴포넌트\(Controlled Component\)

HTML에서 &lt;input&gt;, &lt;textarea&gt;, &lt;select&gt; 와 같은 form element는 일반적으로 사용자의 입력을 기반으로 자신의 state를 관리하고 업데이트 한다.

React에서는 변경할 수 있는 state가 일반적으로 컴포넌트의 state속성에 유지되며, setState\(\)에 의해 업데이트 된다.

React state를 신뢰 가능한 단일 출처\(single source of truth\)로 만들어 두 element를 결합할 수 있다.

폼을 렌더링하는 React 컴포넌트는 form에 발생하는 사용자 입력값을 제어한다.

이러한 방식으로 React에 의해 값이 제어되는 입력 form element를 제어 컴포넌트\(controlled component\)라고 한다.

```jsx
class NameForm extends React.Component {
    constructor(props){
        super(props);
        this.state = {value : ''};

        this.handleChange = this.handleChange.bind(this);
        this.handleSubmit = this.handleSubmit.bind(this);
    }

    handleChange(event) {
        this.setState({value : event.target.value});
    }

    handleSubmit(event) {
        alert("A name was submitted : " + this.state.value);
        event.preventDefault();
    }

    render() {
        return (
            <form onSubmit={this.handleSubmit}>
                <label>
                    Name : 
                    <input type="text" value={this.state.value} onChange={this.handleChange} />
                </label>
                <input type="submit" value="Submit" />
            </form>
        );
    }
}
```

value attribute는 form element에 설정되므로, 표시되는 값은 항상 this.state.value가 되고 React state는 신뢰가능한 단일 출처\(single source of truth\)가 된다.

React state를 업데이트 하기 위해 모든 키 입력에서 handleChange가 동작하기 때문에 사용자가 입력할 때 보여지는 값이 업데이트 된다.

제어 컴포넌트로 사용하면 모든 state 변화는 연관된 핸들러를 가진다.

이를 통해 사용자 입력을 수정하거나 유효성을 검사하는 것이 간단해 진다.

#### textarea 태그

```markup
<textarea>
	Hello there, this is some text in textarea
</textarea>
```

→ html에서 textarea element는 텍스트를 자식으로\(child node\) 정의한다.

React에서 &lt;textarea&gt;는 value attribute를 대신 사용한다.

```jsx
class EssayForm extends React.Component {
    constructor(props) {
        super(props);
        this.state = {
            value: 'Please write an essay about your favorite DOM element.'
        };

        this.handleChange = this.handleChange.bind(this);
        this.handleSubmit = this.handleSubmit.bind(this);
    }

    handleChange(event) {
        this.setState({value : event.target.value});
    }

    handleSubmit(event) {
        alert('An essay was submitted : ' + this.state.value);
        event.preventDefault();
    }

    render() {
        return (
            <form onSubmit={this.handleSubmit}>
                <label>
                    Essay:
                    <textarea value={this.state.value} onChange={this.handleChange} />
                </label>
                <input type="submit" value="Submit" />
            </form>
        );
    }
}
```

#### Select 태그

html에서는 selected option을 통해 초기값을 설정한다.

→ React에서는 최상단 select tag에 value attribute를 통해 선택 값을 처리한다.

```jsx
class FlavorForm extends React.Component {
    constructor(props){
        super(props);
        this.state = {value : 'coconut'};

        this.handleChange = this.handleChange.bind(this);
        this.handleSubmit = this.handleSubmit.bind(this);
    }

    handleChange(event) {
        this.setState({value : event.target.value});
    }

    handleSubmit(event) {
        alert('Your favorite flavor is ' + this.state.value);
        event.preventDefault();
    }

    render() {
        return (
            <form onSubmit={this.handleSubmit}>
                <label>
                    Pick your favorite flavor : 
                    <select value={this.state.value} onChange={this.handleChange}>
                        <option value="grapefruit">Grapefruit</option>
                        <option value="lime">Lime</option>
                        <option value="coconut">Coconut</option>
                        <option value="mango">Mango</option>
                    </select>
                </label>
                <input type="submit" value="Submit" />
            </form>
        );
    }
}
```

전반적으로 &lt;input type="text"&gt;, &lt;textarea&gt;, &lt;select&gt; 모두 매우 비슷하게 동작한다.

모두 제어 컴포넌트를 구현하는데 value attribute를 사용한다.

<table>
  <thead>
    <tr>
      <th style="text-align:left">
        <p>select tag&#xC5D0; multiple option&#xC744; &#xD5C8;&#xC6A9;&#xD55C;&#xB2E4;&#xBA74;,
          value attribute&#xC5D0; &#xBC30;&#xC5F4;&#xC744; &#xC804;&#xB2EC;&#xD560;
          &#xC218; &#xC788;&#xB2E4;.</p>
        <p>&lt;select multiple={true} value={[&apos;B&apos;, &apos;C&apos;]}&gt;</p>
      </th>
    </tr>
  </thead>
  <tbody></tbody>
</table>#### File input tag

HTML의 &lt;input type="file"&gt;은 사용자에게 하나 이상의 파일을 자신의 장치에서 서버로 업로드 하거나 File API를 통해 Javascript로 조작할 수 있다.

```jsx
<input type="file" />
```

값이 읽기 전용이기 때문에 React에서는 비제어 컴포넌트다.

#### 다중입력 제어

여러 input element를 제어할 때, 각 element에 name attribute를 추가하고 event.target.name 값을 통해 handler가 어떤 작업을 할 지 선택할 수 있게 해준다.

```jsx
class Reservation extends React.Component {
    constructor(props) {
        super(props);
        this.state = {
            isGoing = true,
            numberOfGuests: 2
        };

        this.handleInputChange = this.handleInputChange.bind(this);
    }

    handleInputChange(event) {
        const target = event.traget;
        const value = target.type === 'checkbox' ? target.checked : target.value;
        const name = target.name;

        this.setState({
            [name]: value	// ES6의 computed property name 구문을 사용.
        });
    }

    render() {
        return (
            <form>
                <label>
                    Is going:
                    <input
                        name="isGoing"
                        type="checkbox"
                        checked={this.state.isGoing}
                        onChange={this.handleInputChange} />
                </label>
                <br />
                <label>
                    Number of guests:
                    <input
                        name="numberOfGuests"
                        type="number"
                        value={this.state.numberOfGuests}
                        onChange={this.handleInputChange} />
                </label>
            </form>
        );
    }
}
```

| setState\(\)는 자동적으로 현재 state에 일부 state를 병합하기 때문에 바뀐 부분에 대해서만 호출하면 된다. |
| :--- |


#### 제어되는 Input Null 값

제어 컴포넌트에 value prop을 지정하면 의도치않는 한 사용자가 변경할 수 없다.

value를 설정했는데 여전히 수정할 수 있다면 실수로 value를 undefined나 null로 설정했을 수 있다.

```jsx
ReactDOM.render(<input value="hi" />, mountNode);


setTimeout(function() {
	ReactDOM.render(<input value={null} />, mountNode);
}, 1000);
```

#### 제어 컴포넌트의 대안

데이터를 변경할 수 있는 모든 방법에 대해 이벤트 핸들러를 작성하고 React 컴포넌트를 통해 모든 입력상태를 연결해야 하기 때문에 때로는 제어 컴포넌트를 사용하는게 지루할 수 있다.

특히 기존의 코드 베이스를 React로 변경하고자 할 때나, React가 아닌 라이브러리와 React application을 통합하고자 할 때 짜증날 수 있다.

이러한 경우 입력폼을 구현하기위한 대체기술인 비제어 컴포넌트를 확인할 수 있다.

#### 완전한 해결책

유효성 검사, 방문한 필드 추적 및 폼 제출 처리와 같은 완벽한 해결을 원한다면 Formik이 대중적인 선택중 하나다.

그러나 Formik은 제어 컴포넌트 및 state관리에 기초하기 때문에 배우는 걸 쉽게 생각해서는 안된다.

