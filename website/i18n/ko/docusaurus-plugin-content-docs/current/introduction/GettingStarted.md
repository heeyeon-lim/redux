---
id: getting-started
title: Redux 시작하기
description: '소개 > 시작하기: Redux를 배우고 사용하기 위한 자료'
---

import LiteYouTubeEmbed from 'react-lite-youtube-embed';
import 'react-lite-youtube-embed/dist/LiteYouTubeEmbed.css'

# Redux 시작하기

Redux는 자바스크립트 앱을 위한 예측 가능한 상태 컨테이너입니다.

Redux는 여러분이 일관적으로 동작하고, 서로 다른 환경(서버, 클라이언트, 네이티브)에서 작동하고, 테스트하기 쉬운 앱을 작성하도록 도와줍니다. 여기에 더해서 [시간여행형 디버거와 결합된 실시간 코드 수정](https://github.com/reduxjs/redux-devtools)과 같은 훌륭한 개발자 경험을 제공합니다.

여러분은 Redux를 [React](https://reactjs.org)나 다른 뷰 라이브러리와 함께 사용할 수 있습니다. Redux는 매우 작지만(의존 라이브러리 포함 2kB), 사용 가능한 애드온은 매우 많습니다.

## 설치

### Redux Toolkit

[**Redux Toolkit**](https://redux-toolkit.js.org)은 Redux 로직을 작성하기 위해 저희가 공식적으로 추천하는 방법입니다. RTK는 Redux 앱을 만들기에 필수적으로 여기는 패키지와 함수들을 포함합니다. 대부분의 Redux 작업을 단순화하고, 흔한 실수를 방지하며, Redux 앱을 만들기 쉽게 해주는 모범 사례를 통해 만들어졌습니다.

RTK는 [저장소 준비](https://redux-toolkit.js.org/api/configureStore),
[리듀서 생산과 불변 수정 로직 작성](https://redux-toolkit.js.org/api/createreducer),
[상태 "조각" 전부를 한번에 작성](https://redux-toolkit.js.org/api/createslice) 등 일반적인 작업들을 단순화해주는 유틸리티를 포함하고 있습니다.

여러분이 첫 프로젝트에 Redux를 새로 도입하는 입문자든 기존 앱을 단순화하고 싶은 경험자든 상관 없이,
**[Redux Toolkit](https://redux-toolkit.js.org/)**은 더 나은 Redux 코드를 만들게
도와줍니다.

Redux Toolkit은 NPM에서 패키지로 받아 모듈 번들러나 Node 앱에서 사용 가능합니다.

```bash
# NPM
npm install @reduxjs/toolkit

# Yarn
yarn add @reduxjs/toolkit
```

### React Redux 앱 만들기

React와 Redux로 새 앱을 만들기 위해 추천하는 방법은 [Create React App](https://github.com/facebook/create-react-app)를 위한 [공식 Redux+JS 템플릿](https://github.com/reduxjs/cra-template-redux)을 사용하는 것입니다. 이를 통해 **[Redux Toolkit](https://redux-toolkit.js.org/)**와 React Redux가 React 컴포넌트와 통합되는 이점을 누릴 수 있습니다.

```sh
npx create-react-app my-app --template redux
```

### Redux 코어

Redux 코어 라이브러리는 NPM에서 패키지로 받아 모듈 번들러나 Node 앱에서 사용 가능합니다.

```bash
# NPM
npm install redux

# Yarn
yarn add redux
```

`window.Redux` 전역변수를 선언해주는 UMD 패키지도 사용 가능합니다. UMD 패키지는 [`<script>` 태그](https://unpkg.com/redux/dist/redux.js)로 바로 사용 가능합니다.

자세한 사항은 [설치](Installation.md) 페이지를 보세요.

## 기본 예제

여러분의 앱의 상태 전부는 하나의 저장소(_store_)안에 있는 객체 트리에 저장됩니다.
상태 트리를 변경하는 유일한 방법은 무엇이 일어날지 서술하는 객체인 액션(_action_)을 보내는 것 뿐입니다.
액션이 상태 트리를 어떻게 변경할지 명시하기 위해 여러분은 리듀서(_reducers_)를 작성해야 합니다.

```js
import { createStore } from 'redux'

/**
 * 이것이 (state, action) => state 형태의 순수 함수인 리듀서입니다.
 * 리듀서는 액션이 어떻게 상태를 다음 상태로 변경하는지 서술합니다.
 *
 * 상태의 모양은 당신 마음대로입니다: 기본형(primitive)일수도, 배열일수도, 객체일수도,
 * 심지어 Immutable.js 자료구조일수도 있습니다.  오직 중요한 점은 상태 객체를 변경해서는 안되며,
 * 상태가 바뀐다면 새로운 객체를 반환해야 한다는 것입니다.
 *
 * 이 예제에서 우리는 `switch` 구문과 문자열을 썼지만,
 * 여러분의 프로젝트에 맞게
 * (함수 맵 같은) 다른 컨벤션을 따르셔도 좋습니다.
 */
function counter(state = 0, action) {
  switch (action.type) {
    case 'INCREMENT':
      return state + 1
    case 'DECREMENT':
      return state - 1
    default:
      return state
  }
}

// 앱의 상태를 보관하는 Redux 저장소를 만듭니다.
// API로는 { subscribe, dispatch, getState }가 있습니다.
let store = createStore(counter)

// subscribe()를 이용해 상태 변화에 따라 UI가 변경되게 할 수 있습니다.
// 보통은 subscribe()를 직접 사용하기보다는 뷰 바인딩 라이브러리(예를 들어 React Redux)를 사용합니다.
// 하지만 현재 상태를 localStorage에 영속적으로 저장할 때도 편리합니다.

store.subscribe(() => console.log(store.getState())))

// 내부 상태를 변경하는 유일한 방법은 액션을 보내는 것뿐입니다.
// 액션은 직렬화할수도, 로깅할수도, 저장할수도 있으며 나중에 재실행할수도 있습니다.
store.dispatch({ type: 'INCREMENT' })
// 1
store.dispatch({ type: 'INCREMENT' })
// 2
store.dispatch({ type: 'DECREMENT' })
// 1
```

상태를 바로 변경하는 대신, **액션**이라 불리는 평범한 객체를 통해 일어날 변경을 명시합니다. 그리고 각각의 액션이 전체 애플리케이션의 상태를 어떻게 변경할지 결정하는 특별한 함수인 **리듀서**를 작성합니다.

보통의 Redux 앱에는 하나의 루트 리듀서 함수를 가진 단 하나의 저장소가 있습니다. 앱이 커짐에 따라 루트 리듀서를 상태 트리의 서로 다른 부분에서 개별적으로 동작하는 작은 리듀서들로 나눌 수 있습니다. React 앱을 하나의 루트 컴포넌트에서 시작해서 여러 작은 컴포넌트의 조합으로 바꾸는 것과 동일합니다.

이런 아키텍처가 카운터 앱에서는 너무 과한 것처럼 보이지만, 크고 복잡한 앱에서는 이 패턴의 확장성이 잘 드러납니다. 액션에 따른 모든 변경을 추적할 수 있기 때문에, 매우 강력한 개발자 도구를 가능하게 해주기도 합니다. 여러분은 사용자 세션을 기록한 다음 액션 하나하나를 다시 실행해 볼 수 있습니다.

## Redux 배우기

Redux를 배우는 데 도움이 될 다양한 자료가 있습니다.

### Redux 핵심 튜토리얼

[**Redux 핵심 튜토리얼**](../tutorials/essentials/part-1-overview-concepts.md)은 추천 API와 모범 사례를 통해 어떻게 하면 Redux를 올바르게 사용할 수 있는지를 가르쳐주는 "하향식" 튜토리얼입니다. 여기서부터 시작하기를 추천합니다.

### Redux 기초 튜토리얼

[**Redux 기초 튜토리얼**](../tutorials/fundamentals/part-1-overview.md)은 요약 없이 기초 원리부터 "Redux가 어떻게 동작하는가"와 왜 Redux 표준 패턴이 있는지를 알려주는 "상향식" 튜토리얼입니다.

### 추가 튜토리얼

- Redux 레포지토리에 다양한 관점에서 Redux를 사용하는 법을 설명하는 몇몇 예제 프로젝트가 있습니다. 거의 모든 예제는 CodeSandbox의 샌드박스를 가지고 있어서, 온라인으로 상호작용할 수 있는 코드 버전으로 사용할 수 있습니다. **[예제 페이지](./Examples.md)**에서 모든 예제를 확인해보세요
- Redux 창시자 Dan Abramov의 **무료 ["Redux 시작하기" 강의 시리즈](https://app.egghead.io/playlists/fundamentals-of-redux-course-from-dan-abramov-bd5cc867)**와 Egghead.io의 **[관용적인 Redux로 React 애플리케이션 만들기](https://egghead.io/courses/building-react-applications-with-idiomatic-redux)** 강의.
- Redux 메인테이너 Mark Erikson의 **["Redux 기초" 컨퍼런스 토크](https://blog.isquaredsoftware.com/2018/03/presentation-reactathon-redux-fundamentals/)**와 [**"Redux 기초" 워크샵 슬라이드**](https://blog.isquaredsoftware.com/2018/06/redux-fundamentals-workshop-slides/)
- Dave Ceddia의 포스트 [*입문자를 위한 React Redux 튜토리얼 총정리**](https://daveceddia.com/redux-tutorial/)

### 그 외 자료

- **[Redux FAQ](../FAQ.md)**에는 Redux 사용 방법에 대해 흔히 하는 질문에 대한 답변들이 있고, **["레시피" 문서 섹션](../usage/index.md)**에서 파생 데이터 핸들링, 테스팅, 리듀서 로직 구성하기, 보일러 플레이트 줄이기에 대한 정보를 확인할 수 있습니다.
- Redux 메인테이너 Mark Erikson의 **["실용적인 Redux" 튜토리얼 시리즈](http://blog.isquaredsoftware.com/series/practical-redux/)**는 React 및 Redux로 작업하기 위한 실제적인 중고급 기술을 보여줍니다. (**[Educative.io의 인터랙티브 강좌](https://www.educative.io/collection/5687753853370368/5707702298738688)**에서도 확인 가능).
- **[React/Redux 링크 리스트](https://github.com/markerikson/react-redux-links)**는 [리듀서와 실렉터](https://github.com/markerikson/react-redux-links/blob/master/redux-reducers-selectors.md) 다루기, [사이드 이팩트 관리하기](https://github.com/markerikson/react-redux-links/blob/master/redux-side-effects.md), [Redux 구조와 모범 사례](https://github.com/markerikson/react-redux-links/blob/master/redux-architecture.md) 등등의 아티클을 분류해놓은 리스트입니다.
- 우리 커뮤니티는 수천가지의 Redux와 관련된 라이브러리, 애드온 및 툴들을 개발했습니다. **["생태계" 문서 페이지](./Ecosystem.md)**에 우리의 추천 항목들이 있고, **[Redux 애드온 카탈로그](https://github.com/markerikson/redux-ecosystem-links)**에 모든 사용가능한 애드온 목록이 있습니다.

## 도움과 논의

**[Reactiflux 디스코드 커뮤니티](http://www.reactiflux.com)**의 **[#redux 채널](https://discord.gg/0ZcbPKXt5bZ6au5t)**이 우리의 공식 리소스로, Redux 사용법과 학습에 대한 질문들을 할 수 있습니다. Reactiflux를 구경해보고, 질문을 남기고 배워보세요!

또한, **[#redux 태그](https://stackoverflow.com/questions/tagged/redux)**를 이용해서 [스택 오버플로우](https://stackoverflow.com)에 질문을 남겨도 됩니다.

버그 리포트나 다른 피드백을 남겨야한다면, [Github 레포에 이슈를 작성해주세요.](https://github.com/reduxjs/redux)

## Redux를 사용해야 할까요?

Redux는 상태를 관리하기에 좋은 도구이지만 여러분의 상황에 적당한지는 따져 보아야 합니다. **단지 누군가가 사용하라고 했다는 이유만으로 Redux를 사용하지는 마세요 - 시간을 들여서 잠재적인 이점과 그에 따르는 단점을 이해하세요.**

Redux를 사용하기 적절한 때를 알기 위한 몇 가지 제안이 있습니다:

- 계속해서 바뀌는 상당한 양의 데이터가 있다
- 상태를 위한 단 하나의 근원이 필요하다
- 최상위 컴포넌트가 모든 상태를 가지고 있는 것은 더 이상 적절하지 않다

> **Redux가 어떻게 사용되어야 하는지에 대한 여러 생각들을 보려면:**
>
> - **[Redux FAQ: 언제 Redux를 사용해야할까요?](../faq/General.md#when-should-i-use-redux)**
> - **[당신에게 Redux는 필요 없을지도 모릅니다.](https://medium.com/@Dev_Bono/당신에게-redux는-필요-없을지도-모릅니다-b88dcd175754)**
>
> - **[Redux의 도, 파트 1 - 구현 및 의도](http://blog.isquaredsoftware.com/2017/05/idiomatic-redux-tao-of-redux-part-1/)**
>
> - **[Redux의 도, 파트 1 - 관례와 철학](http://blog.isquaredsoftware.com/2017/05/idiomatic-redux-tao-of-redux-part-2/)**
> - **[Redux FAQ](../FAQ.md)**
