---
title: Redux Middleware
date: 2019-12-19 16:10:11
category: Redux
---

리액트 웹 어플리케이션에서 API 서버를 연동할 때는 API 요청에 대한 상태도 잘 관리해야 한다. 예를 들어 요청이 시작되었을 떄는 로딩 중임을, 요청이 성공하거나 실패했을 때는 로딩이 끝났음을 명시해야 한다.

리액트 프로젝트에서 `Redux`를 사용하고 있으며 이러한 `비동기` 작업을 관리해야 한다면, `middleware`을 사용하여 매우 효율적이고 편하게 상태 관리 할 수 있다.

## 미들웨어 한?

`Redux Middleware`는 `Action`을 `Dispatch`했을 때 `Reducer`에서 이를 처리하기 앞서 사전에 지정된 작업들을 실행한다. `Middleware`은 `Action`과 `Reducer`의 중간자라고 볼 수 있다.

전달받은 `Action`을 단순히 콘솔에 기록하거나, 전달받은 `Action`정보를 기반으로 `Action`을 아예 취소하거나, 다른 종류의 `Action`을 추가로 디스패치할 수도 있다.

## 미들웨어 만들기

```js
const loggerMiddleware = store => next => action => {
  //미들웨어 기본 구조
}

export default loggerMiddleware
```

함수에서 파라미터로 받아 오는 `store`는 리덕스 스토어 인스턴스를, `action`은 디스패치된 액션을 가르킨다. `next`는 함수 형태이며, store.dispatch와 비슷한 역활을 한다. 하지만 큰 차이점이 있는데, next(action)을 호출하면 그다음 처리해야 할 미들웨어에게 액션을 넘겨주고, 만약 다음 미들웨어가 없다면 리듀서에게 액션을 넘겨 준다.

```js
const loggerMiddleware = store => next => action => {
  console.group(action && action.type)
  console.log('이전상태', store.getState())
  console.log('액션', action)
  next(action)
  console.log('다음 상태', store.getState())
  console.groupEnd()
}

export default loggerMiddleware
```

`next`를 사용하지 않으면 액션이 리듀서에 전달되지 않는다. 즉 액션이 무시되는 것이다.

```js
import React from 'react'
import ReactDOM from 'react-dom'
import { createStore, applyMiddleware } from 'redux'
import { Provider } from 'react-redux'
import './index.css'
import App from './App'
import rootReducer from './modules'
import loggerMiddleware from './lib/loggerMiddleware'

const store = createStore(rootReducer, applyMiddleware(loggerMiddleware))

ReactDOM.render(
  <Provider store={store}>
    <App />
  </Provider>,
  document.getElementById('root')
)
```

## redux-logger 사용하기

```sh
$ yarn add redux-logger

```

```js
import React from 'react'
import ReactDOM from 'react-dom'
import { createStore, applyMiddleware } from 'redux'
import { Provider } from 'react-redux'
import './index.css'
import App from './App'
import rootReducer from './modules'
import { createLogger } from 'redux-logger'

const logger = createLogger()

const store = createStore(rootReducer, applyMiddleware(logger))

ReactDOM.render(
  <Provider store={store}>
    <App />
  </Provider>,
  document.getElementById('root')
)
```
