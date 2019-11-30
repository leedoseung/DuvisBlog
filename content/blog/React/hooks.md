---
title: Hooks
date: 2019-11-28 23:11:10
category: React
---

## useState

가장 기본적인 Hook 상태관리를 해준다.

```js
import React, { useState } from 'react'

const Info = () => {
  const [form, setForm] = useState({
    name: '',
    nickName: '',
  })

  const { name, nickName } = form

  const onChange = e => {
    const nextForm = {
      ...form,
      [e.target.name]: e.target.value,
    }

    setForm(nextForm)
  }

  return (
    <div>
      <div>
        <input type="text" value={name} onChange={onChange} name="name" />
        <input
          type="text"
          value={nickName}
          onChange={onChange}
          name="nickName"
        />
      </div>
      <div>
        <b>이름:</b>
        {name}
      </div>
      <div>
        <b>닉네임:</b>
        {nickName}
      </div>
    </div>
  )
}

export default Info
```

## useEffect

리액트 컴포넌트가 렌더링될 때마다 특정 작업을 수행하도록 설정할 수 있는 Hook
클래스형 컴포넌트 componentDidMount 와 componentDidUpdate를 합친 형태

```js
useEffect(() => {
  console.log('redering Complate')
  console.log({
    name,
    nickName,
  })
})
```

### 마운트될 때만 실행하고 싶을 때

```js
useEffect(() => {
  console.log('redering Complate')
}, [])
```

### 특정 값이 업데이트될 때만 실행하고 싶을 때

기존 class형 Componenent

```js

componentDidUpdate(prevProps, prevState) {
    if(prevProps !== this.props.value){
        doSomething();
    }
}

```

useEffect 사용

```js
useEffect(() => {
  console.log(name)
}, [name])
```

### 뒷정리하기

useEffect는 기본적으로 렌더링되고 난 직후마다 실행되며, 두 번째 파라미터 배열에 무엇을 넣는지에 따라 실행되는 조건이 달라진다.
컴포넌트가 unmount 전이나 업데이트되기 직전에 어떠한 작업을 수행하고 싶다면 뒷정리 cleanup 함수를 반환해 주어야 한다.

```js
useEffect(() => {
  console.log('effect')
  console.log(name)
  return () => {
    console.log('cleanUp')
    console.log(name)
  }
})
```

## useReducer

리듀서는 현재 상태, 그리고 업데이트를 위해 필요한 정보를 담은 액션(action)값을 전달 받아 새로운 상태를 반환하는 함수이다.
리듀서 함수에서 새로운 상태를 만들 때는 반드시 불변성을 지켜 주어야 한다.

```js
Counter.js

import React, { useReducer } from 'react'

function reducer(state, action) {
  switch (action.type) {
    case 'INCREMENT':
      return { value: state.value + 1 }
    case 'DECREMENT':
      return { value: state.value - 1 }
    default:
      return state
  }
}
const Counter = () => {
  const [state, dispatch] = useReducer(reducer, { value: 0 })

  return (
    <div>
      <p>
        현재 카운트 값은 <b>{state.value}</b> 입니다.
      </p>
      <button onClick={() => dispatch({ type: 'INCREMENT' })}>PLUS</button>
      <button onClick={() => dispatch({ type: 'DECREMENT' })}>MINUS</button>
    </div>
  )
}

export default Counter
```

```js
Info.js

import React, { useReducer } from 'react'

function reducer(state, action) {
  return {
    ...state,
    [action.name]: action.value,
  }
}

const Info = () => {
  const [state, dispatch] = useReducer(reducer, {
    name: '',
    nickName: '',
  })

  const { name, nickName } = state

  const onChange = e => dispatch(e.target)

  return (
    <div>
      <div>
        <input name="name" onChange={onChange} value={name}></input>
        <input name="nickName" onChange={onChange} value={nickName}></input>
      </div>
      <div>
        <div>
          <b>이름:</b> {name}
        </div>
        <div>
          <b>닉네임:</b> {nickName}
        </div>
      </div>
    </div>
  )
}

export default Info
```
