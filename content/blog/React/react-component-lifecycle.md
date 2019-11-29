---
title: React Component Lifecycle
date: 2019-11-27 23:11:72
category: React
---

## 라이프사이클 메서드의 이해

**Will** 접두사가 붙은 메서드는 어떤 작업을 작동하기 **전**에 실행되는 메서드
**Did** 접두사가 붙은 메서드는 어떤 작업을 작동한 **후**에 실행되는 메서드

Life Cycle Category

1. Mount
2. Update
3. UnMount

**Mount**

DOM이 생성되고 웹 브라우저상에 나타나는 것은 Mount라고 한다.

<마운트할 때 호출하는 메서드>

컴포넌트 만들기 => constructor => getDerivedStateFromProps => render => componentDidMount

<ul>
    <li>constructor: 컴포넌트를 새로 만들 때마다 호출되는 클래스 생성자 메서드</li>
    <li>getDerivedStateFromProps: props에 있는 값을 state에 넣을 때 사용하는 메서드</li>
    <li>render: 우리가 준비한 UI를 렌더링하는 메서드</li>
    <li>componentDidMount: 컴포넌트가 웹 브라우저상에 나타난 후 호출하는 메서드</li>
</ul>

**Update**

아래 네 가지 경우에 업데이트

1. props가 바뀔 때
2. state가 바뀔 때
3. 부모 컴포넌트가 리렌더링될 때
4. this.forceUpdate로 강제로 렌더링을 트리거할 때

<업데이트할 때 호출하는 메서드>

업데이트를 발생기키는 요인 => getDerivedStateFromProps => shouldComponentUpdate => render => getSnapshotBeforeUpdate => componentDidUpdate

<ul>
    <li>shouldComponentUpdate: 컴포넌트가 리렌더링을 해야 할지 말아야 할지를 결정하는 메서드</li>
    <li>getSnapshotBeforeUpdate: 컴포넌트 변화를 DOM에 반영하기 바로 직전에 호출하는 메서드</li>
    <li>componentDidUpdate: 컴포넌트 업데이트 작업이 끝난 후 호출하는 메서드</li>
</ul>

**UnMount**

언마운트하기 => componentWillUnmount

componentWillUnmount : 컴포넌트가 웹 브라우저상에서 사라지기 전에 호출하는 메서드

## 라이프사이클 메서드 살펴보기

### render() 함수

이 메서드 안에서 this.props와 this.state에 접급할 수 있으며, 리액트 요소를 반환

**주의할점**

render 메서드 안에서는 이벤트 설정이 아닌 곳에 setState를 사용하면 안되며, 브라우저의 DOM에 접근해서도 안 된다.
DOM정보를 가져오거나 state에 변화를 줄 때는 componentDidMount에서 처리해야 한다.

### componentDidMount 메서드

```js
componentDidMount() { ... }

```

컴포넌트를 만들고, 첫 렌더링을 다 마친 후 실행한다. 이 안에서 다른 자바스크립트 라이브러리 또는 프레임워크의 함수를 호출하거나
이벤트 등록, setTimeout, setInterval, 네트워크 요청 같은 비동기 작업을 처리

### shouldComponentUpdate 메서드

```js
shouldComponentUpdate(nextProps, nextState) { ... }

```

props 또는 state를 변경했을 때, 리렌더링을 시작할지 여부를 지정하는 메서드.
이 메서드에서는 반드시 true or false 값을 반환해야 한다.

default는 true이며 false 값을 반환한다면 업데이트 과정은 중지

이 메서드 안에서 현재 props와 state는 this.props와 this.state로 접근하고,
새로 설정 될 props 또는 state는 nextProps와 nextState로 접근할 수 있다.

**최적화를 할때 사용하는 메서드**

### componentDidUpdate 메서드

```js
componentDidUpdate(prevProps, prevState, snapshot) { ... }

```

리렌더링을 완료한 후 실행.
업데이트 끝난 직후이므로, DOM관련 처리를 해도 무방.

### componentWillUnmount 메서드

컴포넌트를 DOM에서 제거할 때 실행.
componentDidMount에서 등록한 이벤트 타이머, 직접 생성한 DOM이 있다면 여기서 제거 작업을 해야 한다.

### componentDidCatch 메서드

컴포넌트 렌더링 도중에 에러가 발생했을 때 애플리케이션 먹통이 되지 않고 오류 UI를 보여 줄 수 있게 해준다.

```js
componenetDidCatch(error, info) {
    this.setState({
        error: true
    });
    console.log( { error, info });
}


```

```js
LifeCycleSample.js

import React, { Component } from 'react'

class LifeCycleSample extends Component {
  state = {
    number: 0,
    color: null,
  }

  myRef = null

  constructor(props) {
    super(props)
    console.log('constructor')
  }

  static getDerivedStateFromProps(nextProps, prevState) {
    console.log('getDerivedStateFromProps')
    if (nextProps.color !== prevState.color) {
      return { color: nextProps.color }
    }
    return null
  }

  componentDidMount() {
    console.log('componentDidMount')
  }

  shouldComponentUpdate(nextProps, nextState) {
    console.log('shouldComponentUpdate', nextProps, nextState)
    //숫자의 마지막 자리가 4명 리렌더링하지 않는다.
    return nextState.number % 10 !== 4
  }

  componentWillUnmount() {
    console.log('componentWillUnmount')
  }

  handleClick = () => {
    this.setState({
      number: this.state.number + 1,
    })
  }

  getSnapshotBeforeUpdate(prevProps, prevState) {
    console.log('getSnapshotBeforeUpdate')
    if (prevProps.color !== this.props.color) {
      return this.myRef.style.color
    }

    return null
  }

  componentDidUpdate(prevProps, prevState, snapshot) {
    console.log('componentDidUpdate', prevProps, prevState)
    if (snapshot) {
      console.log('업데이트되기 직전 색상: ', snapshot)
    }
  }

  render() {
    console.log('render')

    const style = {
      color: this.props.color,
    }

    return (
      <div>
        {this.props.missing.value}
        <h1 style={style} ref={ref => (this.myRef = ref)}>
          {this.state.number}
        </h1>
        <p>color: {this.state.color} </p>
        <button onClick={this.handleClick}>더하기</button>
      </div>
    )
  }
}

export default LifeCycleSample
```

```js
App.js

import React, { Component } from 'react'
import LifeCycleSample from './LifeCycleSample'
import ErrorBoundary from './ErrorBoundary'

function getRandomColor() {
  return '#' + Math.floor(Math.random() * 16777215).toString(16)
}

class App extends Component {
  state = {
    color: '#000000',
  }

  handleClick = () => {
    this.setState({
      color: getRandomColor(),
    })
  }
  render() {
    return (
      <div>
        <button onClick={this.handleClick}>랜덤 색상</button>
        <ErrorBoundary>
          <LifeCycleSample color={this.state.color} />
        </ErrorBoundary>
      </div>
    )
  }
}

export default App
```

```js
ErrorBoundary.js

import React, { Component } from 'react'

export class ErrorBoundary extends Component {
  state = {
    error: false,
  }

  componentDidCatch(error, info) {
    this.setState({
      error: true,
    })
    console.log({ error, info })
  }

  render() {
    if (this.state.error) return <div>에러가 발생했습니다!</div>
    return this.props.children
  }
}

export default ErrorBoundary
```

초기 렌더링 시

constructor -> getDerivedStateFromProps -> render -> componentDidMount

업데이트 시

getDerivedStateFromProps -> shouldComponentUpdate -> render -> getSnapshotBeforeUpdate -> componentDidUpdate
