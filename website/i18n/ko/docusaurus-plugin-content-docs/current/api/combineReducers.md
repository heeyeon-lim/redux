---
id: combinereducers
title: combineReducers
hide_title: true
description: 'API > combineReducers: merging slice reducers to create combined state'
---

&nbsp;

# `combineReducers(reducers)`

앱이 점점 복잡해지면 [리듀싱 함수](../understanding/thinking-in-redux/Glossary.md#리듀서)를 [상태](../understanding/thinking-in-redux/Glossary.md#상태)의 독립된 부분들을 관리하는 함수들로 분리하고 싶어질겁니다.

`combineReducers` 헬퍼 함수는 서로 다른 리듀싱 함수들을 값으로 가지는 객체를 받아서 [`createStore`](createStore.md)에 넘길 수 있는 하나의 리듀싱 함수로 바꿔줍니다.

생성된 리듀서는 내부의 모든 리듀서들을 호출하고 결과를 모아서 하나의 상태 객체로 바꿔줍니다. **상태 객체의 형태는 `reducers`로 전달된 키들을 따릅니다**.

결과적으로 상태 객체는 이런 형태가 됩니다:

```
{
  reducer1: ...
  reducer2: ...
}
```

상태의 키 이름은 전달되는 객체의 키 이름을 다르게 함으로써 제어할 수 있습니다. 예를 들어, 상태의 모양이 `{ todos, counter }`가 되게하기 위해서는 `combineReducers({ todos: myTodosReducer, counter: myCounterReducer })`이라고 호출하면 됩니다.

관행적으로는 관리하는 상태 조각의 이름을 따라 리듀서를 명명하는 것입니다. 이렇게 하면 ES6의 속성 단축 표기를 사용할 수 있습니다: `combineReducers({ counter, todos })`. 이는 `combineReducers({ counter: counter, todos: todos })`라고 쓰는 것과 동일합니다.

> ##### Flux 사용자를 위한 한마디

> 이 함수는 Flux가 서로 다른 상태를 관리하기 위해 서로 다른 저장소를 두는 것 처럼 리듀서가 각자의 상태 조각들을 관리하게 도와줍니다. Redux에는 하나의 저장소 뿐이지만, `combineReducers`를 통해 리듀서들을 동일하게 논리적으로 분리할 수 있습니다.

#### 인수

1. `reducers` (_Object_): 하나로 합쳐질 각각의 리듀싱 함수들을 값으로 가지는 객체입니다. 각 함수들이 따라야 하는 규칙은 아래의 주석을 참고하세요.

> 예전의 문서는 리듀서 객체를 얻어오기 위해 ES6의 `import * as reducers` 문법을 쓰도록 제안했습니다. 이 방식이 많은 혼란을 가져왔기 때문에 이제는 `reducers/index.js`에서 `combineReducers()`를 통해 하나의 리듀서를 익스포트할 것을 권장합니다. 예제가 아래에 있습니다.

#### 반환

(_Function_): `reducers` 객체 안의 모든 리듀서들을 실행해서 하나의 상태 객체를 만드는 리듀서입니다.

#### 주석

이 함수는 초보자들이 쉽게 빠지는 실수를 방지하기 위해 약간은 완고하고 편향되어 있습니다. 여러분이 직접 루트 리듀서를 작성할 때는 따를 필요가 없었던 규칙들을 강요하는 것은 이 때문입니다.

`combineReducers`로 전달되는 모든 리듀서는 아래의 규칙을 따라야만 합니다:

- 식별되지 않은 모든 상태에 대해서는 첫 인수로 주어진 `state`를 그대로 반환해야 합니다.

- `undefined`를 반환해서는 안 됩니다. 이른 `return`문에서 쉽게 할 수 있는 실수로, 다른 곳에서 에러가 나기 전에 `combineReducers`에서 에러를 발생(throw)시킵니다.

- `state`가 `undefined`로 주어지면 반드시 해당 리듀서의 초기 상태를 반환해야 합니다. 예전 룰을 따르면 초기 상태 또한 `undefined`가 될 수 없습니다. ES6의 선택적 인수(optional arguments) 문법을 사용하면 간편하지만, 직접 첫 인수가 `undefined`가 아닌지 확인할 수도 있습니다.

`combineReducers`가 이들 규칙을 지키는지 확인하지만, 여러분이 기억하고 지키는 것도 중요합니다. `combineReducers`는 여러분이 `Redux.createStore(combineReducers(...), initialState)`를 사용해서 초기 상태를 명시하더라도, 리듀서에 `undefined`를 전달해서 직접 확인합니다. 그러므로 여러분이 `undefined`를 받을 의도 없이 코드를 작성했더라도, **반드시** 리듀서가 `undefined`를 상태로 받더라도 제대로 작동하는지 확인하세요.

#### 예제

#### `reducers/todos.js`

```js
export default function todos(state = [], action) {
  switch (action.type) {
    case 'ADD_TODO':
      return state.concat([action.text])
    default:
      return state
  }
}
```

#### `reducers/counter.js`

```js
export default function counter(state = 0, action) {
  switch (action.type) {
    case 'INCREMENT':
      return state + 1
    case 'DECREMENT':
      return state - 1
    default:
      return state
  }
}
```

#### `reducers/index.js`

```js
import { combineReducers } from 'redux'
import todos from './todos'
import counter from './counter'

export default combineReducers({
  todos,
  counter
})
```

#### `App.js`

```js
import { createStore } from 'redux'
import reducer from './reducers/index'

const store = createStore(reducer)
console.log(store.getState())
// {
//   counter: 0,
//   todos: []
// }

store.dispatch({
  type: 'ADD_TODO',
  text: 'Use Redux'
})
console.log(store.getState())
// {
//   counter: 0,
//   todos: [ 'Use Redux' ]
// }
```

#### 팁

- 이 헬퍼는 편의를 위한 것일 뿐입니다! [다르게 작동하는](https://github.com/acdlite/reduce-reducers) 여러분만의 `combineReducers`를 직접 작성할 수도 있고, 자식 리듀서들에서 상태 객체를 조합하는 루트 리듀싱 함수를 명시적으로 만들 수도 있습니다.

- `combineReducers`는 리듀서 계층의 최상위 뿐 아니라 어느 단계에서도 호출할 수 있습니다. 자식 리듀서가 너무 복잡해지면 다시 사용해서 손자 리듀서를 만들고 이런 식으로 반복할 수도 있습니다.
