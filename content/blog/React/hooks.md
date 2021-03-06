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

## useMemo

함수형 컴포넌트 내부에서 발생하는 연산을 최적화할 수 있다. 먼저 리스트에 숫자를 추가하면 추가된 숫자들의 평균을 보여주는 함수형 컴포넌트를
작성해 보자

```js
Average.js

import React, { useState } from 'react'

const getAverage = number => {
  console.log('Calculating average value...')
  if (number.length === 0) return 0
  const sum = number.reduce((a, b) => a + b)
  return sum / number.length
}

const Average = () => {
  const [list, setList] = useState([])
  const [number, setNumber] = useState('')

  const onChange = e => {
    setNumber(e.target.value)
  }

  const onInsert = e => {
    const nextList = list.concat(parseInt(number))
    setList(nextList)
    setNumber('')
  }

  return (
    <div>
      <div>
        <input value={number} onChange={onChange} />
        <button onClick={onInsert}>등록</button>
      </div>
      <ul>
        {list.map((value, index) => (
          <li key={index}>{value}</li>
        ))}
      </ul>
      <div>
        <b>평균값 : </b> {getAverage(list)}
      </div>
    </div>
  )
}

export default Average
```

숫자를 등록할 때뿐만 아니라 인풋 내용이 수정될 때도 getAverage 함수가 호출되는 것을 확인할 수 있다.
인풋 내용이 바뀔 때는 평균값을 다시 계산할 필요가 없다.

useMemo Hook을 사용하면 이러만 작업을 최적화할 수 있다. 렌더링하는 과정에서 특정 값이 바뀌었을 때만 연상을 수행하고,
원하는 값이 바뀌지 않았다면 이전에 연산했던 결과를 다시 사용하는 방식이다.

```js
Average.js

import React, { useState, useMemo } from 'react'

const getAverage = number => {
  console.log('Calculating average value...')
  if (number.length === 0) return 0
  const sum = number.reduce((a, b) => a + b)
  return sum / number.length
}

const Average = () => {
  const [list, setList] = useState([])
  const [number, setNumber] = useState('')

  const onChange = e => {
    setNumber(e.target.value)
  }

  const onInsert = e => {
    const nextList = list.concat(parseInt(number))
    setList(nextList)
    setNumber('')
  }

  //추가
  const avg = useMemo(() => getAverage(list), [list])

  return (
    <div>
      <div>
        <input value={number} onChange={onChange} />
        <button onClick={onInsert}>등록</button>
      </div>
      <ul>
        {list.map((value, index) => (
          <li key={index}>{value}</li>
        ))}
      </ul>
      <div>
        <b>평균값 : </b> {avg}
      </div>
    </div>
  )
}

export default Average
```

## useCallback

useCallback은 useMemo와 상당히 비슷한 함수.
주로 렌더링 성능을 최적해야 하는 상황에서 사용한다. 이 Hook을 사용하면 이벤트 핸들러 함수를 필요할 때만 생성할 수 있다.
위에 onChange, onInser라는 함수는 컴포넌트가 리렌더링될 때마다 이 함수들이 생성된다. 컴포넌트의 렌더링이 자주 발생하거나 렌더링해야 할 컴포넌트의 개수가 많이지면 이 부분을 최적해 주는 것이 좋다.

```js
Average.js


import React, { useState, useMemo, useCallbck } from 'react'

const getAverage = number => {
  console.log('Calculating average value...')
  if (number.length === 0) return 0
  const sum = number.reduce((a, b) => a + b)
  return sum / number.length
}

const Average = () => {
  const [list, setList] = useState([])
  const [number, setNumber] = useState('')

  const onChange = useCallback(e => {
    setNumber(e.target.value)
  },[]); // 컴포넌트가 처음 렌더링할 때만 함수 생성

  const onInsert = useCallback(e => {
    const nextList = list.concat(parseInt(number))
    setList(nextList)
    setNumber('')
  }),[ number, list]; // number 혹은 list가 바뀌었을 때만 함수 생성

  //추가
  const avg = useMemo(() => getAverage(list), [list])

  return (
    <div>
      <div>
        <input value={number} onChange={onChange} />
        <button onClick={onInsert}>등록</button>
      </div>
      <ul>
        {list.map((value, index) => (
          <li key={index}>{value}</li>
        ))}
      </ul>
      <div>
        <b>평균값 : </b> {avg}
      </div>
    </div>
  )
}

export default Average

```

useCallback(function, []) 두 번째 파라미터 배열에는 어떤 값이 바뀌었을 때 함수를 생성해야 하는지 명시해야 한다.

비여 있는 배열을 넣게 되면 컴포넌트가 렌더링될 때 단 한 번만 함수가 생성되며
onInsert같이 배열에 number, list를 넣게 되면 인풋 내용이 바뀌거나 새로운 항목이 추가될 때마다 함수가 생성된다.

## useRef

함수형 컴포넌트에서 ref를 쉽게 사용할 수 있도록 해준다.
Average 컴포넌트에서 **등록**버튼을 눌렀을 때 포커스가 인풋 쪽으로 넘어가도록 하는 코드

```js

Average.js


import React, { useState, useMemo, useCallbck, useRef } from 'react'

const getAverage = number => {
  console.log('Calculating average value...')
  if (number.length === 0) return 0
  const sum = number.reduce((a, b) => a + b)
  return sum / number.length
}

const Average = () => {
  const [list, setList] = useState([])
  const [number, setNumber] = useState('')
  const inputEl = useRef(null);

  const onChange = useCallback(e => {
    setNumber(e.target.value)
  },[]); // 컴포넌트가 처음 렌더링할 때만 함수 생성

  const onInsert = useCallback(e => {
    const nextList = list.concat(parseInt(number))
    setList(nextList);
    setNumber('');
    inputEl.current.focus();
  }),[ number, list]; // number 혹은 list가 바뀌었을 때만 함수 생성

  //추가
  const avg = useMemo(() => getAverage(list), [list])

  return (
    <div>
      <div>
        <input value={number} onChange={onChange} ref={inputEl} />
        <button onClick={onInsert}>등록</button>
      </div>
      <ul>
        {list.map((value, index) => (
          <li key={index}>{value}</li>
        ))}
      </ul>
      <div>
        <b>평균값 : </b> {avg}
      </div>
    </div>
  )
}

export default Average

```

### 로컬 변수 사용하기

추가로 컴포넌트 로컬 변수를 사용해야 할 때도 useRef를 활용할 수 있다. 로컬 변수란 렌더링과 상관없이 바뀔 수 있는 값을 의미한다.

class 형 로컬변수

```js

import React, { Component } from 'react';

class MyComponent extends Component {
  id = 1;
  setId = (n) => {
    this.id = n ;
  }

  printId = () => {
    console.log(this.id);
  }

  ...
}

```

함수 형 로컬변수

```js

import React, { useRef } from 'react';

class MyComponent extends Component {
  id = useRef(1);
  setId = (n) => {
    id.current = n;
  }

  printId = () => {
    console.log(id.current);
  }

  ...
}

```

이렇게 ref 안의 값이 바뀌어도 컴포넌트가 리렌더링되지 않는다는 점에 주의.

## 커스텀 Hooks 만들기

```js
useInput.js

import { useReducer } from 'react'

function reducer(state, action) {
  return {
    ...state,
    [action.name]: action.value,
  }
}

export default function useInputs(initialForm) {
  const [state, dispatch] = useReducer(reducer, initialForm)

  const onChange = e => dispatch(e.target)

  return [state, onChange]
}
```

```js
info.js

import React from 'react'
import useInputs from './useInputs'

const info = () => {
  const [state, onChange] = useInputs({
    name: '',
    nickName: '',
  })

  const [name, nickName] = state

  ...
}
```
