---
id: createstore
title: createStore
hide_title: true
description: 'API > createStore: creating a core Redux store'
---

&nbsp;

# `createStore(reducer, [preloadedState], [enhancer])`

앱의 상태 트리 전체를 보관하는 Redux [저장소](Store.md)를 만듭니다.
앱 내에는 단 하나의 저장소만 있어야 합니다.

#### 인수

1. `reducer` _(Function)_: 주어진 현재 상태 트리와 [액션](../understanding/thinking-in-redux/Glossary.md#액션)에서 다음 [상태 트리](../understanding/thinking-in-redux/Glossary.md#상태)를 반환하는 [리듀싱 함수](../understanding/thinking-in-redux/Glossary.md#리듀서)입니다.

2. [`preloadedState`] _(any)_: 초기 상태입니다. 유니버설 앱의 서버나 이전의 직렬화된 사용자 세션에서 상태를 채우기 위해 선택적으로 지정할 수 있습니다. 만약 [`combineReducers`](combineReducers.md)로 리듀서를 만들었다면, 이 인수는 전달했던 것과 같은 키 구조를 가지는 평범한 객체여야 합니다. 그렇지 않다면 리듀서가 이해할 수 있는 어떤 것도 사용할 수 있습니다.

3. [`enhancer`] _(Function)_: 저장소 인핸서입니다. 미들웨어나 시간여행, 영속성 등의 서드파티 기능을 저장소에 추가하기 위해 지정할 수 있습니다. Redux와 함께 제공되는 인핸서는 [`applyMiddleware()`](./applyMiddleware.md) 뿐입니다.

#### 반환

([_`Store`_](Store.md)): 앱의 전체 상태를 가지고 있는 객체입니다. 이 객체의 상태를 바꾸는 유일한 방법은 [액션을 보내는](Store.md#dispatch)것 뿐입니다. UI를 업데이트 하기 위해 상태를 [구독](Store.md#subscribe) 할 수도 있습니다.

#### 예제

```js
import { createStore } from 'redux'

function todos(state = [], action) {
  switch (action.type) {
    case 'ADD_TODO':
      return state.concat([action.text])
    default:
      return state
  }
}

const store = createStore(todos, ['Use Redux'])

store.dispatch({
  type: 'ADD_TODO',
  text: 'Read the docs'
})

console.log(store.getState())
// [ 'Use Redux', 'Read the docs' ]
```

#### 팁

- 앱에 하나 이외의 저장소를 만들지 마세요! 대신 여러개의 리듀서를 하나의 루트 리듀서로 만들기 위해 [`combineReducers`](combineReducers.md)를 사용하세요.

- 상태의 형식은 당신이 결정하기 나름입니다. 평범한 객체를 사용하거나 [Immutable](http://facebook.github.io/immutable-js/) 같은 것을 사용할 수도 있습니다. 잘 모르겠다면, 평범한 객체로 시작하세요.

- 상태가 평범한 객체라면, 절대 변경하지 않도록 하세요! 예를 들어 리듀서에서 `Object.assign(state, newData)` 같은 것을 반환하는 대신 `Object.assign({}, state, newData)`를 반환하세요. 이를 통해 이전 상태를 덮어쓰지 않을 수 있습니다. [객체 확산 연산자 제안](../usage/UsingObjectSpreadOperator.md)을 사용한다면 `return { ...state, ...newData }`처럼 쓸 수도 있습니다.

- 서버에서 실행되는 유니버셜 앱을 위해서는 매 요청마다 별개의 저장소 인스턴스를 만드세요. 저장소 인스턴스에 데이터를 가져오는 액션을 보낸 다음 완료되면 서버에서 앱을 렌더링하면 됩니다.

- 저장소가 생성되면, Redux는 최초의 상태를 가지는 저장소를 만들기 위해 더미 액션을 보냅니다. 이 액션을 직접 다뤄야 하는 것은 아닙니다. 단지 여러분의 리듀서가 `undefined`를 인수로 받았을 때 최초 상태를 반환해야 한다는 점만 기억해두면 됩니다.

- 여러개의 저장소 인핸서를 적용하기 위해서는 [`compose()`](./compose.md)를 사용해야 합니다.
