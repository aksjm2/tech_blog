# 고급 :: 01. 접근성

#### 접근성이 필요한 이유

웹 접근성\(a11y\)은 모두가 사용할 수 있도록 웹사이트를 디자인, 개발하는 것을 의미한다.

보조과학기술\(assistive technology\)들이 웹페이지를 해석할 수 있도록 접근성을 갖추는 것이 필요하다.

#### 표준 및 지침

* WCAG\(Web Content Accessibility Guidelines\)

<table>
  <thead>
    <tr>
      <th style="text-align:left">
        <p>WCAG&#xB294; &#xC811;&#xADFC;&#xC131;&#xC744; &#xAC16;&#xCD98; &#xC6F9;&#xC0AC;&#xC774;&#xD2B8;&#xB97C;
          &#xB9CC;&#xB4DC;&#xB294;&#xB370; &#xD544;&#xC694;&#xD55C; &#xC9C0;&#xCE68;&#xC744;
          &#xC81C;&#xACF5;&#xD55C;&#xB2E4;.</p>
        <ul>
          <li><a href="https://www.wuhcag.com/wcag-checklist/">Wuhcag&#xC758; WCAG &#xCCB4;&#xD06C;&#xB9AC;&#xC2A4;&#xD2B8;</a>
          </li>
          <li><a href="https://webaim.org/standards/wcag/checklist">WebAIM&#xC758; WCAG &#xCCB4;&#xD06C;&#xB9AC;&#xC2A4;&#xD2B8;</a>
          </li>
          <li><a href="https://a11yproject.com/checklist.html">The A11Y Project&#xC758; &#xCCB4;&#xD06C;&#xB9AC;&#xC2A4;&#xD2B8;</a>
          </li>
        </ul>
        <p>WCAG &#xCCB4;&#xD06C;&#xB9AC;&#xC2A4;&#xD2B8;&#xB97C; &#xD1B5;&#xD574;
          &#xAC04;&#xB7B5;&#xD558;&#xAC8C; &#xC0B4;&#xD3B4;&#xBCF8;&#xB2E4;.</p>
      </th>
    </tr>
  </thead>
  <tbody></tbody>
</table>* WAI-ARIA\(Web Accessibility Initiative - Accessible Rich Internet Applications\)

<table>
  <thead>
    <tr>
      <th style="text-align:left">
        <p>WAI-ARIA &#xBB38;&#xC11C;&#xC5D0;&#xB294; &#xC811;&#xADFC;&#xC131;&#xC744;
          &#xAC16;&#xCD98; Javascript &#xC704;&#xC82F;&#xC744; &#xB9CC;&#xB4DC;&#xB294;&#xB370;
          &#xD544;&#xC694;&#xD55C; &#xAE30;&#xC220;&#xC774; &#xB2F4;&#xACA8;&#xC788;&#xB2E4;.</p>
        <p>&#xCC38;&#xACE0;&#xB85C;, JSX&#xC5D0;&#xC11C;&#xB294; &#xBAA8;&#xB4E0;
          aria-* HTML Attribute&#xB97C; &#xC9C0;&#xC6D0;&#xD55C;&#xB2E4;.</p>
        <p>React&#xC5D0;&#xC11C; &#xB300;&#xBD80;&#xBD84;&#xC758; DOM property&#xC640;
          Attribute&#xC5D0; &#xB300;&#xD55C; &#xAC12;&#xC774; Camel-case&#xB85C;
          &#xC9C0;&#xC6D0;&#xB418;&#xB294; &#xBC18;&#xBA74;, aria-*&#xC640; &#xAC19;&#xC740;
          attribute&#xB294; &#xC77C;&#xBC18;&#xC801;&#xC778; HTML&#xACFC; &#xB9C8;&#xCC2C;&#xAC00;&#xC9C0;&#xB85C;
          hypen-case(kebab-case, lisp-case &#xB4F1;)&#xB85C; &#xC791;&#xC131;&#xD574;&#xC57C;
          &#xD55C;&#xB2E4;.</p>
      </th>
    </tr>
  </thead>
  <tbody></tbody>
</table>#### Semantic HTML

semantic HTML은 웹 application에 있어 접근성의 기초이다.

정보의 의미가 강조되는 HTML element를 웹사이트에서 사용하면 자언스럽게 접근성이 갖추어지곤 한다.

가끔 React로 구성한코드가 돌아가게 만들기위해 &lt;div&gt;와 같은 element를 사용하여 HTML의 의미를 깨뜨리곤 한다.

특히 목록\(&lt;ol&gt;, &lt;ul&gt;, &lt;dl&gt;\)과 HTML &lt;table&gt;을 사용할때 문제가 두드러진다.

이 경우 ReactFragment를 사용하여 여러 element를 하나로 묶어주는 것을 권장한다.

```jsx
import React, { Fragment } from 'react';


function ListItem( { item } ) {
	return(
		<Fragment>
			<dt>{item.term}</dt>
			<dd>{item.description}</dd>
		<Fragment>
	);
}


function Glossary(props) {
	return (
		<dl>
			{props.items.map(item => (
				<ListItem item={item} key={item.id} />
			))}
		</dl>
	);
}
```

```jsx

function Glossary(props) {
	return (
		<dl>
			{props.items.map(item => (
				<Fragment key={item.id}> // 항목을 매핑할때 Fragment는 반드시 key property가 있어야 한다.
					<dt>{item.term}</dt>
					<dd>{item.description}</dd>
				</Fragment>
			))}
		</dl>
	);
}
```

Fragment 태그에 어떤 props도 필요하지 않고, 사용하는 도구에서 지원한다면 아래와 같이 짧게 줄여 쓸 수 있다.

```jsx
function ListItem({item}){
	return (
		<>
			<dt>{item.term}</dt>
			<dd>{item.description}</dd>
		</>
	);
}
```

#### 접근성 있는 폼

**라벨링**

&lt;input&gt;과 &lt;textarea&gt; 같은 모든 폼 컨트롤은 구분할 수 있는 라벨이 필요하다.

스크린 리더를 사용하는 사용자를 위해 자세한 설명이 담긴 라벨을 제공해야 한다.

<table>
  <thead>
    <tr>
      <th style="text-align:left">
        <ul>
          <li><a href="https://www.w3.org/WAI/tutorials/forms/labels/">W3C&#xC5D0;&#xC11C; &#xC81C;&#xACF5;&#xD558;&#xB294; &#xC5D8;&#xB9AC;&#xBA3C;&#xD2B8; &#xB77C;&#xBCA8;&#xB9C1; &#xBC29;&#xBC95;</a>
          </li>
          <li><a href="https://webaim.org/techniques/forms/controls">WebAIM&#xC5D0;&#xC11C; &#xC81C;&#xACF5;&#xD558;&#xB294; &#xC5D8;&#xB9AC;&#xBA3C;&#xD2B8; &#xB77C;&#xBCA8;&#xB9C1; &#xBC29;&#xBC95;</a>
          </li>
          <li><a href="https://www.paciellogroup.com/blog/2017/04/what-is-an-accessible-name/">The Paciello Group&#xC774; &#xC124;&#xBA85;&#xD55C; &#xC811;&#xADFC; &#xAC00;&#xB2A5;&#xD55C; &#xC774;&#xB984;&#xB4E4;</a>
          </li>
        </ul>
      </th>
    </tr>
  </thead>
  <tbody></tbody>
</table>표준 HTML에 대한 예시들이 React에 바로 사용될 수 있으나, for attribute 만 JSX에서 htmlFor로 사용하는 것에 주의해야한다.

**사용자에게 오류 안내.**

오류 상황은 모든 사용자가 알수 있어야 한다.

<table>
  <thead>
    <tr>
      <th style="text-align:left">
        <ul>
          <li><a href="https://www.w3.org/WAI/tutorials/forms/notifications/">The W3C demonstrates user notifications</a>
          </li>
          <li><a href="https://webaim.org/techniques/formvalidation/">WebAIM looks at form validation</a>
          </li>
        </ul>
      </th>
    </tr>
  </thead>
  <tbody></tbody>
</table>#### 포커스 컨트롤

**키보드 포커스와 포커스 윤곽선**

키보드 포커스는 현재 키보드 입력을 받아들일 수 있는 DOM 내의 element를 나타낸다.

다른 포커스 윤곽선으로 교체하고 싶은 경우. outline:0 과같은 윤곽선 제거 CSS를 사용하기도 한다.

**원하는 콘텐츠로 건너뛰기.**

Application은 사용자들의 키보드 탐색을 돕고 탐색 속도를 높일 수 있도록, 이전에 탐색한 영역을 건너 뛸 방법을 제공해야 한다.

Skiplinks 또는 Skip Navigation Link 들은 키보드 사용자가 페이지와 상호작용할 때만 표시되는 숨겨진 탐색 링크이다.

내부의 페이지 앵커와 약간의 스타일링으로 매우 쉽게 구현될 수 있다.

**프로그래밍적으로 포커스 관리하기**

React Application들은 런타임동안 지속해서 HTML DOM을 변경하기 때문에, 가끔 키보드 포커스를 잃거나 예상치 못한 Element에 포커스를 맞추곤 한다.

이를 수정하기 위해 프로그래밍적으로 키보드 포커스를 올바른 방향으로 변경해주어야 한다.

예를 들어, 모달이 닫힌 후에는 모달을 열었던 버튼으로 키보드 포커스를 다시 맞춰야 한다.

React에서 포커스를 지정하려면, DOM element에 ref를 사용할 수 있다. 그 후, 컴포넌트 내에서 필요할 때마다 포커스를 지정할 수 있다.

```jsx
class CustomTextInput extends React.Component {
    constructor(props) {
        super(props);
        this.textInput = React.createRef();
    }


	focus() {
		this.textInput.current.focus();
	}

    render() {
        <input
            type="text"
            ref={this.textInput}
        />
    }
}
```

가끔씩 부모 컴포넌트가 자식 컴포넌트 내의 element에 포커스를 잡아야 할 때가 있다.

이때, 자식 컴포넌트에 특별한 프로퍼티를 주어 DOM ref를 부모 컴포넌트로 노출하는 방식으로 부모의 ref를 자식의 DOM 노드에 넘겨줄 수 있다.

```jsx
function CustomTextInput(props) {
    return (
        <div>
            <input ref={props.inputRef} />
        </div>
    );
}

class Parent extends React.Component {
    constructor(props) {
        super(props);
        this.inputElement = React.createRef();
    }

    render() {
        return (
            <CustomTextInput inputRef={this.inputElement} />
        );
    }
}

// 필요시마다 포커스 가능.
this.inputElement.current.focus();
```

Higher Order Component를 사용하여 컴포넌트를 확장할 때는 감싸진 컴포넌트에 React에서 제공하는 forwardRef 함수를 사용하여 ref를 넘겨줄 수 있다.

서드파티 Higher Order Component에서 ref를 넘겨줄 수 없다면, 위와 같은 패턴을 여전히 차선책으로 사용할 수 있다.

