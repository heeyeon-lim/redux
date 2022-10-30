---
id: applymiddleware
title: applyMiddleware
hide_title: true
description: 'API > applyMiddleware: extending the Redux store'
---

&nbsp;

# `applyMiddleware(...middleware)`

Redux에 임의의 기능을 넣어 확장하는 방법으로는 미들웨어를 추천합니다. 미들웨어는 저장소의 [`dispatch`](Store.md#dispatch) 메서드를 재미삼아서나 실용적으로 감쌀 수 있게 해줍니다. 미들웨어의 중요한 기능 중 하나는 조합 가능하다는 점입니다. 여러 개의 미들웨어가 조합될 수 있고, 각각의 미들웨어는 체인 내에서 자기 앞이나 뒤의 미들웨어에 대해 아무것도 몰라도 됩니다.

가장 일반적인 미들웨어의 사용법은 많은 보일러플레이트나 [Rx](https://github.com/Reactive-Extensions/RxJS)와 같은 라이브러리에 대한 의존성 없이도 비동기 액션을 지원하는 것입니다. 이는 [비동기 액션](../understanding/thinking-in-redux/Glossary.md#비동기-액션)을 보통의 액션처럼 보내게 해줌으로써 이루어집니다.

예를 들어, [redux-thunk](https://github.com/gaearon/redux-thunk)는 액션 생산자가 디스패치 함수를 통해 제어를 역전할 수 있게 합니다. 액션 생산자는 [`dispatch`](Store.md#dispatch)를 인수로 받아 비동기적으로 호출할 수 있습니다. 이런 함수들은 _thunk_ 라고 불립니다. 다른 예로는 [redux-promise](https://github.com/acdlite/redux-promise)가 있습니다. 이 미들웨어는 [Promise](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Global_Objects/Promise) 비동기 액션을 보내고, 이 Promise가 결정되었을 때 보통의 액션을 보내게 해 줍니다.

미들웨어는 [`createStore`](createStore.md)에 포함되어 있지 않으며 Redux 아키텍쳐의 기반이 되는 부분은 아니지만, 코어 내에서 지원해야 할 정도로 유용하다고 생각했습니다. 이를 통해 [`dispatch`](Store.md#dispatch)를 생태계 내에서 확장하는 하나의 표준화된 방법을 제공하고, 서로 다른 미들웨어들이 표현력과 유용성을 통해 경쟁할 수 있게 했습니다.

#### 인수

- `...middleware` (_arguments_): *미들웨어 API*를 따르는 함수입니다. 각각의 미들웨어는 [`Store`](Store.md)의 [`dispatch`](Store.md#dispatch)와 [`getState`](Store.md#getState) 함수를 명명된 인수로 받아서, 함수를 반환합니다. 이 함수는 미들웨어의 디스패치 함수에서 `next`로 주어져서, 다른 인수와 함께, 아니면 다른 시점에, 아니면 전혀 호출되지 않을 수도 있는, `next(action)`을 호출하는 `action`의 함수여야 합니다. 체인의 마지막 미들웨어는 `next` 인자로 원래 저장소의 [`dispatch`](Store.md#dispatch)를 받아 체인을 마무리합니다. 그러므로 미들웨어의 시그니처는 `({ getState, dispatch }) => next => action`입니다.

#### 반환

(_Function_) 주어진 미들웨어를 적용하는 저장소 인핸서입니다. 시그니처는 `createStore => createStore'`이지만, 인핸서를 적용하는 가장 간단한 방법은 [`createStore()`](./createStore.md)의 마지막 인수인 `enhancer`로 넘기는 것입니다.

#### 예제: Custom Logger Middleware

```js
import { createStore, applyMiddleware } from 'redux'
import todos from './reducers'

function logger({ getState }) {
  return next => action => {
    console.log('will dispatch', action)

    // Call the next dispatch method in the middleware chain.
    const returnValue = next(action)

    console.log('state after dispatch', getState())

    // This will likely be the action itself, unless
    // a middleware further in chain changed it.
    return returnValue
  }
}

const store = createStore(todos, ['Use Redux'], applyMiddleware(logger))

store.dispatch({
  type: 'ADD_TODO',
  text: 'Understand the middleware'
})
// (These lines will be logged by the middleware:)
// will dispatch: { type: 'ADD_TODO', text: 'Understand the middleware' }
// state after dispatch: [ 'Use Redux', 'Understand the middleware' ]
```

#### 예제: Using Thunk Middleware for Async Actions

```js
import { createStore, combineReducers, applyMiddleware } from 'redux'
import thunk from 'redux-thunk'
import * as reducers from './reducers'

const reducer = combineReducers(reducers)
// applyMiddleware supercharges createStore with middleware:
const store = createStore(reducer, applyMiddleware(thunk))

function fetchSecretSauce() {
  return fetch('https://www.google.com/search?q=secret+sauce')
}

// These are the normal action creators you have seen so far.
// The actions they return can be dispatched without any middleware.
// However, they only express “facts” and not the “async flow”.
function makeASandwich(forPerson, secretSauce) {
  return {
    type: 'MAKE_SANDWICH',
    forPerson,
    secretSauce
  }
}

function apologize(fromPerson, toPerson, error) {
  return {
    type: 'APOLOGIZE',
    fromPerson,
    toPerson,
    error
  }
}

function withdrawMoney(amount) {
  return {
    type: 'WITHDRAW',
    amount
  }
}

// Even without middleware, you can dispatch an action:
store.dispatch(withdrawMoney(100))

// But what do you do when you need to start an asynchronous action,
// such as an API call, or a router transition?

// Meet thunks.
// A thunk is a function that returns a function.
// This is a thunk.
function makeASandwichWithSecretSauce(forPerson) {
  // Invert control!
  // Return a function that accepts `dispatch` so we can dispatch later.
  // Thunk middleware knows how to turn thunk async actions into actions.
  return function (dispatch) {
    return fetchSecretSauce().then(
      sauce => dispatch(makeASandwich(forPerson, sauce)),
      error => dispatch(apologize('The Sandwich Shop', forPerson, error))
    )
  }
}

// Thunk middleware lets me dispatch thunk async actions
// as if they were actions!
store.dispatch(makeASandwichWithSecretSauce('Me'))

// It even takes care to return the thunk's return value
// from the dispatch, so I can chain Promises as long as I return them.
store.dispatch(makeASandwichWithSecretSauce('My wife')).then(() => {
  console.log('Done!')
})

// In fact I can write action creators that dispatch
// actions and async actions from other action creators,
// and I can build my control flow with Promises.
function makeSandwichesForEverybody() {
  return function (dispatch, getState) {
    if (!getState().sandwiches.isShopOpen) {
      // You don't have to return Promises, but it's a handy convention
      // so the caller can always call .then() on async dispatch result.
      return Promise.resolve()
    }

    // We can dispatch both plain object actions and other thunks,
    // which lets us compose the asynchronous actions in a single flow.
    return dispatch(makeASandwichWithSecretSauce('My Grandma'))
      .then(() =>
        Promise.all([
          dispatch(makeASandwichWithSecretSauce('Me')),
          dispatch(makeASandwichWithSecretSauce('My wife'))
        ])
      )
      .then(() => dispatch(makeASandwichWithSecretSauce('Our kids')))
      .then(() =>
        dispatch(
          getState().myMoney > 42
            ? withdrawMoney(42)
            : apologize('Me', 'The Sandwich Shop')
        )
      )
  }
}

// This is very useful for server side rendering, because I can wait
// until data is available, then synchronously render the app.

import { renderToString } from 'react-dom/server'

store
  .dispatch(makeSandwichesForEverybody())
  .then(() => response.send(renderToString(<MyApp store={store} />)))

// I can also dispatch a thunk async action from a component
// any time its props change to load the missing data.

import React from 'react'
import { connect } from 'react-redux'

function SandwichShop(props) {
  const { dispatch, forPerson } = props

  useEffect(() => {
    dispatch(makeASandwichWithSecretSauce(forPerson))
  }, [forPerson])

  return <p>{this.props.sandwiches.join('mustard')}</p>
}

export default connect(state => ({
  sandwiches: state.sandwiches
}))(SandwichShop)
```

#### 팁

- 미들웨어는 저장소의 [`dispatch`](Store.md#dispatch) 함수만을 감쌉니다. 기술적으로는, 미들웨어가 할 수 있는 모든 것을 모든 `dispatch` 호출을 직접 감싸서 할 수 있습니다. 하지만 이들을 한 곳에서 관리하고 전체 프로젝트에서 액션의 변환을 정의할 수 있는 가장 쉬운 방법은 미들웨어입니다.

- 만약 `applyMiddleware`이외의 저장소 인핸서를 사용한다면, 미들웨어는 비동기적일 수 있으므로 `applyMiddleware`를 체인에서 앞쪽에 두도록 하세요. 예를 들어 `applyMiddleware`를 [redux-devtools](https://github.com/gaearon/redux-devtools) 앞에 두지 않으면 DevTools은 Promise 등의 미들웨어에 전달되는 액션들을 볼 수 없을겁니다.

- 만약 조건부로 미들웨어를 적용하고 싶다면 필요할 때에만 임포트하도록 하세요:

  ```js
  let middleware = [a, b]
  if (process.env.NODE_ENV !== 'production') {
    const c = require('some-debug-middleware')
    const d = require('another-debug-middleware')
    middleware = [...middleware, c, d]
  }

  const store = createStore(
    reducer,
    preloadedState,
    applyMiddleware(...middleware)
  )
  ```

이렇게 하면 번들링 도구들이 필요없는 모듈과 리듀서를 제거해서 빌드 사이즈를 줄이기 쉽게 됩니다.

- `applyMiddleware` 자신이 뭔지 궁금한가요? 당연히 미들웨어 자체보다 더 강력한 확장 매커니즘입니다. 사실 `applyMiddleware`는 Redux의 가장 강력한 확장 매커니즘인 [저장소 인핸서](../understanding/thinking-in-redux/Glossary.md#저장소-인핸서)의 한 예입니다. 저장소 인핸서의 다른 예로는 [redux-devtools](https://github.com/gaearon/redux-devtools)가 있습니다. 미들웨어는 저장소 인핸서보다는 덜 강력하지만, 작성하기는 더 쉽습니다.

- 미들웨어는 실제보다 더 복잡하게 들립니다. 미들웨어를 정말로 이해하려면 기존의 미들웨어가 어떻게 작동하는지 보고, 여러분이 직접 작성해보는 방법 뿐입니다. 함수를 중첩하는 일에 겁을 먹을 수도 있지만, 대부분의 미들웨어들이 사실 10줄 내외이고, 중첩과 조합가능성이야말로 미들웨어 시스템을 강력하게 만들어줍니다.

- 저장소 인핸서를 여럿 적용하려면, [`compose()`](./compose.md)를 사용하세요.
