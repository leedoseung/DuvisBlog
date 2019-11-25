---
title: state
date: 2019-11-25 13:10:11
category: React
---

## State ?

리액트에서 state는 컴포넌트 내부에서 바뀔 수 있는 값을 의미.
Props는 읽기전용. 값을 변경하려면 부모 컴포넌트에서 변경해야한다.

React에서는 두가지의 state가 있다.

1. 클래스형 컴포넌트가 지니고 있는 state
2. 함수형 컴포넌트에서 useState라는 함수를 통해 사용하는 state

### 클래스형 컴포넌트의 state

```js
import React from 'react'

class Counter extends Component {
  constructor(props) {
    super(props)
    this.state = {
      number: 0,
    }
  }

  render() {
    const { number } = this.state
    return (
      <div>
        <h1>{number}</h1>
        <button
          onclick={() => {
            this.setState({ number: number + 1 })
          }}
        >
          +1
        </button>
      </div>
    )
  }
}
```

### state를 constructor에서 꺼낼시

```js
import React from 'react'

class Counter extends Component {
    state = {
      number: 0,
    }

  render() {
    const { number } = this.state
    return (...
    )
  }
}
```

이 방식으로 선언하도록 하자.

### this.setState에 객체 대신 함수 인자 전달하기

```js

onclick = { () => {
  this.setState( { number : numbe r+ 1});
  this.setState( { number: this.state.number + 1});
}}


```

this.setState를 두 번 사용하는 것임에도 불구하고 +1만 됨.
this.setState를 두 번 사용한다고 해서 state값이 바로 바뀌지는 않기 때문이다.

해결책은 함수를 인자로 넣어 주는 것.

```js

//prevState 는 기존상태, props는 현재 지니고 있는 props
this.setState( (prevState, props) => {
  return(

  )
})

this.setState( (prevState) => {
  return(
    number : prevState.number + 1
  );
});

//위와 같음
this.setState( prevState => ({
    number:  prevState.number + 1
}));

```

### this.setState가 끝난 후 특정 작업 실행

```js
<button
  onClick={() => {
    this.state(
      {
        number: number + 1,
      },
      () => {
        console.log('State Call')
        console.log(this.state)
      }
    )
  }}
>
  +1
</button>
```

## 함수형 컴포넌트에서 useState 사용하기

리액트 버전 16.8 이후부터 가능 (React Hooks)

```js

Say.js

import React, { useState } from 'react';

const Say = () => {
  const [message, setMessage] = useState('');
  const onClickEnter = () => setMessage('Hello');
  const onClickLeave = () => setMessage('bye');

  return(
    <div>
      <button onClick={onClickEnter}>입장</button>
      <button onClcik={onCleckLeave}>퇴장</button>
    </div>
    <h1>{message}</h1>
  );

  export default Say;

}


```

### 한 컴포넌트에서 useState 여러 번 사용하기

```js

Say.js

import React, { useState } from 'react';

const Say = () => {
  const [message, setMessage] = useState('');
  const onClickEnter = () => setMessage('Hello');
  const onClickLeave = () => setMessage('bye');

  const [color, setColor ] = useState('black');


  return(
    <div>
      <button onClick={onClickEnter}>입장</button>
      <button onClcik={onCleckLeave}>퇴장</button>
    </div>
    <h1 style={{ color }}>{message}</h1>
    <button style={{ color : 'red' }}   onClick={ () => setColor('red')}></button>
    <button style={{ color : 'green' }} onClick={ () => setColor('green')}></button>
    <button style={{ color : 'blue' }}  onClick={ () => setColor('blue')}></button>
  );

  export default Say;

}


```

## state를 사용할 때 주의사항

1. setState or userState를 통해 전달받은 세터 함수를 사용할 것
2. 배열이나 객체를 업데이트 해야 할 경우 ... spread 연산자를 사용하여 사본을 만든 후 업데이트 후 적용

```js
//객체 다루기
const object = { a: 1, b: 2, c: 3 }
const nextObject = { ...object, b: 2 }

//배열 다루기
const array = [
  { id: 1, value: true },
  { id: 2, value: false },
  { id: 3, value: true },
]

let nextArray = array.concat({ id: 4 }) // 새 항목 추가
nextArray.filter(item => item.id !== 2) // id가 2인 항목 제거
nextArray.map(item => (item.id === 1 ? { ...item, value: false } : item)) // id가 1인 항목의 value를 false
```
