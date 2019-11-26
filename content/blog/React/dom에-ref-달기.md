---
title: DOM에 Ref 달기
date: 2019-11-26 22:11:75
category: React
---

## DOM에 Ref 달기

DOM ID를 직접 달아 컨트롤 하는 방법은 피하자.
컴포넌트를 여러 곳 에서 사용 할 경우 문제가 발생하기 때문.

```js
//id를 부여하지 않고 state를 사용하여 처리 할 수 있다.
import React, { Component } from 'react'
import './ValidationSample.css'

export class ValidationSample extends Component {
  state = {
    password: '',
    clicked: false,
    validated: false,
  }

  handleOnChange = e => {
    this.setState({
      password: e.target.value,
    })
  }

  handleButtonClick = () => {
    this.setState({
      password: '',
      clicked: true,
      validated: this.state.password === '0000',
    })
    this.input.focus()
  }
  render() {
    return (
      <div>
        <input
          //input 에 ref를 사용
          ref={ref => (this.input = ref)}
          type="text"
          onChange={this.handleOnChange}
          className={
            this.state.clicked
              ? this.state.validated
                ? 'success'
                : 'failure'
              : ''
          }
        />
        <button onClick={this.handleButtonClick}>검증하기</button>
      </div>
    )
  }
}

export default ValidationSample
```

Scrollbox 맨밑으로 내리기 예제

```js
ScrollBox.js

import React, { Component } from 'react'

export class ScrollBox extends Component {
  scrollToBottom = () => {
    const { scrollHeight, clientHeight } = this.box

    this.box.scrollTop = scrollHeight - clientHeight
  }

  render() {
    const style = {
      border: '1px solid black',
      width: '300px',
      height: '300px',
      overflow: 'auto',
      display: 'relative',
    }

    const innerStyle = {
      width: '100%',
      height: '600px',
      background: 'linear-gradient(white, black)',
    }

    return (
      <div
        style={style}
        ref={ref => {
          this.box = ref
        }}
      >
        <div style={innerStyle} />
      </div>
    )
  }
}

export default ScrollBox
```

```js
App.js

class App extends Component {
  render() {
    return (
      <div>
        <ScrollBox ref={ref => (this.scrollBox = ref)} />
        <button onClick={() => this.scrollBox.scrollToBottom()}>
          Go To Bottom
        </button>
      </div>
    )
  }
}
export default App
```

함수형 컴포넌트에서 ref는 useRef라는 Hook 함수를 쓴다.
