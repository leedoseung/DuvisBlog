---
title: React-virtualized
date: 2019-12-04 15:54:10
category: React
---

## react-virtualized를 사용한 렌더링 최적화

리스트 컴포넌트에서 스크롤되기 전에 보이지 않는 컴포넌트는 렌더링 하지 않고 크기만 차지하게끔 할 수 있다.
그리고 만약 스크롤되면 해당 스크롤 위치에서 보여 주어야 할 컴포넌트를 자연스럽게 렌더링 시킨다. 이 라이브러리를 사용하면 낭비되는 자원을 아주 쉽게 아낄 수 있다.

### 설치

```sh

$ yarn add react-virtualized

```

react-virtualized 제공하는 List 컴포너트를 사용하여 TodoList 최적화

### TodoList 수정

```js
TodoList.js

import React, { useCallback } from 'react'
import { List } from 'react-virtualized'
import TodoListItem from './TodoListItem'
import './TodoList.scss'

const TodoList = ({ todos, onRemove, onToggle }) => {
  const rowRenderer = useCallback(
    ({ index, key, style }) => {
      const todo = todos[index]
      return (
        <TodoListItem
          todo={todo}
          key={key}
          onRemove={onRemove}
          onToggle={onToggle}
          style={style}
        />
      )
    },
    [onRemove, onToggle, todos]
  )

  return (
    <List
      className="TodoList"
      width={512} //전체넓이
      height={513} //전체높이
      rowCount={todos.length} // 항목 개수
      rowHeight={57} // 항목 높이
      rowRenderer={rowRenderer} // 항목을 렌더링할 때 쓰는 함수
      list={todos} //배열
      style={{ outline: 'none' }} // List에 기본 적용되는 outline 스타일 제거
    />
  )
}

export default React.memo(TodoList)
```

**rowRenderer** 함수는 react-virtualized의 List 컴포넌트에서 각 TodoItem을 렌더링할 때 사용,
index, key, style 값을 파라미터 값으로 받는다 . 객체타입
