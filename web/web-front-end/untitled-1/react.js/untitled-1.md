# 주요 개념 :: 09. Lifting State Up

종종 동일한 데이터에 대한 변경사항을 여러 컴포넌트에 반영해야 할 필요가 있다.

이럴 때 가장 가까운 공통 조상으로 state를 끌어올리는 것이 좋다.

주어진 온도에서 물의 끓는 여부를 추정하는 온도 계산기를 만들어 본다.

* BoilingVerdict : 섭씨온도를 의미하는 celsius prop를 받아 이 온도가 물이 끓기에 충분한지 여부를 출력한다.

```jsx
function BoilingVerdict(props) {
	if( props.celsius >= 100 ) {
		return <p>The water would boil.</p>
	}
	return <p>The water would not boil.</p>
}
```

* Calculator : 온도를 입력할 수 있는 input을 렌더링 하고 그 값을 this.state.temperature에 저장한다. 또한 현재 입력값에 대한 BoilingVerdict 컴포넌트를 렌더링한다.

```jsx
class Calculator extends React.Component {
    constructor(props){
        super(props);
        this.handleChange = this.handleChange.bind(this);
        this.state = {temperature: ''};
    }

    handleChange(e) {
        this.setState({temperature: e.target.value});
    }

    render() {
        const temperature = this.state.temperature;
        return (
            <fieldset>
                <legend>Enter temperature in Celsius:</legend>
                <input value={temperature} onChange={this.handleChange} />
                
                <BoilingVerdict celsius={parseFloat(temperature)}/>
            </fieldset>
        );
    }
}
```

#### Add Second Input.

화씨입력을 추가한다.

```jsx
const sclaeNames = {
    c: 'Celsius',
    f: 'Fahrenheit'
}

class TemperatureInput extends React.Component {
    constructor(props) {
        super(props);
        this.handleChange = this.handleChange.bind(this);
        this.state = {temperature: ''};
    }

    handleChange(e){
        this.setState({temperature: e.target.value});
    }

    render() {
        const temperature = this.state.temperature;
        const scale = this.props.scale;
        return (
            <fieldset>
                <legend>Enter temperature in {scaleNames[scale]}</legend>
                <input value={temperature} onChange={this.handleChange} />
            </fieldset>
        );
    }
}
```

```jsx

class Calculator extends React.component {
    render(){
        <div>
            <TemperatureInput scale="c" />
            <TemperatureInput scale="f" />
        </div>
    }
}
```

두개의 입력필드를 갖게 되었다. 하지만 둘중 하나의 온도를 입력하더라도 다른 하나가 변경되지 않는 문제가 있다.

두 입력 필드간 동기화를 유지하고자 한다.

#### 변환 함수 작성

```jsx
function toCelsius(fahrenheit) {
	return (fahrenheit - 32) * 5 / 9;
}
function toFahrenheit(celsius) {
	return (celsius * 9 /5) + 32
}
```

섭씨를 화씨로, 화씨를 섭씨로 변경하는 함수 작성.

temperature 문자열과 변환함수를 인수로 받아, 문자열을 반환하는 추가 함수 작성.

```jsx
function tryConvert(temperature, convert) {
	const input = parseFloat(temperature);
	if (Number.isNaN(input)) {
		return '';
	}
	const output = convert(input);
	const rounded = Math.round(output * 1000) / 1000;
	return rounded.toString();
}
```

#### State 끌어올리기

두 TemperatureInput 컴포넌트가 각각의 입력값을 자신의 state에 독립적으로 저장하고 있다.

Calculator가 공통의 조상이므로, 두 TemperatureInput 컴포넌트는 calculator로 부터 props로 값을 표시하도록한다.

event handling 에서는 마찬가지로 props로 넘겨받은 조상의 함수를 호출하게 된다.

```jsx
class TemperatureInput extends React.Component {
    constructor(props) {
        super(props);
        this.handleChange = this.handleChange.bind(this);
    }

    handleChange(e){
        this.props.onTemperatureChange(e.target.value);
    }

    render(){
        const temperature = this.props.temperature;
        const scale = this.props.scale;

        return (
            <fieldset>
                <legend>Enter temperature in {scaleNames[scale]}:</legend>
                <input value={temperature} onChange={this.handleChange} />
            </fieldset>
        );
    }
}
```

```jsx
class Calculator extends React.Component {
    constructor(props){
        super(props);
        this.handleCelsiusChange = this.handleCelsiusChange.bind(this);
        this.handleFahrenheitChange = this.handleFahrenheitChange.bind(this);
        this.state = {temperature: '', scale: 'c'};
    }

    handleCelsiusChange(temperature) {
        this.setState({scale: 'c', temperature});
    }

    handleFahrenheitChange(temperature) {
        this.setState({scale: 'f', temperature});
    }

    render(){
        const scale = this.state.scale;
        const temperature = this.state.temperature;
        const celsius = scale === 'f' ? tryConvert(temperature, toCelsius) : temperature;
        const fahrenheit = scale === 'c' ? tryConvert(temperature, toFahrenheit) : temperature;

        return (
            <div>
                <TemperatureInput
                    scale="c"
                    temperature={celsius}
                    onTemperatureChange={this.handleCelsiusChange} />
                <TemperatureInput
                    scale="f"
                    temperature={fahrenheit}
                    onTemperatureChange={this.handleFahrenheitChange} />
                <BoilingVerdict celsius={parseFloat(celsius)} />
            </div>
        );
    }
}
```

어떤 값을 수정하던 간에, Calculator의 this.state.temperature와 this.state.scale이 갱신된다.

입력 필드 중 하나는 있는 그대로의 값을 받으므로, 사용자가 입력한 값이 보존되고, 다른 입력필드의 값은 항상 다른 하나에 기반해 재 계산된다.

<table>
  <thead>
    <tr>
      <th style="text-align:left">
        <ul>
          <li>React&#xB294; DOM &lt;input&gt;&#xC758; onChange&#xC5D0; &#xC9C0;&#xC815;&#xB41C;
            &#xD568;&#xC218;&#xB97C; &#xD638;&#xCD9C;&#xD55C;&#xB2E4;. &#xC704; &#xC608;&#xC2DC;&#xC758;
            &#xACBD;&#xC6B0;, TemperatureInput&#xC758; handleChange&#xC5D0; &#xD574;&#xB2F9;.</li>
          <li>TemperatureInput &#xCEF4;&#xD3EC;&#xB10C;&#xD2B8;&#xC758; handleChange
            &#xBA54;&#xC11C;&#xB4DC;&#xB294; &#xC0C8;&#xB85C; &#xC785;&#xB825;&#xB41C;
            &#xAC12;&#xACFC; &#xD568;&#xAED8;, this.props.onTemperatureChange()&#xB97C;
            &#xD638;&#xCD9C;&#xD55C;&#xB2E4;. onTemperatureChange&#xB97C; &#xD3EC;&#xD568;&#xD55C;
            &#xC774; &#xCEF4;&#xD3EC;&#xB10C;&#xD2B8;&#xC758; props&#xB294; &#xBD80;&#xBAA8;
            &#xCEF4;&#xD3EC;&#xB10C;&#xD2B8;&#xC778; Calculator&#xB85C;&#xBD80;&#xD130;
            &#xC81C;&#xACF5;&#xBC1B;&#xC740; &#xAC83;&#xC774;&#xB2E4;.</li>
          <li>&#xC774;&#xC804; &#xB80C;&#xB354;&#xB9C1; &#xB2E8;&#xACC4;&#xC5D0;&#xC11C;,
            Calculator&#xB294; &#xC12D;&#xC528; TemperatureInput&#xC758; onTemperatureChange&#xB97C;
            Calculator&#xC758; handleCelsiusChange&#xC758; &#xBA54;&#xC11C;&#xB4DC;&#xB85C;,
            &#xD654;&#xC528; TemperatureInput&#xC758; onTemperatureChange&#xB97C; Calculator&#xC758;
            handleFahrenheitChange &#xBA54;&#xC11C;&#xB4DC;&#xB85C; &#xC9C0;&#xC815;&#xD574;
            &#xB450;&#xC5C8;&#xB2E4;. &#xB530;&#xB77C;&#xC11C; &#xC6B0;&#xB9AC;&#xAC00;
            &#xB458; &#xC911;&#xC5D0; &#xC5B4;&#xB5A4; &#xC785;&#xB825;&#xD544;&#xB4DC;&#xB97C;
            &#xC218;&#xC815;&#xD558;&#xB290;&#xB0D0;&#xC5D0; &#xB530;&#xB77C;&#xC11C;
            Calculator&#xC758; &#xB450; &#xBA54;&#xC11C;&#xB4DC; &#xC911; &#xD558;&#xB098;&#xAC00;
            &#xD638;&#xCD9C;&#xB41C;&#xB2E4;.</li>
          <li>&#xC774;&#xB4E4; &#xBA54;&#xC11C;&#xB4DC;&#xB294; &#xB0B4;&#xBD80;&#xC801;&#xC73C;&#xB85C;
            Calculator &#xCEF4;&#xD3EC;&#xB10C;&#xD2B8;&#xAC00; &#xC0C8; &#xC785;&#xB825;&#xAC12;,
            &#xADF8;&#xB9AC;&#xACE0; &#xD604;&#xC7AC; &#xC218;&#xC815;&#xD55C; &#xC785;&#xB825;
            &#xD544;&#xB4DC;&#xC758; &#xC785;&#xB825;&#xB2E8;&#xC704;&#xC640; &#xD568;&#xAED8;
            this.setState()&#xB97C; &#xD638;&#xCD9C;&#xD558;&#xAC8C; &#xD568;&#xC73C;&#xB85C;&#xC368;
            React&#xC5D0;&#xAC8C; &#xC790;&#xC2E0;&#xC744; &#xB2E4;&#xC2DC; &#xB80C;&#xB354;&#xB9C1;&#xD558;&#xB3C4;&#xB85D;
            &#xC694;&#xCCAD;&#xD55C;&#xB2E4;.</li>
          <li>React&#xB294; UI&#xAC00; &#xC5B4;&#xB5BB;&#xAC8C; &#xBCF4;&#xC5EC;&#xC57C;
            &#xD558;&#xB294;&#xC9C0; &#xC54C;&#xC544;&#xB0B4;&#xAE30; &#xC704;&#xD574;
            Calculator &#xCEF4;&#xD3EC;&#xB10C;&#xD2B8;&#xC758; render &#xBA54;&#xC11C;&#xB4DC;&#xB97C;
            &#xD638;&#xCD9C;&#xD55C;&#xB2E4;. &#xB450; &#xC785;&#xB825; &#xD544;&#xB4DC;&#xC758;
            &#xAC12;&#xC740; &#xD604;&#xC7AC; &#xC628;&#xB3C4;&#xC640; &#xD65C;&#xC131;&#xD654;&#xB41C;
            &#xB2E8;&#xC704;&#xB97C; &#xAE30;&#xBC18;&#xC73C;&#xB85C; &#xC7AC; &#xACC4;&#xC0B0;&#xB41C;&#xB2E4;.
            &#xC628;&#xB3C4;&#xC758; &#xBCC0;&#xD638;&#xB098;&#xC774; &#xC774; &#xB2E8;&#xACC4;&#xC5D0;&#xC11C;
            &#xC218;&#xD589;&#xB41C;&#xB2E4;.</li>
          <li>React&#xB294; Calculator&#xAC00; &#xC804;&#xB2EC;&#xD55C; &#xC0C8; props&#xC640;
            &#xD568;&#xAED8; &#xAC01; TemperatureInput &#xCEF4;&#xD3EC;&#xB10C;&#xD2B8;&#xC758;
            render &#xBA54;&#xC11C;&#xB4DC;&#xB97C; &#xD638;&#xCD9C;&#xD55C;&#xB2E4;.
            &#xADF8;&#xB7EC;&#xBA74;&#xC11C; UI&#xAC00; &#xC5B4;&#xB5BB;&#xAC8C; &#xBCF4;&#xC5EC;&#xC57C;&#xD560;&#xC9C0;&#xB97C;
            &#xD30C;&#xD55C;&#xB2E4;.</li>
          <li>React&#xB294; BoilingVerdict &#xCEF4;&#xD3EC;&#xB10C;&#xD2B8;&#xC5D0;&#xAC8C;
            &#xC12D;&#xC528;&#xC628;&#xB3C4;&#xB97C; props&#xB85C; &#xC804;&#xB2EC;&#xD558;&#xBA70;,
            &#xADF8; &#xCEF4;&#xD3EC;&#xB10C;&#xD2B8;&#xC758; render &#xBA54;&#xC11C;&#xB4DC;&#xB97C;
            &#xD638;&#xCD9C;&#xD55C;&#xB2E4;.</li>
          <li>React DOM&#xC740; &#xBB3C;&#xC758; &#xB053;&#xB294; &#xC5EC;&#xBD80;&#xC640;
            &#xC62C;&#xBC14;&#xB978; &#xC785;&#xB825;&#xAC12;&#xC744; &#xC77C;&#xCE58;&#xC2DC;&#xD0A4;&#xB294;
            &#xC791;&#xC5C5;&#xACFC; &#xD568;&#xAED8; DOM&#xC744; &#xAC31;&#xC2E0;&#xD55C;&#xB2E4;.
            &#xAC12;&#xC744; &#xBCC0;&#xACBD;&#xD55C; &#xC785;&#xB825; &#xD544;&#xB4DC;&#xB294;
            &#xD604;&#xC7AC; &#xC785;&#xB825;&#xAC12;&#xC744; &#xADF8;&#xB300;&#xB85C;
            &#xBC1B;&#xACE0;, &#xB2E4;&#xB978; &#xC785;&#xB825; &#xD544;&#xB4DC;&#xB294;
            &#xBCC0;&#xD658;&#xB41C; &#xC628;&#xB3C4; &#xAC12;&#xC744; &#xAC31;&#xC2E0;&#xB41C;&#xB2E4;.</li>
        </ul>
      </th>
    </tr>
  </thead>
  <tbody></tbody>
</table>