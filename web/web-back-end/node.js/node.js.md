---
description: Node.js에 대한 기술 소개.
---

# Node.js :: 소개

#### 01. Introduction

* 2009년 라이언 달\(Ryan Dahl\)이 개발.
* 서버 사이드 자바스크립트.
* 웹 어플리케이션을 제작할 수 있는 플랫폼
* V8 자바스크립트 엔진을 사용 \(C++로 개발\)
* Node는 단일 스레드 기반의 None-Blocking I/O

#### 02. Contents

V8로 인해 자바스크립트의 속도가 빨라지면서, 자바스크립트를 웹 브라우저가 아닌곳에서 쓸 수 있게 표준을 만들자는 의견이 많아졌고, CommonJS라는 프로젝트가 시작되었다.

CommonJS 표준 발표 이후 Ryan Dahl은 CommonJS 표준과 V8 자바스크립트 엔진을 기반으로 Node.js를 개발했다.

일반적인 소켓 통신 뿐만 아니라, Node.js 자체를 HTTP 웹서버로 실행 할 수 있고, WebSocket\(Socket.io\)등의 HTTP 기반 실시간 프로토콜도 손쉽게 사용할 수 있다.

실시간 통신을 자바스크립트 몇줄로 구현할 수 있게 된 것.

결과적으로 Node.js는 자바스크립트를 웹브라우저 속에서만 사용되던 언어에서 범용 스크립트 언어로 탈바꿈 시켰으며 Python이나, Perl, Ruby와 동일한 레벨이 되었다.

#### 03. Characteristic

**단일 Thread Non-blocking I/O**

지금까지 소프트웨어 프로그래밍 세계에서는 성능향상을 위해 끊임없는 연구를 해왔다. 그중 널리 쓰이게된 방식이 Multi-Thread 방식이다.

Multi-Thread 방식은 프로세스안에서 Thread를 여러개 만들어 로직을 동시에 처리하는 방식이다.

이 방식은 지금까지 널리 쓰이고 있지만 복잡한 동기화문제가 늘 골칫거리였다.

프로그래머들은 수 많은 동기화\(Synchronize\) 모델 또는 락\(Lock\)에 대해 학습해야 했고, 제대로 쓰기도 힘들었으며 생산성 저하로 이어졌다. → 비용증가.

Node.js는 Multi-Thread 모델 대신 단일 Thread 모델과 Non-blocking I/O를 채택했다.

* Blocking 방식 : 한줄이 끝나야 다음줄이 실행됨. → C언어 실행 방식을 생각하면됨. 순차실행, 먼저 실행되는 Command line이 다음 command line을 block 해서 Blocking 방식이라 불린다.
* Non-Blocking 방식 : 함수를 매개변수로 전달하는 부류가 Non-blocking 방식으로 동작하고, 함수 호출 후 리턴값을 받거나 그냥 함수만 호출하는 부류는 Blocking 방식으로 실행된다. 먼저 실행되는 명령이 함수 return값을 받거나 하면, 아래 줄이 먼저 실행된다.

