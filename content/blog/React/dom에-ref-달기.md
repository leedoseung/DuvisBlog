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
