---
title: Immer.js
date: 2019-12-05 14:00:10
category: React
---

## 불변성을 유지해주는 Immer.js

구조가 복잡한 객체를 전개 연사자를 사용하여 작성하는데는 복잡하고, 가독성도 떨어진다.
Immer 라이브러리를 사용하면, 구조가 복잡한 객체도 매우 쉽고 짧은 코드를 사용하여 불변성을 유지하면서 업데이트해 줄 수 있다.

### 설치하기

```sh
yarn add immer

```

### Immer 사용법

```js
import produce from 'immer'
const nextState = produce(originalState, draft => {
  //바꾸고 싶은 값 바꾸기
  draft.somewhere.depp.inside = 5
})
```

**예시코드**

```js
import produce from 'immer'

const originalState = [
  { id: 1, todo: '샬랄샬라라라라', checked: false },
  { id: 2, todo: '블라블라블라블', checked: true },
  { id: 3, todo: '크크크크크크크', checked: true },
]

const nextState = produce(originalState, draft => {
  // id가 2인 항목의 checked 값을 true로 설정
  const todo = draft.find(t => t.id === 2)
  todo.checked = true
  // 혹은 draft[1].checked = true;

  //배열에 새로운 데이터 추가
  draft.push({
    id: 3,
    todo: '키키키키키키',
    checked: false,
  })

  //id = 1인 항목을 제거하기

  draft.splice(
    draft.findIndex(t => t.id === 1),
    1
  )
})
```

```js
App.js

import React, { useRef, useState, useCallback } from 'react'
import produce from 'immer'

const App = () => {
  const nextId = useRef(1)
  const [form, setForm] = useState({ name: '', userName: '' })
  const [data, setData] = useState({
    array: [],
    uselessValue: null,
  })

  const onChange = useCallback(
    e => {
      const { name, value } = e.target
      setForm(
        produce(form, draft => {
          draft[name] = value
        })
      )
    },
    [form]
  )

  const onSubmit = useCallback(
    e => {
      e.preventDefault()
      const info = {
        id: nextId.current,
        name: form.name,
        userName: form.userName,
      }

      setData(
        produce(data, draft => {
          draft.array.push(info)
        })
      )

      setForm({
        name: '',
        userName: '',
      })

      nextId.current += 1
    },
    [data, form.name, form.userName]
  )

  const onRemove = useCallback(
    id => {
      setData(
        produce(data, draft => {
          draft.array.splice(
            draft.array.findIndex(t => t.id === id),
            1
          )
        })
      )
    },
    [data]
  )

  return (
    <div>
      <form onSubmit={onSubmit}>
        <input name="name" onChange={onChange} value={form.name} />
        <input name="userName" onChange={onChange} value={form.userName} />
        <button type="submit">등록</button>
      </form>
      <div>
        <ul>
          {data.array.map(info => (
            <li key={info.id} onClick={() => onRemove(info.id)}>
              {info.userName} ({info.name})
            </li>
          ))}
        </ul>
      </div>
    </div>
  )
}

export default App
```
