---
id: store
title: Store
description: 'API > Store: the core Redux store methods'
---

# Store

앱의 전체 [상태 트리](../understanding/thinking-in-redux/Glossary.md#상태)를 가지고 있는 저장소입니다.
이 안의 상태를 바꾸는 유일한 방법은 여기에 [액션](../understanding/thinking-in-redux/Glossary.md#액션)을 보내는 것 뿐입니다.

저장소는 클래스가 아닙니다. 단지 안에 몇가지 메서드가 들어있는 객체일 뿐입니다.
생성하기 위해서는 루트 [리듀싱 함수](../understanding/thinking-in-redux/Glossary.md#리듀서)를 [`createStore`](createStore.md)에 전달하면 됩니다.

> ##### Flux 사용자를 위한 한마디

> Flux를 쓰다가 오셨다면, 이해해야 하는 한 가지 중요한 차이점이 있습니다. Redux는 디스패쳐를 가지지도 않고, 여러개의 저장소를 지원하지도 않습니다. **대신, 하나의 루트 [리듀싱 함수](../understanding/thinking-in-redux/Glossary.md#리듀서)를 가지는 하나의 저장소만 있습니다.** 앱이 커짐에 따라 저장소를 추가하는 대신 루트 리듀서를 여러개의 작은 리듀서로 나누면 됩니다. 각 리듀서는 상태 트리의 서로 다른 부분들에서 독립적으로 작동합니다. 이들을 합치기 위해 [`combineReducers`](combineReducers.md) 같은 헬퍼를 사용할 수 있습니다. 이는 마치 React 앱에 하나의 루트 컴포넌트만 존재하지만, 그 안에 여러 작은 컴포넌트들이 조합되어 있는 것과 마찬가지입니다.

### Store 메서드

- [Store](#store)
  - [Store 메서드](#store-메서드)
  - [Store 메서드](#store-메서드-1)
    - [`getState()`](#getstate)
      - [반환](#반환)
    - [`dispatch(action)`](#dispatchaction)
      - [전달인자](#전달인자)
      - [반환](#반환-1)
      - [참고](#참고)
      - [예제](#예제)
    - [`subscribe(listener)`](#subscribelistener)
      - [전달인자](#전달인자-1)
        - [반환](#반환-2)
        - [예제](#예제-1)
    - [`replaceReducer(nextReducer)`](#replacereducernextreducer)
      - [전달인자](#전달인자-2)

## Store 메서드

### [`getState()`](#getState)

애플리케이션의 현재 상태 트리를 반환합니다.
저장소의 리듀서가 마지막으로 반환한 값과 동일합니다.

#### 반환

_(any)_: 애플리케이션의 현재 상태 트리.

---

&nbsp;

### [`dispatch(action)`](#dispatch)

액션을 보냅니다. 이것이 상태 변경을 일으키기 위한 유일한 방법입니다.

저장소의 리듀싱 함수는 [`getState()`](#getState)의 현재 결과와 주어진 `액션`과 함께 동기적으로 호출됩니다. 반환된 값이 다음 상태가 되어 이제부터 [`getState()`](#getState)에서 반환될 것이고, 상태 변경 리스너들은 즉시 알림을 받을 것입니다.

> ##### Flux 사용자들을 위한 한마디
>
> 만약 `dispatch`를 [리듀서](../understanding/thinking-in-redux/Glossary.md#리듀서)안에서 호출하려고 하면 “Reducers may not dispatch actions.” 이라는 에러를 일으킬 것입니다. 이는 Flux의 “Cannot dispatch in a middle of dispatch” 에러와 비슷하지만, 이와 관련된 문제를 발생시키지는 않습니다. Flux에서는 Store가 액션을 다루고 업데이트를 처리하는 동안 액션을 보내는 것이 금지되어 있습니다. 이는 컴포넌트 생명주기 훅이나 다른 곳에서 액션을 보낼 수 없게 하기 때문에 좋지 않습니다.

> Redux에서는 루트 리듀서가 새 상태를 반환한 다음에 구독자들이 호출되기 때문에, 리스너 안에서도 액션을 _보낼 수_ 있습니다. 액션을 보낼 수 없는 곳은 리듀서 안쪽 뿐이며, 리듀서에서는 사이드이펙트가 허용되지 않기 때문입니다. 액션에 대한 응답으로 사이드이펙트를 일으키고 싶다면 [액션 생산자](../understanding/thinking-in-redux/Glossary.md#액션-생산자) 안에서 하는게 적절합니다.

#### 전달인자

1. `action` (_Object_<sup>†</sup>): 앱의 변경사항을 기술하는 평범한 객체입니다. 액션은 저장소로 데이터를 보내는 유일한 방법이기 때문에 UI 이벤트, 네트워크 콜백, 웹소켓 등 다른 어떤 소스에서 오는 데이터든간에 액션을 통해 보내져야 합니다. 액션은 반드시 어떤 형태의 액션이 행해질지 지시하는 `type` 필드를 가져야 합니다. `type`에는 [Symbols](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Global_Objects/Symbol) 보다는 문자열을 사용하는 편이 직렬화가 가능하기 때문에 더 낫습니다. `type` 외에 액션 객체의 구조는 여러분에게 달려 있습니다. 관심이 있다면 액션을 어떻게 구성하는것을 추천하는지 [Flux Standard Action](https://github.com/acdlite/flux-standard-action)에서 확인해보세요.

#### 반환

(Object<sup>†</sup>): 송달된 액션(참고를 보세요).

#### 참고

<sup>†</sup> [`createStore`](createStore.md) 호출에서 얻을 수 있는 평범한(vanilla) 저장소는 평범한 오브젝트만을 액션으로 받을 수 있으며 받은 액션은 바로 리듀서에 넘깁니다.

하지만 [`createStore`](createStore.md)를 [`applyMiddleware`](applyMiddleware.md)로 감싸면 미들웨어가 액션을 중간에 다르게 처리해서 [비동기 액션](../understanding/thinking-in-redux/Glossary.md#비동기-액션)을 보낼 수 있게 해줍니다. 비동기 액션은 보통 Promise, Observable, thunk와 같은 비동기 원시 타입입니다.

미들웨어는 커뮤니티에서 만들어지고 Redux에 기본적으로 포함되어 나오지는 않습니다. 사용하기 위해서는 [redux-thunk](https://github.com/gaearon/redux-thunk)나 [redux-promise](https://github.com/acdlite/redux-promise) 같은 패키지를 직접 설치해줘야 합니다. 여러분이 직접 미들웨어를 만들 수도 있습니다.

비동기적으로 API를 호출하거나, 액션 생산자 안에서 현재 상태를 읽거나, 사이드이펙트를 일으키거나, 이들을 순차적으로 엮는 방법을 알아보려면 [`applyMiddleware`](applyMiddleware.md)의 예제를 보세요.

#### 예제

```js
import { createStore } from 'redux'
const store = createStore(todos, ['Use Redux'])

function addTodo(text) {
  return {
    type: 'ADD_TODO',
    text
  }
}

store.dispatch(addTodo('Read the docs'))
store.dispatch(addTodo('Read about the middleware'))
```

---

### [`subscribe(listener)`](#subscribe)

변경사항에 대한 리스너를 추가합니다. 리스너는 액션이 보내져서 상태 트리의 일부가 변경될 수 있을 때마다 호출됩니다. 콜백 안에서 현재 상태 트리를 읽으려면 [`getState()`](#getState)를 호출하면 됩니다.

다음 유의사항들을 따른다면 리스너 안에서 [`dispatch()`](#dispatch)를 호출할 수도 있습니다:

1. 리스너는 사용자 액션에 응답하기 위해서나 특정 조건 하에서만 [`dispatch()`](#dispatch)를 호출해야 합니다. 기술적으로는 아무런 조건 없이 [`dispatch()`](#dispatch)를 호출할 수 있지만, 호출될때마다 다시 리스너를 작동시키기 때문에 무한 루프에 빠질 것입니다.

2. 구독자들은 매번 [`dispatch()`](#dispatch)가 호출되기 전 시점의 것들이 사용됩니다. 만약 리스너 안에서 구독이나 구독취소를 하더라도 지금 진행중인 [`dispatch()`](#dispatch)에는 영향을 미치지 않을 것입니다. 하지만 중첩 여부와 관계 없이, 다음 [`dispatch()`](#dispatch) 호출에서는 더 최근의 구독 목록이 사용될 것입니다.

3. 리스너가 호출되기 전에 중첩된 [`dispatch()`](#dispatch)들이 상태를 여러번 업데이트 할 수 있기 때문에, 리스너가 모든 상태 변화를 받아볼 것이라고 생각하지는 마십시오. 하지만 [`dispatch()`](#dispatch)가 시작하기 전에 등록된 모든 구독자들은 해당 시점의 최신 상태를 받아볼 것이 보장됩니다.

이것은 로우레벨 API이기 때문에 직접 사용하기보다는 React(또는 다른) 바인딩을 사용하게 될 것입니다. 상태 변화에 반응하기 위해 콜백을 주로 사용한다면, [직접 `observeStore` 유틸리티를 작성](https://github.com/reactjs/redux/issues/303#issuecomment-125184409)하기를 원할수도 있습니다. `Store`는 [`Observable`](https://github.com/zenparsing/es-observable)이기도 하므로 [RxJS](https://github.com/ReactiveX/RxJS) 같은 라이브러리를 통해 변경사항을 `subscribe` 할 수 있습니다.

리스너의 구독을 해지하려면 `subscribe`가 반환한 함수를 사용하세요.

#### 전달인자

1. `listener` (_Function_): 액션이 보내져서 상태 트리가 바뀌게 될 때마다 호출할 콜백입니다. 현재 상태 트리를 읽기 위해 콜백 내에서 [`getState()`](#getState)를 호출할 수 있습니다. 저장소의 리듀서는 순수 함수일 것이므로 상태 트리의 값이 변경되었는지 확인하기 위해 레퍼런스를 비교할 수 있습니다.

##### 반환

(_Function_): 변경 리스너를 구독 해지하는 함수.

##### 예제

```js
function select(state) {
  return state.some.deep.property
}

let currentValue
function handleChange() {
  let previousValue = currentValue
  currentValue = select(store.getState())

  if (previousValue !== currentValue) {
    console.log(
      'Some deep nested property changed from',
      previousValue,
      'to',
      currentValue
    )
  }
}

const unsubscribe = store.subscribe(handleChange)
unsubscribe()
```

---

### [`replaceReducer(nextReducer)`](#replaceReducer)

현재 저장소에서 상태를 계산하기 위해 사용중인 리듀서를 교체합니다.

이것은 고급 API로 코드 분할이나 동적으로 리듀서를 불러오고 싶을 때 사용할 수 있습니다. Redux에서 핫 리로딩을 구현하기 위해서도 사용할 수 있습니다.

#### 전달인자

1. `reducer` (_Function_) 저장소가 앞으로 사용할 리듀서
