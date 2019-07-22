# Vue.js :: 컴포넌트

#### 01. 컴포넌트를 사용한 작성방법

컴포넌트는 Vue의 또 다른 중요한 개념이다.

컴포넌트는 그 자체로 제기능을 하며, 기능으로 구분지은 최소단위라고 생각할 수 있겠다.

front-end 화면을 구성할 때 CBD\(component based development\)로 만든다고 생각하면 된다.

Vue에서, 컴포넌트는 본질적으로 미리 정의된 옵션을 가진 Vue 인스턴스\(객체\)다.

&gt;&gt; 컴포넌트 등록

```javascript
Vue.component('todo-item', {
	template: '<li>Task 1</li>'
});
```

&gt;&gt; 다른 컴포넌트의 템플릿에서 불러와 사용가능하다.

```javascript
<ol>
	<todo-item></todo-item>
</ol>
```

=== 모두 똑같은 내용이 렌더링 될 것이다. ===

&gt;&gt; 부모 컴포넌트의 데이터를 주입할 수 있는 방법.

```javascript
<div id="app-7">
	<ol>
		<todo-item
			v-for="item in groceryList"
			v-bind:todo="item"	// todo에 groceryList의 item을 하나씩 바인딩한다.
			v-bind:key="item.id"> // 각 바인딩되는 구성요소에는 key가 필요하다.
		</todo-item>
	</ol>
</div>
```

```javascript

Vue.component('todo-item', {
	props: ['todo'], // todo-item 컴포넌트는 props라는 사용자 정의 속성을 입력받을 수 있다. 이름은 todo로 정의됨.
	template: '<li>{{ todo.text }}</li>'
});


var app7 = new Vue({
	el: '#app-7',
	data: {
		groceryList: [
			{ id: 0, text: 'TASK 1' },
			{ id: 1, text: 'TASK 2' },
			{ id: 2, text: 'TASK 3' }
		]
	}
});

```

&gt;&gt; 컴포넌트 사용예시

```javascript

<div id="app"> 					// 하나의 Vue 객체에
  <app-nav></app-nav> 			// app-nav라는 component :: 상단 메뉴 바
  <app-view>					// app-view 라는 component :: 좌측 메뉴와 Content가 포함될 컴포넌트
    <app-sidebar></app-sidebar>	// app-sidebar 라는 component :: 좌측 메뉴
    <app-content></app-content>	// app-content 라는 component :: Content
  </app-view>
</div>

```

전반적으로 화면 구성을 쪼개서 관리할 수 있다는 점과, 화면 구성이 달라지는 메뉴의 경우에도 컴포넌트를 추가하는 방식으로 작성하면

추후에는 컴포넌트의 조합으로 화면을 개발할 수 있다는 큰 장점이 있겠다.

