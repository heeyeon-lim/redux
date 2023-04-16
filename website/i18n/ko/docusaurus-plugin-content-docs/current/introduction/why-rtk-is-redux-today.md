---
id: why-rtk-is-redux-today
title: Redux Toolkit이 오늘날 Redux를 사용하는 방법인 이유 
description: '소개 > RTK가 현재의 Redux인 이유: RTK가 어떻게 Redux 코어를 대체하는지에 대한 상세 내용'
---

## Redux Toolkit은 무엇인가요?

[**Redux Toolkit**](https://redux-toolkit.js.org) (줄여서 **"RTK"**)은 Redux 로직을 작성하기 위해 저희가 공식적으로 추천하는 방법입니다. `@reduxjs/toolkit` 패키지는 코어 `redux` 패키지를 포함하며, Redux 앱을 만드는 데 필수적인 API 메서드와 공통 의존성을 포함합니다. Redux Tookit은 저희가 제안하는 모범 사례를 통해 만들어졌으며, 대부분의 Redux 작업을 단순화하고, 흔한 실수를 방지하며, Redux 애플리케이션을 더 쉽게 작성할 수 있도록 지원합니다.

**현재 Redux 로직을 작성하고 있다면 Redux Toolkit을 사용하여 코드를 작성하세요!**

RTK는 일반적인 작업들을 단순화해주는 유틸리티가 포함되어 있습니다. 예를 들면, [스토어 설정](https://redux-toolkit.js.org/api/configureStore),
[리듀서 생성과 불변 수정 로직 작성](https://redux-toolkit.js.org/api/createreducer),
와 [한번에 모든 상태 슬라이스 작성](https://redux-toolkit.js.org/api/createslice) 이 있습니다.

Redux를 처음 사용하는 입문자든, 기존 애플리케이션을 간소화하고자 하는 경험자든, **[Redux Toolkit](https://redux-toolkit.js.org/)** 은 여러분의 Redux 코드를 개선할 수 있도록 도와줍니다. 

:::팁

"모던 Redux"를 Redux Toolkit과 함께 사용하는 방법을 배우려면 다음 페이지를 참조하세요:

- [**"Redux Essentials" 튜토리얼**](../tutorials/essentials/part-1-overview-concepts.md), 은 실제 앱에서 Redux Toolkit을 사용하여 "Redux를 올바르게 사용하는 방법"을 가르칩니다,
- [**Redux 기초, 파트 8: Redux Toolkit과 함께 하는 Modern Redux**](../tutorials/fundamentals/part-8-modern-redux.md), 는 이전 섹션에 나온 저수준의 예시들을 모던 Redux Toolkit과 동등한 예제로 변환하는 방법을 보여줍니다
- [**Redux 사용하기: 모던 Redux로 마이그레이션하기**](../usage/migrating-to-modern-redux.mdx), 는 다양한 종류의 레거시 Redux 로직을 모던 Redux로 마이그레이션하는 방법을 다룹니다

:::

## Redux Toolkit은 Redux 코어와 어떻게 다른가요? 

### "Redux"란 무엇인가요?

우선 "Redux가 무엇인가?"를 질문해야 합니다.

Redux는 실제로 다음과 같습니다:

- "전역" 상태를 포함하는 단일 스토어 
- 앱에 어떤 일이 일어날 때 스토어에 일반 객체 액션을 디스패치하는 것
- 액션을 살펴보고 불변성을 유지한 채 업데이트된 상태를 반환하는 순수 리듀서 함수 

필수는 아니지만, [Redux 코드에는 보통 아래 항목들이 포함됩니다.](../tutorials/fundamentals/part-7-standard-patterns.md):

- 액션 객체를 생성하는 액션 생성자 
- 부수 효과를 가능하게 하는 미들웨어 
- 부수 효과를 가진 동기 또는 비동기 로직을 포함하는 Thunk 함수 
- ID로 항목 조회를 가능하게 하는 정규화된 상태
- Reselect 라이브러리를 사용하여 파생된 데이터를 최적화하는 메모이제이션된 셀렉터 함수
- 액션의 이력과 상태 변경을 확인할 수 있는 Redux DevTools 확장 프로그램
- 액션, 상태 및 기타 함수에 대한 TypeScript 타입 

추가적으로, Redux는 보통 React-Redux 라이브러리와 함께 사용되어 React 컴포넌트가 Redux 스토어와 상호 작용할 수 있게 합니다.

### Redux 코어는 무엇을 하나요?

Redux 코어는 매우 작고 의도적으로 주관적이지 않은 라이브러리입니다. 이는 몇 가지 작은 API 기본 요소를 제공합니다:

- `createStore`는 실제 Redux 스토어를 생성합니다 
- `combineReducers`는 여러 개의 slice리듀서를 하나의 큰 리듀서로 결합합니다 
- `applyMiddleware`는 여러 개의 미들웨어를 스토어 인핸서(enhancer)로 결합합니다 
- `compose`는 여러 개의 스토어 인핸서를 하나의 스토어 인핸서로 결합합니다 

이 외에, 애플리케이션에서 Redux와 관련된 모든 로직은 완전히 여러분이 작성해야 합니다.

이것의 좋은 점은 Redux를 많은 다양한 방법으로 사용할 수 있다는 것입니다. 그러나 나쁜 점은 Redux 코드 작성을 더 쉽게하는 도우미가 없다는 것입니다. 

예를 들어, 리듀서 함수는 _그저_ 함수입니다. Redux Toolkit 이전에는 대개 'switch' 문과 수동 업데이트를 사용하여 리듀서를 작성했습니다. 또한, 여러분은 직접 액션 생성자와 액션 타입도 작성했을 것입니다: 

```js title="Legacy hand-written Redux usage"
const ADD_TODO = 'ADD_TODO'
const TODO_TOGGLED = 'TODO_TOGGLED'

export const addTodo = text => ({
  type: ADD_TODO,
  payload: { text, id: nanoid() }
})

export const todoToggled = id => ({
  type: TODO_TOGGLED,
  payload: id
})

export const todosReducer = (state = [], action) => {
  switch (action.type) {
    case ADD_TODO:
      return state.concat({
        id: action.payload.id,
        text: action.payload.text,
        completed: false
      })
    case TODO_TOGGLED:
      return state.map(todo => {
        if (todo.id !== action.payload.id) return todo

        return {
          ...todo,
          completed: !todo.completed
        }
      })
    default:
      return state
  }
}
```

이 코드들은 `redux` 코어 라이브러리의 어떤 API에도 의존하지 않지만, 작성하기에는 많은 양의 코드입니다. 불변성을 지키기 위해 수동으로 객체를 복사(spread)하고 배열을 조작해야 합니다. 이 과정에서 상태를 실수로 변경하는 것이 매우 쉽습니다 (항상 Redux 버그의 #1 원인이죠!). 또한, 한 가지 기능의 코드를 `actions/todos.js`, `constants/todos.js`, 그리고 `reducers/todos.js`와 같이 여러 파일에 분산시키는 것이 엄격히 요구되는 것은 아니지만 일반적이었습니다.

또한, 스토어를 설정하려면 대개 일련의 단계를 거쳐서 thunks와 같이 자주 사용되는 미들웨어를 추가하고 Redux DevTools 확장 프로그램 지원을 활성화해야 합니다. 이러한 요소들이 거의 모든 Redux 앱에 쓰이는 기본적인 도구들인데도 말이죠.

### Redux Toolkit은 무얼 하나요? 

이것들은 _원래_ Redux 문서에서 보여준 패턴들이지만, 이들은 아주 장황하고 반복적인 코드를 필요로 합니다. 이러한 많은 보일러 플레이트 코드들은 Redux를 사용하는 데 필요하지 않습니다. 게다가, 이러한 보일러 플레이트 코드는 더 많은 실수를 유발할 가능성이 있습니다. 

**저희는 수동으로 작성하는 Redux 로직에서 "보일러 플레이트"를 제거하고, 흔한 실수를 방지하고, 기본적인 Redux 작업을 간단하게 만드는 API를 제공하기 위해 Redux Toolkit을 만들었습니다.**.

Redux Toolkit은 모든 Redux 앱에서 가장 일반적으로 하는 작업을 간소화하는 두 가지 주요 API로 시작합니다:

- `configureStore`는 한 번의 호출로 Redux 스토어를 설정하며, 리듀서를 결합하고 thunk 미들웨어를 추가하고, Redux DevTools 통합을 하는 등의 작업을 수행합니다. 또한, 이름이 있는 옵션 매개변수를 사용하기 때문에 `createStore`보다 구성이 쉽습니다.
- `createSlice`는 [Immer 라이브러리](https://immerjs.github.io/immer/)를 사용하는 리듀서를 작성할 수 있게 해줍니다. 이를 통해 `state.value = 123`과 같은 "변형 (mutating)" JS 문법을 spreads 없이도 불변성을 유지하며 업데이트할 수 있습니다. 또한, 각 리듀서에 대한 액션 생성자 함수를 자동으로 생성하고, 리듀서 이름에 기반하여 내부적으로 액션 타입 문자열을 생성합니다. 마지막으로, TypeScript와 잘 호환됩니다.

즉, _여러분_ 이 작성하는 코드는 크게 간단해질 수 있습니다. 예를 들어, 이전의 todos 리듀서는 아래와 같이 단순화될 수 있습니다.

```js title="features/todos/todosSlice.js"
import { createSlice } from '@reduxjs/toolkit'

const todosSlice = createSlice({
  name: 'todos',
  initialState: [],
  reducers: {
    todoAdded(state, action) {
      state.push({
        id: action.payload.id,
        text: action.payload.text,
        completed: false
      })
    },
    todoToggled(state, action) {
      const todo = state.find(todo => todo.id === action.payload)
      todo.completed = !todo.completed
    }
  }
})

export const { todoAdded, todoToggled } = todosSlice.actions
export default todosSlice.reducer
```

모든 액션 생성자와 액션 타입은 자동으로 생성되며, 리듀서 코드는 더 짧고 이해하기 쉬워집니다. 또한, 각 케이스별로 무엇이 업데이트 되는지 더 명확히 보입니다.

`configureStore`를 사용하여 스토어 설정을 간단하게 줄일 수 있습니다: 

```js title="app/store.js"
import { configureStore } from '@reduxjs/toolkit'
import todosReducer from '../features/todos/todosSlice'
import filtersReducer from '../features/filters/filtersSlice'

export const store = configureStore({
  reducer: {
    todos: todosReducer,
    filters: filtersReducer
  }
})
```

**`configureStore`를 한번 호출하면 기존에 수동으로 하던 모든 설정 작업이 자동으로 이루어집니다**:

- 슬라이스 리듀서가 자동으로 `combineReducers()`에 전달됩니다 
- `redux-thunk` 미들웨어가 자동으로 추가됩니다 
- 개발 모드 미들웨어가 추가되어, 실수로 상태를 변경하는 것을 방지합니다 
- Redux DevTools 확장 프로그램이 자동으로 설정됩니다 
- 미들웨어와 DevTools 인핸서가 결합되어 스토어에 추가됩니다 

동시에, **`configureStore` 는 이러한 기본 동작들을 수정할 수 있는 옵션도 제공합니다** (thunks를 끄거나 sagas를 추가하거나, 프로덕션 환경에서 DevTools를 비활성화하는 것과 같은 작업)

Redux Toolkit은 이 외에도, 다음과 같은 일반적인 Redux 작업을 수행할 수 있는 API를 제공합니다:

- `createAsyncThunk`: "비동기 요청 전후에 액션을 디스패치"하는 표준 패턴을 추상화합니다
- `createEntityAdapter`: 정규화된 상태에서 CRUD 작업을 수행하기 위한 미리 만들어진 리듀서와 셀렉터 
- `createSelector`: 메모이제이션된 셀렉터를 위한 표준 Reselect API 다시 내보내기(re-export) 
- `createListenerMiddleware`: 디스패치된 액션에 대한 응답으로 로직을 실행하기 위한 사이드 이펙트 미들웨어 

마지막으로, RTK 패키지에는 Redux앱에서 데이터를 패칭하고 캐싱하는 완벽한 솔루션인 "RTK Query"가 있습니다. RTK Query는 별도의 선택적 `@reduxjs/toolkit/query` 진입점으로 제공됩니다. 이는 엔드포인트 (REST, GraphQL 또는 기타 비동기 함수)를 정의하고, 데이터를 패칭하고, 로딩 상태를 업데이트하며 결과를 캐싱 하는 리듀서와 미들웨어를 생성합니다. 또한, 컴포넌트내에서 데이터를 패칭할 수 있는 React hooks도 자동으로 생성합니다. `const { data, isFetching} = useGetPokemonQuery('pikachu')` 와 같은 형태로 사용할 수 있습니다.

각 API들은 완전히 선택사항이며 특정 사용 사례를 위해 설계되었습니다. 즉, **사용자는 앱에서 실제로 사용할 API를 선택할 수 있습니다**. 그러나, 이 모든 API가 작업을 수행하는 데 있어 권장되는 방법입니다. 

**Redux Toolkit은 여전히 "Redux" 입니다!** 여전히 하나의 스토어가 있고, 상태를 불변하게 업데이트하는 리듀서와 업데이트를 위해 디스패치 된 액션 객체가 있으며, 비동기 로직을 위한 thunks를 작성할 수 있고, 정규화된 상태를 관리하며, TypeScript로 코드를 작성하고, DevTools를 사용할 수 있습니다. **동일한 결과를 얻기 위해 작성해야 하는 코드가 훨씬 적어졌을 뿐입니다!**

## Redux Toolkit을 사용하길 바라는 이유

Redux의 유지 관리자로서 우리의 의견은 다음과 같습니다:

:::팁

**우리는 _모든_ Redux 사용자들이 Redux Toolkit으로 Redux 코드를 작성하기를 원합니다. 코드를 단순화하고 많은 일반적인 Redux 실수와 버그를 제거하기 때문입니다!**

:::

이전 "보일러 플레이트"가 많고 복잡했던 Redux 패턴은 Redux의 _필수적인_ 부분이 아니었습니다. 이러한 패턴은 그저 다음과 같은 이유 때문에 존재했습니다:

- 최초의 "Flux Architecture"가 일부 같은 접근 방식을 사용했습니다 
- 초기 Redux 문서에는 코드를 타입별로 다른 파일에 분리할 수 있도록 액션 타입 상수와 같은 것들을 보여줬습니다 
- JavaScript는 기본적으로 가변 언어이며, 불변 업데트를 작성하려면 수동으로 객체 복사(spread)와 배열 업데이트가 필요합니다 
- Redux는 몇 주 안에 만들어졌으며 의도적으로 몇 가지 API 기본 요소로 구성되도록 설계되었습니다 

게다가, Redux 커뮤니티는 추가적인 보일러 플레이트를 더하는 몇 가지 특정한 접근 방식들을 채택해 왔습니다: 

- 부수효과를 작성하는 일반적인 접근 방식으로 `redux-saga` 미들웨어의 사용을 강조하는 것
- Redux 액션 객체에 대한 TS 타입을 수작업으로 작성하고 유니언 타입을 생성하여 타입 수준에서 디스패치할 수 있는 액션을 제한하는 것을 주장하는 것

여러 해 동안 저희는 실제로 사람들이 Redux를 어떻게 사용하는지 보았습니다. 수백 개의 추가 라이브러리를 작성하여 액션 타입과 생성자를 생성하고 비동기 로직과 부수 효과를 만들고 데이터를 패칭하는 작업을 하는 것을 보았습니다. 또한 저희는 사용자들에게 꾸준히 고통을 안겨주는 문제점들을 보았습니다. 예를 들어 상태를 실수로 변형하는 것, 단순한 상태 업데이트를 위해 수십 줄의 코드를 작성하는 것, 코드 베이스가 어떻게 통합되는지 추적하는 데 문제가 있었습니다. 저희는 Redux를 배우고 사용하려는 수천 명의 사용자들과, 이 모든 조각들이 어떻게 함께 동작하는지 이해하는 데 어려움을 겪으며, 많은 개념과 추가 코드의 양에 혼란스러워 하는 사용자들을 도왔습니다. 저희는 사용자들이 직면하는 문제를 _알고 있습니다_. 

**이러한 문제를 해결하도록 Redux Toolkit을 설계했습니다!**

- Redux Toolkit은 스토어 설정을 한번의 함수 호출로 단순화하면서, 필요시 스토어의 옵션을 완전히 구성할 수 있는 기능을 유지합니다 
- Redux Toolkit은 언제나 Redux버그의 #1 원인이었던 우발적인 상태 변이를 제거합니다 
- Redux Toolkit은 수동으로 액션 생성자 또는 액션 타입을 작성할 필요가 없게 만듭니다
- Redux Toolkit은 수동으로 작성해야하고 오류도 발생하기 쉬운 불변 업데이트 로직을 작성할 필요가 없습니다 
- Redux Toolkit은 Redux 기능의 코드를 여러 개의 분리된 파일에 분산하는 대신 단일 파일에 작성하기 쉽게 만들어줍니다 
- Redux Toolkit은 우수한 TypeScript 지원을 제공하며, 코드에서 정의해야하는 타입의 수를 최소화하고 높은 타입 안전성을 제공하기 위해 설계된 API를 제공합니다
- RTK Query를 사용하면 데이터를 패칭하고 로딩 상태를 추적하는 데 필요한 _어떠한_ thunks, 리듀서, 액션 생성자 또는 이펙트 훅도 작성하지 않아도 됩니다 

그렇기 때문에:

:::팁

**새로운 Redux 코드를 작성할 때 더 이상 예전의 `redux` 코어 패키지를 사용하지 않고, Redux Toolkit (`@reduxjs/toolkit` 패키지)을 사용하는 것을 권장합니다!**

:::

기존 애플리케이션의 경우에도, 적어도 `createStore` 를 `configureStore` 로 교체하는 것을 권장합니다. 개발 모드 미들웨어를 사용하면 기존 코드에서 발생할 수 있는 우연한 변이와 직렬화 오류를 잡을 수 있게 도와줍니다. 또한, 가장 많이 사용하는 리듀서(그리고 앞으로 작성할 리듀서들) 를 `createSlice`로 교체할 것을 권장합니다 - 코드가 더 짧고 이해하기 쉬워지며, 안정성이 향상되므로 앞으로의 시간과 노력을 절약할 수 있습니다.

**`redux` 코어 패키지는 여전히 작동하지만, 이제는 구식으로 여겨집니다**. `@reduxjs/toolkit`에서 redux 코어 패키지의 모든 API가 재내보내지며, `configureStore` 는 `createStore` 가 수행하는 모든 작업을 수행하지만 더 나은 기본 동작 및 구성을 제공합니다.

Redux Toolkit이 수행하는 작업을 더 잘 이해하기 위해 하위 개념을 이해하는 것이 유용합니다. 그래서 ["Redux Fundamentals" 튜토리얼에서는 추상화 없이 Redux가 어떻게 작동하는지 보여줍니다](../tutorials/fundamentals/part-1-overview.md). _그러나_, 튜토리얼은 예시들을 학습 도구로만 보여주며, Redux Toolkit이 예전에 수동으로 작성된 Redux 코드를 어떻게 단순화시키는지 보여주며 마무리합니다.

`redux` 코어 패키지를 단독으로 사용해도 코드는 계속 작동할 것입니다. **하지만, `@reduxjs/toolkit`로 전환하고 Redux Toolkit API를 사용하도록 코드를 업데이트하는 것을 강력히 권장합니다!**

## 추가 정보

자세한 내용은 다음 문서 페이지 및 블로그 게시물을 참조하세요. 

- [Redux Essentials: Redux Toolkit App Structure](../tutorials/essentials/part-2-app-structure.md)
- [Redux Fundamentals: Modern Redux with Redux Toolkit](../tutorials/fundamentals/part-8-modern-redux.md)
- [Redux Style Guide: Best Practices and Recommendations](../style-guide/style-guide.md)
- [Presentation: Modern Redux with Redux Toolkit](https://blog.isquaredsoftware.com/2022/06/presentations-modern-redux-rtk/)
- [Mark Erikson: Redux Toolkit 1.0 Announcement and development history](https://blog.isquaredsoftware.com/2019/10/redux-toolkit-1.0/)
