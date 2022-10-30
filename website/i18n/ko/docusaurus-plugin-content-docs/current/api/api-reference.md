---
id: api-reference
title: API 레퍼런스
---

# API 레퍼런스

Redux API의 드러난 부분은 매우 작습니다. Redux는 ([reducers](../understanding/thinking-in-redux/Glossary.md#리듀서)와 같은)구현을 위한 규약 모음을 정의하고, 이들 규약을 한데 묶기 위한 몇 가지 헬퍼 함수들을 제공합니다.

이 절은 Redux의 전체 API 문서입니다. Redux는 상태를 관리하는데에만 집중한다는 점을 기억해두세요. 실제 앱에서는 [react-redux](https://github.com/gaearon/react-redux)와 같은 UI 바인딩 또한 필요할겁니다.

### 최상위 익스포트

- [createStore(reducer, [preloadedState])](createStore.md)
- [combineReducers(reducers)](combineReducers.md)
- [applyMiddleware(...middlewares)](applyMiddleware.md)
- [bindActionCreators(actionCreators, dispatch)](bindActionCreators.md)
- [compose(...functions)](compose.md)

### Store API

- [Store](Store.md)
  - [getState()](Store.md#getState)
  - [dispatch(action)](Store.md#dispatchaction)
  - [subscribe(listener)](Store.md#subscribelistener)
  - [replaceReducer(nextReducer)](Store.md#replacereducernextreducer)

### 임포트하기

위의 함수들은 모두 최상위 익스포트입니다. 아래와 같이 임포트할 수 있습니다:

#### ES6

```js
import { createStore } from 'redux'
```

#### ES5 (CommonJS)

```js
var createStore = require('redux').createStore
```

#### ES5 (UMD build)

```js
var createStore = Redux.createStore
```
