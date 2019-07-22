# Vue.js :: 템플릿 문법

#### 01. Overview

Vue.js는 렌더링 된 DOM을 기본 Vue 인스턴스의 데이터에 선언적으로 바인딩 할 수 있는 HTML 기반 템플릿 구문을 사용한다.

모든 Vue.js 템플릿은 스펙을 호환하는 브라우저 및 HTML Parser로 구문분석할 수 있는 유효한 HTML이다.

내부적으로 Vue는 템플릿을 가상 DOM 렌더링 함수로 컴파일한다. 반응형 시스템과 결합된 Vue는 앱 상태가 변경될 때

최소한으로 DOM을 조작하고 다시 적용할 수 있는 최소한의 컴포넌트를 지능적으로 파악할 수 있다.

가상 DOM 개념에 익숙하고 javascript의 기본기능을 선호하는 경우 템플릿 대신 렌더링 함수를 직접 작성할 수 있으며, 선택사항으로 JSX를 지원한다.

#### 02. 보간법\(Interpolation\)

**문자열**

 데이터 바인딩의 가장 기본형태는 "Mustache"  구문\(이중 중괄호\)을 사용한 텍스트 보간이다.

```javascript
<span>메시지 : {{ message }}</span>
```

 Mustache 태그는 해당 데이터 객체의 message 속성 값으로 대체된다. 또한 데이터 객체의 message 속성이 변경될 때 마다 갱신된다.

 **v-once** directive를 사용하여 데이터 변경시 업데이트 되지 않는 일회성 보간을 수행할 수 있지만, 같은 노드의 바인딩에도 영향을 미친다는 점을 유의한다.

```javascript
<span v-once>다시는 변경하지 않는다. : {{ message }}</span>
```

**원시 HTML**

 이중 중괄호\(mustaches\)는 HTML이 아닌 일반 텍스트로 데이터를 해석한다. 실제 HTML을 출력하려면 **v-html** directive를 사용해야 한다.

```javascript
<p>Using mustaches : {{ rawHtml }}</p>
<p>Using v-html directive: <span v-html="rawHtml"></span></p>
```

 **v-html directive** 는 데이터 바인딩은 무시하고, 해당 속성값의 html 코드로 대체된다.

 Vue.js는 문자열 기반 템플릿 엔진이 아니기 때문에 v-html을 이용해 템플릿을 사용할 수 없다.

 이와 달리 컴포넌트 UI 재사용 및 구성을 위한 기본 단위로 사용하는 것을 추천한다.

<table>
  <thead>
    <tr>
      <th style="text-align:left">
        <p>&#xC6F9; &#xC0AC;&#xC774;&#xD2B8;&#xC5D0;&#xC11C; &#xC784;&#xC758;&#xC758;
          HTML&#xC744; &#xB3D9;&#xC801;&#xC73C;&#xB85C; &#xB80C;&#xB354;&#xB9C1;&#xD558;&#xBA74;
          XSS &#xCDE8;&#xC57D;&#xC810;&#xC73C;&#xB85C; &#xC27D;&#xAC8C; &#xC774;&#xC5B4;&#xC9C8;
          &#xC218; &#xC788;&#xB2E4;.</p>
        <p>&#xC2E0;&#xB8B0;&#xD560; &#xC218; &#xC788;&#xB294; &#xCF58;&#xD150;&#xCE20;&#xC5D0;&#xC11C;&#xB9CC;
          HTML &#xBCF4;&#xAC04;&#xC744; &#xC0AC;&#xC6A9;&#xD558;&#xACE0; &#xC0AC;&#xC6A9;&#xC790;&#xAC00;
          &#xC81C;&#xACF5;&#xD55C; &#xCF58;&#xD150;&#xCE20;&#xC5D0;&#xC11C;&#xB294;
          &#xC808;&#xB300; &#xC0AC;&#xC6A9;&#xD558;&#xBA74; &#xC548;&#xB41C;&#xB2E4;.</p>
      </th>
    </tr>
  </thead>
  <tbody></tbody>
</table>**속성**

 Mustaches는 HTML 속성에서 사용할 수 없다. 대신 **v-bind directive**를 사용한다.

```javascript
<div v-bind:id="dynamicId"></div>
```

 boolean 속성을 사용할 때 **단순히 true**인 경우 v-bind는 조금 다르게 동작한다.

```javascript
<button v-bind:disabled="isButtonDisabled">Button</button>
```

 isButtonDisabled가 null, undefined, false 값을 가지면 disabled 속성은 렌더링된 button element에 포함되지 않는다.

**Javascript 표현식 사용**

 Vue.js는 모든 데이터 바인딩 내에서 Javascript 표현식의 모든 기능을 제공한다.

```javascript
{{ number + 1 }}
{{ ok? 'YES' : 'NO' }}
{{ message.split('').reverse().join('') }}
<div v-bind:id="'list-'+id"></div>
```

 이 표현식은 Vue 인스턴스 데이터 범위내에서 Javascript로 계산된다.

 제한사항으로는 각 바인딩에 **하나의 단일 표현식**만 포함될 수 있다.

```javascript
{{ var a = 1 }} // 구문은 동작하지 않음. 표현식만 동작.
{{ if (ok) { return message } }} // 조건문은 동작하지 않음.
```

<table>
  <thead>
    <tr>
      <th style="text-align:left">
        <p>&#xD15C;&#xD50C;&#xB9BF; &#xD45C;&#xD604;&#xC2DD;&#xC740; &#xC0CC;&#xB4DC;&#xBC15;&#xC2A4;
          &#xCC98;&#xB9AC;&#xB41C;&#xB2E4;. Math&#xB098; Date&#xAC19;&#xC740; &#xC804;&#xC5ED;&#xC73C;&#xB85C;
          &#xC0AC;&#xC6A9;&#xAC00;&#xB2A5;&#xD55C; &#xAC83;&#xC5D0;&#xB9CC; &#xC811;&#xADFC;&#xC774;
          &#xAC00;&#xB2A5;&#xD558;&#xB2E4;.</p>
        <p>&#xD15C;&#xD50C;&#xB9BF; &#xD45C;&#xD604;&#xC2DD;&#xC5D0;&#xC11C; &#xC0AC;&#xC6A9;&#xC790;
          &#xC815;&#xC758; &#xC804;&#xC5ED;&#xC5D0; Access &#xD558;&#xBA74; &#xC548;&#xB41C;&#xB2E4;.</p>
      </th>
    </tr>
  </thead>
  <tbody></tbody>
</table>#### 03. Directive

**Definition**

 Directive는 v- 접두사가 있는 특수 속성이다.

 Directive의 속성값은 단일 Javascript 표현식이 된다. \(v-for는 예외\)

 Directive의 역할은 표현식의 값이 변경될 때 사이드이펙트를 반응적으로 DOM에 적용하는 것이다.

**Parameter**

 일부 Directive는 콜론으로 표시되는 Parameter를 사용할 수 있다. 예를 들어, v-bind directive는 반응적으로 HTML 속성을 갱신하는데 사용된다.

```javascript
<a v-bind:href="url">...</a>
```

 여기서 href는 전달인자로, Element의 href 속성을 표현식 url의 값에 bind 하는 v-bind directive에 알려준다.

 또 다른 예로 DOM 이벤트를 수신하는 v-on directive가 있다.

```javascript
<a v-on:click="doSomething"> ... </a>
```

 전달인자는 이벤트를 받을 이름이다. – method 명이 되지 않을까.

**수식어**

 수식어는 점으로 표시되는 특수 접미사로, directive를 특별한 방법으로 바인딩 해야함을 나타낸다.

 예를 들어, .prevent 수식어는 트리거된 이벤트에서 event.preventDefault\(\)를 호출하도록 v-on directive에게 알려준다.

```javascript
<form v-on:submit.prevent="onSubmit"> .... </form>
```

#### 04. 약어

**Background**

 v- 접두사는 템플릿의 Vue 특정속성을 식별하기 위한 시각적인 신호 역할을 한다. 

 이 기능은 Vue.js를 사용하여 기존 마크업에 동적인 동작을 적용할 때 유용하지만 일부 자주 사용되는 Directive에 대해 너무 장황하다고 느껴질 수 있다.

 동시에 Vue.js가 모든 템플릿을 관리하는 SPA\(Single Page Application\)을 만들 때 v- 접두어의 필요성이 떨어진다.

 따라서, 가장 **자주 사용되는 두개의 Directive인 v-bind와 v-on**에 대해 특별한 약어를 제공한다.

**v-bind**

```javascript
<a v-bind:href="url"> ... </a> //전체 문법
<a :href="url">...</a> // 약어사용
```

**v-on**

```javascript
<a v-on:click="doSomething">...</a> //전체 문법
<a @click="doSomething">...</a> // 약어 사용.
```

