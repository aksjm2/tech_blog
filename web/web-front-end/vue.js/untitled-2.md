# Vue.js :: 인스턴스

#### 01. Vue 인스턴스 생성

모든 Vue 앱은 Vue 객체를 생성하는 것부터 시작한다.

엄격히 MVVM 패턴과 관련이 없지만 Vue의 디자인은 부분적으로 영향을 받았다. 컨벤션으로 Vue 인스턴스를 참조하기 위해 변수 vm\(ViewModel의 약자\)을 사용한다.

Vue 인스턴스를 인스턴스화 할 때, 데이터, 템플릿, 마운트할 엘리먼트, 메소드, 라이프사이클, 콜백 등의 옵션을 포함하는 options 객체를 전달해야한다.

전체 Option에 대한 목록은 [API Reference](https://kr.vuejs.org/v2/api) 에서 확인한다.

Vue Constructor는 미리 정의된 옵션으로 재사용 가능한 컴포넌트 Constructor를 생성하도록 확장될 수 있다.

```text
Root Instance
└─ TodoList
   ├─ TodoItem
   │  ├─ DeleteTodoButton
   │  └─ EditTodoButton
   └─ TodoListFooter
      ├─ ClearTodosButton
      └─ TodoListStatistics
```

Vue 앱은 new Vue를 통해 만들어진 root vue instance로 구성되며, 선택적으로 중첩이 가능하고 재사용 가능한 component tree로 구성된다.

#### 02. Attributes & Methods

```javascript

// data object
var data = { a : 1 }


// add to Vue Instance
var vm = new Vue({
	data: data
});


// reference example (They are same)
vm.a === data.a // => true


// attribute's change makes change of original data
vm.a = 2
data.a // 2
```

데이터가 변경되면 화면은 다시 렌더링 된다.

유념할 점은 data에 있는 속성들은 인스턴스가 생성될 때 존재한 것들만 반응형이라는 것이다.

새로운 속성을 추가하면 화면이 갱신되지 않는다.

```javascript
vm.b = 'hi'
```

따라서, 데이터를 반응형으로 표시해야 된다면 Vue Instance 생성시에 빈 값이나 null 등으로 초기화 해놓고 사용한다.

```javascript
data: {
	count : 0,
	text: '',
    error : null,
    isChanged : false,
    array_data : []
}
```

인스턴스 생성시에 등록되어있던 속성의 변경에 따른 렌더링을 막는 방법은 아래와 같다.

```javascript
Object.freeze(data)  // 기존 속성이 변경되는 것을 막아 반응형 시스템이 추적할 수 없다는것을 의미한다.
```

Vue instance의 속성은 다른 사용자 정의 속성과 구분하기위해 $를 붙여 사용한다.

```javascript
var data = { a : 1 }
var vm = new Vue({
	el: '#example',
	data: data
})


vm.$data === data // true
vm.$el === document.getElementById('example') // true


//$watch 는 인스턴스 메소드.
vm.$watch('a', function(newVal, oldVal){
	// vm.a가 변경되면 호출됨.
})
```

#### 03. Instance Life cycle hook

각 Vue 인스턴스는 생성될 때 일련의 초기화 단계를 거친다. 그 과정에서 사용자 저으이 로직을 실행할 수 있는 라이프사이클 훅도 호출된다.

e.g., created 훅은 인스턴스가 생성된 후에 호출된다.

```javascript

new Vue({
	data: {
		a : 1
	},
	created: function(){
		console.log('a is : '+this.a)
	}
})
```

인스턴스 라이프사이클의 여러 단계에서 호출될 다른 훅도 존재한다.

mounted, updated, destroyed 등이 있다.

모든 라이프사이클 훅은 this 를 통해 Vue 인스턴스를 가리키며 호출한다.

| options 속성이나 콜백에 `created: () => console.log(this.a)` 이나 `vm.$watch('a', newValue => this.myMethod())` 와 같은 [화살표 함수](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Functions/%EC%95%A0%EB%A1%9C%EC%9A%B0_%ED%8E%91%EC%85%98) 사용을 지양하기 바랍니다. 화살표 함수들은 부모 컨텍스트에 바인딩되기 때문에, `this` 컨텍스트가 호출하는 Vue 인스턴스에서 사용할 경우 `Uncaught TypeError: Cannot read property of undefined` 또는 `Uncaught TypeError: this.myMethod is not a function`와 같은 오류가 발생하게 됩니다. |
| :--- |


#### 04. Life cycle diagram

![The Vue Instance Lifecycle](https://kr.vuejs.org/images/lifecycle.png)

