---
id: examples
title: 예제
description: '소개 > 예제: Redux 인터렉티브 예제'
---

# 예제

Redux [소스 코드](https://github.com/reduxjs/redux/tree/master/examples)는 몇 가지 예제와 함께 배포되고 있습니다. 대부분은 온라인에서 직접 만져볼 수 있는 에디터인 [CodeSandbox](https://codesandbox.io)에도 올라가 있습니다.

## 카운터 바닐라(Counter Vanilla)

다음 명령어로 [카운터 바닐라(Counter Vanilla)](https://github.com/reduxjs/redux/tree/master/examples/counter-vanilla) 예제를 실행할 수 있습니다.

```sh
git clone https://github.com/reduxjs/redux.git

cd redux/examples/counter-vanilla
open index.html
```

아니면 [sandbox](https://codesandbox.io/s/github/reduxjs/redux/tree/master/examples/counter-vanilla)에서 확인해보세요.

<iframe class="codesandbox"src="https://codesandbox.io/embed/github/reduxjs/redux/tree/master/examples/counter-vanilla/?codemirror=1&runonclick=1"sandbox="allow-modals allow-forms allow-popups allow-scripts allow-same-origin"></iframe>

이 예제는 빌드 시스템이나 뷰 프레임워크가 필요하지 않으며, ES5에서 사용되는 Redux API를 보여줍니다.

## 카운터(Counter)

다음 명령어로 [카운터(Counter)](https://github.com/reduxjs/redux/tree/master/examples/counter) 예제를 실행할 수 있습니다.

```sh
git clone https://github.com/reduxjs/redux.git

cd redux/examples/counter
npm install
npm start
```

아니면 [sandbox](https://codesandbox.io/s/github/reduxjs/redux/tree/master/examples/counter)에서 확인해보세요.

<iframe class="codesandbox"src="https://codesandbox.io/embed/github/reduxjs/redux/tree/master/examples/counter/?codemirror=1&runonclick=1"sandbox="allow-modals allow-forms allow-popups allow-scripts allow-same-origin"></iframe>

이 예제는 Redux를 React와 함께 사용하는 가장 기본적인 예입니다. 간단하게하기 위해 저장소가 변경 될 때 React 구성 요소를 수동으로 다시 렌더링합니다. 실제 프로젝트에서는 고성능 [React Redux](https://github.com/reactjs/react-redux)바인딩을 사용하는 것이 좋습니다.

이 예제에는 테스트가 포함되어 있습니다.

## Todos

다음 명령어로 [Todos](https://github.com/reactjs/redux/tree/master/examples/todos) 예제를 실행할 수 있습니다.

```sh
git clone https://github.com/reduxjs/redux.git

cd redux/examples/todos
npm install
npm start
```

아니면 [sandbox](https://codesandbox.io/s/github/reduxjs/redux/tree/master/examples/todos)에서 확인해보세요.

<iframe class="codesandbox"src="https://codesandbox.io/embed/github/reduxjs/redux/tree/master/examples/todos/?codemirror=1&runonclick=1"sandbox="allow-modals allow-forms allow-popups allow-scripts allow-same-origin"></iframe>

상태(state) 업데이트가 Redux의 구성 요소와 함께 작동하는 방법을 잘 이해할 수있는 가장 좋은 예제입니다. 이 예제는 리듀서가 취급하는 액션(action)을 다른 리듀서에 위임하는 방법과 [React Redux](https://github.com/reactjs/react-redux)를 사용하여 프리젠테이션 구성요소에서 컨테이너 구성요소를 생성하는 방법을 보여줍니다.

이 예제에는 테스트가 포함되어 있습니다.

## Todos with Undo

다음 명령어로 [Todos with Undo](https://github.com/reactjs/redux/tree/master/examples/todos-with-undo) 예제를 실행할 수 있습니다.

```sh
git clone https://github.com/reduxjs/redux.git

cd redux/examples/todos-with-undo
npm install
npm start
```

아니면 [sandbox](https://codesandbox.io/s/github/reduxjs/redux/tree/master/examples/todos-with-undo)에서 확인해보세요.

<iframe class="codesandbox"src="https://codesandbox.io/embed/github/reduxjs/redux/tree/master/examples/todos-with-undo/?codemirror=1&runonclick=1"sandbox="allow-modals allow-forms allow-popups allow-scripts allow-same-origin"></iframe>

이 예제는 이전 예제(todo)의 변형입니다. 거의 동일하지만, [Redux Undo](https://github.com/omnidan/redux-undo)로 리듀서를 감싸면 몇 줄의 코드로 앱에 실행취소/다시실행 기능을 추가 할 수 있습니다.

## Todos w/ Flow

Run the [Todos w/ Flow](https://github.com/reduxjs/redux/tree/master/examples/todos-flow) example:

```sh
git clone https://github.com/reduxjs/redux.git

cd redux/examples/todos-flow
npm install
npm start
```

Or check out the [sandbox](https://codesandbox.io/s/github/reduxjs/redux/tree/master/examples/todos-flow):

<iframe class="codesandbox"src="https://codesandbox.io/embed/github/reduxjs/redux/tree/master/examples/todos-flow/?codemirror=1&runonclick=1"sandbox="allow-modals allow-forms allow-popups allow-scripts allow-same-origin"></iframe>

This is like the previous Todos examples, but shows how to use Redux in conjunction with [Flow](https://flow.org/).

## TodoMVC

다음 명령어로 [TodoMVC](https://github.com/reduxjs/redux/tree/master/examples/todomvc) 예제를 실행할 수 있습니다.

```sh
git clone https://github.com/reduxjs/redux.git

cd redux/examples/todomvc
npm install
npm start
```

Or check out the [sandbox](https://codesandbox.io/s/github/reduxjs/redux/tree/master/examples/todomvc):

<iframe class="codesandbox"src="https://codesandbox.io/embed/github/reduxjs/redux/tree/master/examples/todomvc/?codemirror=1&runonclick=1"sandbox="allow-modals allow-forms allow-popups allow-scripts allow-same-origin"></iframe>

이것은 고전적인 [TodoMVC](http://todomvc.com/) 예제입니다. 비교를 위해 여기에 있지만, Todos 예제와 같은 것을 다룹니다.

이 예제는 테스트가 포함되어 있습니다.

## 쇼핑 카트(Shopping Cart)

다음 명령어로 [쇼핑 카트(Shopping Cart)](https://github.com/reactjs/redux/tree/master/examples/shopping-cart) 예제를 실행할 수 있습니다.

```sh
git clone https://github.com/reduxjs/redux.git

cd redux/examples/shopping-cart
npm install
npm start
```

Or check out the [sandbox](https://codesandbox.io/s/github/reduxjs/redux/tree/master/examples/shopping-cart):

<iframe class="codesandbox"src="https://codesandbox.io/embed/github/reduxjs/redux/tree/master/examples/shopping-cart/?codemirror=1&runonclick=1"sandbox="allow-modals allow-forms allow-popups allow-scripts allow-same-origin"></iframe>

이 예제는 앱이 커짐에 따라 중요한 특징적인 Redux 패턴을 보여줍니다. 특히 ID를 통해 정규화된 방법으로 엔티티(entities)를 저장하는 방법, 여러 레벨에서 리듀서를 작성하는 방법 및 리듀서와 함께 셀렉터를 정의하여 상태 모양에 대한 지식을 캡슐화하는 방법을 보여줍니다. 또한 [Redux Logger](https://github.com/fcomb/redux-logger)를 사용하여 로그를 남기고, [Redux Thunk](https://github.com/gaearon/redux-thunk) 미들웨어를 사용하여 조건부로 액션을 처리(dispatch)하는 방법을 보여줍니다. .

## Tree View

다음 명령어로 [Tree View](https://github.com/reactjs/redux/tree/master/examples/tree-view) 예제를 실행할 수 있습니다.

```sh
git clone https://github.com/reduxjs/redux.git

cd redux/examples/tree-view
npm install
npm start
```

Or check out the [sandbox](https://codesandbox.io/s/github/reduxjs/redux/tree/master/examples/tree-view):

<iframe class="codesandbox"src="https://codesandbox.io/embed/github/reduxjs/redux/tree/master/examples/tree-view/?codemirror=1&runonclick=1"sandbox="allow-modals allow-forms allow-popups allow-scripts allow-same-origin"></iframe>

이 예제는 깊이 중첩된 트리뷰를 렌더링하고 정규화된 형식으로 상태(state)를 나타내는 방법을 보여 줍니다. 이를 통해 리듀서에서 쉽게 업데이트 할 수 있습니다. 컨테이너 컴포넌트는 자신들이 렌더링하는 트리 노드에만 관여함으로써 우수한 렌더링 성능을 갖을 수 있습니다.

이 예제는 테스트가 포함되어 있습니다.

## 비동기(Async)

다음 명령어로 [비동기(Async)](https://github.com/rackt/redux/tree/master/examples/async) 예제를 실행할 수 있습니다.

```sh
git clone https://github.com/reduxjs/redux.git

cd redux/examples/async
npm install
npm start
```

Or check out the [sandbox](https://codesandbox.io/s/github/reduxjs/redux/tree/master/examples/async):

<iframe class="codesandbox"src="https://codesandbox.io/embed/github/reduxjs/redux/tree/master/examples/async/?codemirror=1&runonclick=1"sandbox="allow-modals allow-forms allow-popups allow-scripts allow-same-origin"></iframe>

이 예제는 비동기 API 읽기, 사용자 입력에 응답하여 데이터 가져오기, 로딩중 표시 보여주기, 응답을 캐싱하기 및 캐시 무효화하기를 포함하고 있습니다. 또한 [Redux Thunk](https://github.com/gaearon/redux-thunk) 미들웨어를 사용하여 비동기의 부수작용을 캡슐화합니다.

## 범용(Universal)

다음 명령어로 [Universal](https://github.com/reduxjs/redux/tree/master/examples/universal) 예제를 실행할 수 있습니다.

```sh
git clone https://github.com/reduxjs/redux.git

cd redux/examples/universal
npm install
npm start
```

이것은 Redux와 React를 사용한 [서버 렌더링](../usage/ServerRendering.md)의 기본 데모입니다. 클라이언트 저장소가 기존 상태에서 부팅할 수 있도록 서버에서 초기 저장소 상태를 준비하고 클라이언트에 전달하는 방법을 보여줍니다.

## 리얼 월드(Real World)

다음 명령어로 [리얼 월드(Read World)](https://github.com/reduxjs/redux/tree/master/examples/real-world) 예제를 실행할 수 있습니다.

```sh
git clone https://github.com/reduxjs/redux.git

cd redux/examples/real-world
npm install
npm start
```

Or check out the [sandbox](https://codesandbox.io/s/github/reduxjs/redux/tree/master/examples/real-world):

<iframe class="codesandbox" src="https://codesandbox.io/embed/github/reduxjs/redux/tree/master/examples/real-world/?codemirror=1&runonclick=1" sandbox="allow-modals allow-forms allow-popups allow-scripts allow-same-origin"></iframe>

이 예제는 최상급 예제입니다. 디자인도 반영되어 있습니다. 이제는 들어온 엔티티(entities)를 표준화 된 캐시에 유지하기, API 호출을 위한 사용자 정의 미들웨어 구현, 부분적으로 로드된 데이터 렌더링, 페이지네이션, 응답 캐싱, 에러메시지 표시 및 라우팅에 대해 다룹니다. 또한 Redux DevTools도 포함됩니다.

## 더 많은 예제

[Redux Addons Catalog](https://github.com/markerikson/redux-ecosystem-links)의 [Redux Apps and Examples](https://github.com/markerikson/redux-ecosystem-links/blob/master/apps-and-examples.md) 페이지에서 더 많은 예제를 찾아볼 수 있습니다.
