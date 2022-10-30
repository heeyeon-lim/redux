---
id: migrating-to-redux
title: Redux로 마이그레이션
---

# Redux로 마이그레이션

Redux는 하나의 거대한 프레임워크가 아닌, 규약들과 [그들을 함께 작동하도록 묶는 함수들](../api/api-reference.md)로 이루어져 있습니다. 여러분의 "Redux 코드"의 주요 부분은 Redux API를 사용하지도 않으며, 여러분은 대부분의 시간을 함수를 작성하는데 보낼겁니다.

이는 Redux에서 옮기는 것과 Redux로 옮기는 것 양쪽 모두를 쉽게 만들어줍니다.
우리는 여러분이 묶여있기를 원하지 않습니다!

## Flux에서

[리듀서](../understanding/thinking-in-redux/Glossary.md#리듀서)는 Flux 저장소의 "핵심"을 지니고 있기 때문에, [Flummox](https://github.com/acdlite/flummox), [Alt](https://github.com/goatslacker/alt), [traditional Flux](https://github.com/facebook/flux), 기타 Flux 라이브러리 중 무엇을 쓰고 있었다고 해도 기존의 Flux 프로젝트에서 Redux로 옮기는 것은 어렵지 않습니다.

Redux에서 저들 라이브러리로 옮기는 것도 아래의 순서를 반대로 행하면 가능합니다.

여러분이 해야 할 순서는 다음과 같습니다:

- 기존 앱이 리듀서와 호환되게 하는 Flux 저장소를 만드는 `createFluxStore(reducer)`라는 이름의 함수를 만듭니다. 내부적으로는 Redux의 [`createStore`](../api/createStore.md) ([source](https://github.com/reduxjs/redux/blob/v4.0.5/src/createStore.js)) 구현과 비슷합니다. 이 저장소의 디스패치 핸들러는 액션을 받아서 `reducer`를 호출하고, 다음 상태를 저장하고, 변경사항을 알립니다.

- 이를 통해 앱의 모든 Flux 저장소들을 리듀서로 재작성하면서도 `createFluxStore(reducer)`를 익스포트해서 앱의 나머지 부분이 변화를 신경쓰지 않고 Flux 저장소를 바라보게끔 합니다.

- 저장소를 재작성하면서 저장소 안에서 API를 받아오거나 액션을 일으키는 것 같은 Flux 안티패턴들을 피해야 합니다. 리듀서 기반으로 옮기면서 여러분의 Flux 코드는 더 읽기 쉬워질겁니다!

- Flux 저장소를 모두 리듀서 기반으로 옮기고 나면 Flux 라이브러리를 Redux 저장소로 교체하고 작성한 리듀서들을 [`combineReducers(reducers)`](../api/combineReducers.md)을 통해 합칩니다.

- 이제 남은것은 [React-Redux](../tutorials/fundamentals/part-5-ui-and-react.md)나 다른 것들을 사용해 UI를 옮기는 작업입니다.

- 마지막으로 비동기 코드를 더 단순화하기 위해 미들웨어같은 Redux 문법을 사용하기를 원할겁니다.

## Backbone에서

Backbone's model layer is quite different from Redux, so we don't suggest mixing them. If possible, it is best that you rewrite your app's model layer from scratch instead of connecting Backbone to Redux. However, if a rewrite is not feasible, you may use [backbone-redux](https://github.com/redbooth/backbone-redux) to migrate gradually, and keep the Redux store in sync with Backbone models and collections.

If your Backbone codebase is too big for a quick rewrite or you don't want to manage interactions between store and models, use [backbone-redux-migrator](https://github.com/naugtur/backbone-redux-migrator) to help your two codebases coexist while keeping healthy separation. Once your rewrite finishes, Backbone code can be discarded and your Redux application can work on its own once you configure router.
