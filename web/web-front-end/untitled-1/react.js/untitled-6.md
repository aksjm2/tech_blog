---
description: 컴포넌트에서 관리하는 내부 변수인 State와 React의 Life Cycle을 정리한다.
---

# 주요개념 :: 04. State & Lifecycle

[주요 개념 :: 03. Components and Props](https://app.gitbook.com/@aksjm2/s/worklog/~/edit/drafts/-LkM3_0cXIM3r5sJiKid/web/untitled-1/react.js/untitled-7) 에서 작성했던 tick 부터 출발. → 전체 DOM을 재 rendering 하는 것이아니라 특정 component만 변경하는 것으로 처리할 것.

Clock 컴포넌트를 완전히 재사용 하고, 캡슐화 하는 방법을 진행한다.

```jsx
function Clock(props) {
	return (
		<div>
			<h1>Hello, World!</h1>
			<h2>It is {props.date.toLocaleTimeString()}.</h2>
		</div>
	);
}


function tick(){
	ReactDOM.render(
		<Clock date={new Date()} />,
		document.getElementById('root')
	)
}


setInterval(tick, 1000);
```

* 누락된 점 : Clock이 타이머를 설정하고, 매초 UI를 업데이트 해야한다. 이를 위해 컴포넌트에 "state"를 추가해야 한다.

state는 props와 유사하지만, 비공개\(private\)이며 컴포넌트에 의해 완전히 제어된다.

함수에서 클래스로 변환하기.\(5 step\)

* step 1 React.Component를 확장하는 동일한 이름의 ES6 Class를 생성한다.
* step 2 render\(\) 라고 불리는 빈 메서드를 추가한다.
* step 3 함수의 내용을 render\(\) 메서드 안으로 옮긴다.
* step 4 render\(\) 내용 안에 있는 props를 this.props로 변경한다.
* step 5 남아있는 빈 함수선언을 삭제한다.

```jsx
class Clock extends React.Component {
	render(){
		return (
			<div>
				<h1>Hello, world!</h1>
				<h2>It is {this.props.date.toLocaleTimeString()}.</h2>
			</div>
		);
	}
}
```



