---
id: bindactioncreators
title: bindActionCreators
hide_title: true
description: 'API > bindActionCreators: wrapping action creators for dispatching'
---

&nbsp;

# `bindActionCreators(actionCreators, dispatch)`

값이 [액션 생산자](../understanding/thinking-in-redux/Glossary.md#액션-생산자)인 객체를 받아서, 같은 키를 가지지만 각각의 생산자들을 [`dispatch`](Store.md#dispatch)로 감싸서 바로 호출 가능하게 만든 객체로 바꿉니다.

보통은 [`Store`](Store.md) 인스턴스에서 바로 [`dispatch`](Store.md#dispatch)를 호출하면 됩니다. Redux를 React와 함께 사용한다면, [react-redux](https://github.com/gaearon/react-redux)가 [`dispatch`](Store.md#dispatch) 함수를 함께 제공하므로 바로 호출할 수 있습니다.

`bindActionCreators`의 유일한 사용처는, Redux를 상관하지 않는 컴포넌트로 액션 생산자를 넘기지만 [`dispatch`](Store.md#dispatch)나 Redux 저장소는 넘기고 싶지 않을 때입니다.

편의상 첫 번째 인수로 함수를 넘겼다가 함수를 반환 받을 수도 있습니다.

#### 인자

1. `actionCreators` (_Function_ or _Object_): [액션 생산자](../understanding/thinking-in-redux/Glossary.md#액션-생산자) 또는 값으로 액션 생산자들을 가지는 객체.

2. `dispatch` (_Function_): [`Store`](Store.md) 인스턴스에서 가져온 [`dispatch`](Store.md#dispatch) 함수.

#### 반환

(_Function_ or _Object_): 원래의 객체를 모사하지만 각각의 함수가 원래의 액션 생산자가 반환하는 액션을 바로 디스패치하는 객체. 만약 `actionCreators`로 함수를 넘겼다면, 반환값 역시 함수가 됩니다.

#### 예제

#### `TodoActionCreators.js`

```js
export function addTodo(text) {
  return {
    type: 'ADD_TODO',
    text
  }
}

export function removeTodo(id) {
  return {
    type: 'REMOVE_TODO',
    id
  }
}
```

#### `SomeComponent.js`

```js
import React from 'react'
import { bindActionCreators } from 'redux'
import { connect } from 'react-redux'

import * as TodoActionCreators from './TodoActionCreators'
console.log(TodoActionCreators)
// {
//   addTodo: Function,
//   removeTodo: Function
// }

function TodoListContainer(props) {
  // Injected by react-redux:
  const { dispatch, todos } = props

  // Here's a good use case for bindActionCreators:
  // You want a child component to be completely unaware of Redux.
  // We create bound versions of these functions now so we can
  // pass them down to our child later.

  const boundActionCreators = useMemo(
    () => bindActionCreators(TodoActionCreators, dispatch),
    [dispatch]
  )
  console.log(boundActionCreators)
  // {
  //   addTodo: Function,
  //   removeTodo: Function
  // }

  useEffect(() => {
    // Note: this won't work:
    // TodoActionCreators.addTodo('Use Redux')

    // You're just calling a function that creates an action.
    // You must dispatch the action, too!

    // This will work:
    let action = TodoActionCreators.addTodo('Use Redux')
    dispatch(action)
  }, [])

  return <TodoList todos={todos} {...this.boundActionCreators} />

  // An alternative to bindActionCreators is to pass
  // just the dispatch function down, but then your child component
  // needs to import action creators and know about them.

  // return <TodoList todos={todos} dispatch={dispatch} />
}

export default connect(state => ({ todos: state.todos }))(TodoListContainer)
```

#### 팁

- 왜 전통적인 Flux처럼 액션 생산자를 저장소 인스턴스에 바로 바인드하지 않는지 물을 수도 있습니다. 문제는 그 방법이 서버에서 렌더해야 하는 유니버설 앱에서는 잘 작동하지 않는다는겁니다. 보통은 매 요청마다 서로 다른 데이터로 저장소 인스턴스를 준비해야 하지만, 액션 생산자를 선언 중에 바인드하면 모든 요청에 대해 하나의 저장소 인스턴스 밖에 쓸 수 없습니다.

- ES5를 사용한다면 `import * as` 대신 `require('./TodoActionCreators')`를 `bindActionCreators`의 첫번째 인수로 넘기면 됩니다. 모듈 시스템과는 상관 없이 `actionCreators`의 인수 값이 함수이기만 하면 됩니다.
