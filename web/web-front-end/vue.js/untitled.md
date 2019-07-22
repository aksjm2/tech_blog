# Vue.js :: 기본 개념 정리



#### 01. Vue.js란?

Vue는 UI를 만들기위한 프레임워크이다. 다른 단일형 프레임워크와는 달리, Vue는 점진적으로 채택할 수 있도록 설계되었다.

핵심 라이브러리는 뷰 레이어에만 초점을 맞추어 다른 라이브러리나 기존 프로젝트와의 통합이 쉽다.

#### 02. 시작하기

* script import

```javascript
// cdn import
<script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script> // 개발자 버전 - 디버깅 가능.
<script src="https://cdn.jsdelivr.net/npm/vue"></script>             // 상용 버전 - 속도, 용량 최적화.


// node.js를 활용할 줄 안다면, vue-cli 시작 할 수 있음.
```

#### 03. 선언적 렌더링

Vue.js의 핵심은 간단한 템플릿 구문을 사용해 선언적으로 DOM에 데이터를 렌더링 할 수 있다는 것이다.

&gt; Text 데이터에 대한 Mapping 예 

```javascript
<div id="app">
  {{ message }}
</div>
```

```javascript
var app = new Vue({
	el: '#app',        // Vue 객체에서 사용하는 element를 선언. jQuery와 동일한 것같다. ID로 찾아감.
	data: {
		message : 'Hello World!'	// 실제 html element에 작성해 둔 template에 해당하는 data를 setting 하는 작업.
	}
});


// 이후 app.message를 다른 값으로 변환하면 자동으로 렌더링 된다.(데이터 반응형)
```

&gt; DOM Element Attribute에 대한 예

```javascript
<div id="app-2">
	<span v-bind:title="message">
		title 확인
	</span>
</div>
```

```javascript
var app2 = new Vue({
	el: '#app-2',
	data : {
		message: 'Page Load time :  '+new Date() +' !!!';
	}
});
```

<table>
  <thead>
    <tr>
      <th style="text-align:left">
        <p>Directive</p>
        <p>: Vue&#xC5D0;&#xC11C; &#xC81C;&#xACF5;&#xD558;&#xB294; &#xD2B9;&#xC218;
          &#xC18D;&#xC131;&#xC784;&#xC744; &#xB098;&#xD0C0;&#xB0B4;&#xB294; v- &#xC811;&#xB450;&#xC5B4;&#xAC00;
          &#xBD99;&#xC5B4;&#xC788;&#xC73C;&#xBA70;, &#xC0AC;&#xC6A9;&#xC790;&#xAC00;
          &#xC9D0;&#xC791;&#xD560; &#xC218; &#xC788;&#xB4EF; &#xB80C;&#xB354;&#xB9C1;
          &#xB41C; DOM&#xC5D0; &#xD2B9;&#xC218;&#xD55C; &#xBC18;&#xC751;&#xD615;
          &#xB3D9;&#xC791;&#xC744; &#xD55C;&#xB2E4;.</p>
        <p>v-bind&#xB294; element&#xC758; attribute(&#xC18D;&#xC131;)&#xC5D0; &#xB370;&#xC774;&#xD130;&#xB97C;
          &#xBC14;&#xC778;&#xB529;&#xD558;&#xACE0; &#xB80C;&#xB354;&#xB9C1; &#xD560;
          &#xC218; &#xC788;&#xB2E4;.</p>
        <p>v-if, v-for &#xB4F1; &#xC5EC;&#xB7EC;&#xAC00;&#xC9C0; &#xB514;&#xB809;&#xD2F0;&#xBE0C;&#xAC00;
          &#xC874;&#xC7AC;&#xD55C;&#xB2E4;.</p>
      </th>
    </tr>
  </thead>
  <tbody></tbody>
</table>&gt; 조건문과 반복문

```javascript
<div id="app-3">
	<p v-if="seen">now you can see me</p>
</div>
```

```javascript
var app3 = new Vue({
	el: '#app-3',
	data: {
		seen: true
	}
});
```

```javascript
<div id="app-4">
	<ol>
		<li v-for="todo in todo_list">
			{{ todo.text }}
		</li>
	</ol>
</div>
```

```javascript
var app4 = new Vue({
	el: '#app-4',
	data : {
		todo_list: [
			{text: 'Taks 1'},
			{text: 'Taks 2'},
			{text: 'Taks 3'}
		]
	}
});
```

app4.todo\_list.push\({text: 'new Item'}\) 등으로 동적 추가가능.

#### 04. 사용자 입력 핸들링

사용자가 앱과 상호작용을 할 수 있게 하기위해 v-on directive를 사용하여 vue 인스턴스에 메소드를 호출하는 이벤트 리스너를 등록할 수 있다.

* v-on : document.getElementById\('el'\).addEventListener\(\), $\('\#el'\).on\(\) 과 유사.

```javascript
<div id="app-5">
	<p>{{ message }}</p>
	<button v-on:click="reverseMessage">reverse</button>
</div>
```

```javascript
var app5 = new Vue({
	el: '#app-5',
	data: {
		message : 'TESTMESSAGE'
	},
	methods: {
		reverseMessage: function(){
			this.message = this.message.split('').reverse().join('')	//split으로 배열로 만든후, Array.reverse로 배열을 역순으로 처리, 다시 join을 통해 string으로 변환. 결과를 다시 app5.message에 넣으므로 자동 랜더링되어 표시됨.
		}
	}
});
```

* v-model : form의 input과 app의 상태를 양방향으로 바인딩 하는 기능이다.

```javascript
<div id="app-6">
	<p>{{ message }}</p>
	<input v-model="message">
</div>
```

```javascript
var app6 = new Vue({
	el: '#app-6',
	data: {
		message: 'Hello world!'
	}
});
```

