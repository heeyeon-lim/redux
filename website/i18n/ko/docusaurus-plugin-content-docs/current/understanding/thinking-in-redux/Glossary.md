---
id: glossary
title: 용어집
---

# 용어집

이 문서는 Redux의 핵심 용어들을 그 타입 시그니처와 함께 모아둔 용어집입니다. 타입은 [Flow notation](http://flowtype.org/docs/quick-reference.html)에 따라 문서화되어 있습니다.

## 상태

```js
type State = any
```

**상태**(**상태 트리**라고도 합니다)는 넓은 의미의 단어이지만, Redux API에서는 보통 저장소에 의해 관리되고 [`getState()`](api/Store.md#getState)에 의해 반환되는 하나의 상태값을 지칭합니다. 상태는 Redux 애플리케이션의 전체 상태를 나타내며, 보통 깊게 중첩되어 있는 객체입니다.

컨벤션에 따르면 최상위 상태는 객체나 Map과 같은 키-값 컬렉션이지만, 기술적으로는 어떤 타입이든 가능합니다. 다만 상태를 항상 직렬화 가능하게 두는 것이 좋습니다. JSON으로 쉽게 변환될 수 없는 것들을 안에 넣지 마세요.

## 액션

```js
type Action = Object
```

**액션**은 상태를 변화시키려는 의도를 표현하는 평범한 객체입니다. 액션은 저장소에 데이터를 넣는 유일한 방법입니다. UI 이벤트에서 왔든, 네트워크 콜백에서 왔든, 웹소켓과 같은 다른 소스에서 왔든 모든 데이터는 액션으로써 보내집니다.

액션은 어떤 형태의 액션이 행해질지 표시하는 `type` 필드를 가져야 합니다.. 타입은 상수로 정의되고 다른 모듈에서 임포트할 수 있습니다. 문자열은 직렬화될 수 있기 때문에 타입으로 [Symbol](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Global_Objects/Symbol) 보다는 문자열을 쓰는 것이 좋습니다.

액션 객체에서 `type`외의 다른 부분은 여러분 마음대로입니다. 관심이 있다면 [Flux Standard Action](https://github.com/acdlite/flux-standard-action)에서 액션이 어떻게 구성되어야 하는지 권장사항을 알아보세요.

아래의 [비동기 액션](#비동기-액션)도 보시기 바랍니다.

## 리듀서

```js
type Reducer<S, A> = (state: S, action: A) => S
```

**리듀서**(**리듀싱 함수**라고 부르기도 합니다)는 누적값과 값을 받아서 새로운 누적값을 반환하는 함수입니다. 이들은 값들의 컬렉션을 받아서 하나의 값으로 줄이는데 사용됩니다.

리듀서는 Redux만의 개념은 아닙니다. 기본 개념은 함수형 프로그래밍에서 왔습니다. 자바스크립트와 같은 함수형이 아닌 프로그래밍 언어들에도 대부분 리듀싱을 위한 내장 API가 있습니다. 자바스크립트에서는 [`Array.prototype.reduce()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/Reduce)가 해당됩니다.

Redux에서 누적값은 상태 객체이고, 누적될 값은 액션입니다. 리듀서는 주어진 이전 상태와 액션에서 새로운 상태를 계산합니다. 이들은 반드시 같은 입력이 있으면 같은 출력을 반환하는 **순수 함수**여야만 합니다. 이들은 부수효과를 가져서는 안됩니다. 이를 통해 핫 리로딩과 시간여행과 같은 멋진 기능들이 가능해집니다.

리듀서는 Redux에서 가장 중요한 개념입니다.

_API 호출을 리듀서 안에 넣지 마세요._

## 디스패치 함수

```js
type BaseDispatch = (a: Action) => Action
type Dispatch = (a: Action | AsyncAction) => any
```

**디스패치 함수**는 액션이나 [비동기 액션](#비동기-액션)을 받는 함수입니다. 받은 다음 하나나 여러개의 액션을 저장소에 보낼수도 보내지 않을수도 있습니다.

보통의 디스패치 함수와 저장소 인스턴스가 미들웨어를 거치지 않고 제공하는 기본 [`dispatch`](api/Store.md#dispatchaction) 함수를 구분해야 합니다.

기본 디스패치 함수는 **반드시** 동기적으로 저장소의 리듀서에 액션을 보내야 합니다. 그러면 리듀서는 저장소가 반환한 이전 상태와 함께 새 상태를 계산합니다. 리듀서가 사용하기 위해서 액션은 평범한 객체여야 합니다.

[미들웨어](#미들웨어)는 기본 디스패치 함수를 감쌉니다. 미들웨어를 통해 디스패치 함수는 액션 뿐 아니라 [비동기 액션](#비동기-액션)울 처리할 수 있습니다. 미들웨어는 액션이나 비동기 액션을 다음 미들웨어에 넘기기 전에 변환하거나, 지연시키거나, 무시하거나, 해석할 수 있습니다. 더 알아보려면 아래를 보세요.

## 액션 생산자

```js
type ActionCreator<A, P extends any[] = any[]> = (...args: P) => Action | AsyncAction
```

**액션 생산자**는 단지 액션을 만드는 함수입니다. 액션은 정보의 묶음이고, 액션 생산자는 액션을 만드는 곳이니 두 용어를 혼동하지 마세요.

액션 생산자를 호출하면 액션을 만들어낼 뿐 디스패치하지는 않습니다. 저장소를 변경하기 위해서는 [`dispatch`](api/Store.md#dispatch) 함수를 호출해야 합니다. 액션 생산자를 호출해 그 결과를 저장소 인스턴스로 바로 디스패치하는 함수를 **바인드된 액션 생산자**라고 부르기도 합니다.

액션 생산자가 현재 상태를 읽어야 하거나, API 호출을 실행해야 하거나, 라우트 전환같은 부수효과를 일으켜야 한다면, 액션 대신 [비동기 액션](#비동기-액션)을 반환해야 합니다.

## 비동기 액션

```js
type AsyncAction = any
```

**비동기 액션**은 디스패치 함수로 보내지는 값이지만, 아직 리듀서에게 받아들여질 준비가 되어 있지는 않았습니다. 비동기 액션은 기본 [`dispatch()`](api/Store.md#dispatchaction) 함수로 전달되기 전에 [미들웨어](#미들웨어)를 통해 액션(이나 일련의 액션들)으로 바뀌어야 합니다. 비동기 액션은 여러분이 사용하는 미들웨어에 따라 서로 다른 타입이 될 수 있습니다. 이들은 종종 Promise나 썽크와 같은 비동기 기본형으로, 리듀서에게 직접 전달되지는 않지만, 작업이 완료되면 액션을 보냅니다.

## 미들웨어

```js
type MiddlewareAPI = { dispatch: Dispatch, getState: () => State }
type Middleware = (api: MiddlewareAPI) => (next: Dispatch) => Dispatch
```

미들웨어는 [디스패치 함수](#디스패치-함수)를 결합해서 새 디스패치 함수를 반환하는 고차함수입니다. 이들은 종종 [비동기 액션](#비동기-액션)을 액션으로 전환합니다.

미들웨어는 함수 결합을 통해 서로 결합할 수 있습니다. 이는 액션을 로깅하거나, 라우팅과 같은 부수효과를 일으키거나, 비동기 API 호출을 일련의 동기 액션으로 바꾸는데 유용합니다.

미들웨어에 대해 자세히 알아보려면 [`applyMiddleware(...middlewares)`](../../api/applyMiddleware.md)를 보세요.

## 저장소

```js
type Store = {
  dispatch: Dispatch,
  getState: () => State,
  subscribe: (listener: () => void) => () => void,
  replaceReducer: (reducer: Reducer) => void
}
```

저장소는 애플리케이션의 상태 트리를 가지고 있는 객체입니다.
리듀서 수준에서 결합이 일어나기 때문에, Redux 앱에는 단 하나의 저장소만 있어야 합니다.

- [`dispatch(action)`](api/Store.md#dispatchaction)는 위에서 설명한 기본 디스패치 함수입니다.
- [`getState()`](api/Store.md#getState)는 저장소의 현재 상태를 반환합니다.
- [`subscribe(listener)`](api/Store.md#subscribelistener)는 상태가 바뀔 때 호출될 함수를 등록합니다.
- [`replaceReducer(nextReducer)`](api/Store.md#replacereducernextreducer)는 핫 리로딩과 코드 분할을 구현할때 사용됩니다. 여러분이 사용할 일은 별로 없습니다.

자세한 사항은 [저장소 API 레퍼런스](api/Store.md#dispatchaction)를 보세요.

## 저장소 생산자

```js
type StoreCreator = (reducer: Reducer, preloadedState: ?State) => Store
```

저장소 생산자는 Redux 저장소를 만드는 함수입니다. 디스패치 함수와 마찬가지로, Redux 패키지에 들어있는 기본 저장소 생산자인 [`createStore(reducer, preloadedState)`](api/createStore.md)와 저장소 인핸서에서 반환되는 저장소 생산자는 구분해야 합니다.

## 저장소 인핸서

```js
type StoreEnhancer = (next: StoreCreator) => StoreCreator
```

저장소 인핸서는 저장소 생산자를 결합하여 강화된 새 저장소 생산자를 반환하는 고차함수입니다. 이는 미들웨어와 비슷하게 조합가능한 방식으로 저장소 인터페이스를 바꿀 수 있게 해줍니다.

저장소 인핸서는 React에서 "컴포넌트 인핸서"로 불리는 고차 컴포넌트와 같은 개념입니다.

저장소는 인스턴스라기보다는 함수들이 모인 보통의 객체이기 때문에, 기존의 저장소를 변경하지 않고도 복제본을 만들고 수정할 수 있습니다. [`compose`](api/compose.md) 문서에 자세한 예제가 있습니다.

여러분이 저장소 인핸서를 작성할 일은 거의 없지만, [개발자 도구](https://github.com/reduxjs/redux-devtools)가 제공하는 인핸서는 사용하실겁니다. 이 인핸서는 앱에 다른 일이 일어날 걱정 없이도 시간여행을 가능하게 해줍니다. 재미있게도, [Redux의 미들웨어 구현](api/applyMiddleware.md)도 저장소 인핸서입니다.
