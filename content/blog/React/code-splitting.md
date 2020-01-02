---
title: Code Splitting
date: 2020-01-02 11:00:10
category: React
---

Create app 에 기본 탑재된 `SplitChunks` 기능을 통한 코드 스플리팅은 단순히 효율적인 캐싱 효과 만 있을 뿐이다.

예를 들어 페이지 A, B, C로 구성된 싱글 페이지 애플리케이션을 개발한다고 가정해 보자.
사용자가 A 페이지에 방문했다면 B 페이지와 C 페이지에서 사용하는 컴포넌트 정보는 필요하지 않는다. 사용자가 실제적으로 B, C 페이지에 이동하려고 할 때만 필요하다.

리액트 프로젝트에서 별도로 설정하지 않으면 A, B, C 컴포넌트에 대한 코드가 모두 한 파일에(Main)에 저장되어 버린다.

이러면 로딩이 오래 걸리고 사용자 경험도 안좋아지고 트래픽도 많이 나오게 된다.

이러한 문제점을 해결해 줄 수 있는 방법이 바로 `코드 비동기 로딩`이다.
이 또한 `code splitting` 방법 중 하나이다.

`코드 비동기 로딩`을 통해 자바스크립트 함수, 객체, 혹은 컴포넌트를 처음에 불러오지 않고 필요한 시점에 불러와서 사용할 수 있다.

## Javascript 함수 비동기 로딩

```js
notify.js

export default function notify() {
  alert('noti!!')
}
```

```js
App.js

import React from 'react'
import logo from './logo.svg'
import './App.css'
import notify from './notify'

function App() {
  const onclick = () => {
    notify()
  }
  return (
    <div className="App">
      <header className="App-header">
        <img src={logo} className="App-logo" alt="logo" />
        <p onClick={onclick}>Hello React!</p>
      </header>
    </div>
  )
}

export default App
```

이렇게 코드를 작성하고 빌드하면 notify 코드가 main 파일 안에 들어가게 된다.
하지만 다음과 같이 import를 상단에서 하지 않고 import() 함수 형태로 메서드 안에서 사용하면, 파일을 따로 분리시켜서 저장한다. 그리고 실제 함수가 필요한 지점에 파일을 불러와서 함수를 사용할 수 있다.

```js
import React from 'react'
import logo from './logo.svg'
import './App.css'

function App() {
  const onclick = () => {
    import('./notify').then(result => result.default())
  }
  return (
    <div className="App">
      <header className="App-header">
        <img src={logo} className="App-logo" alt="logo" />
        <p onClick={onclick}>Hello React!</p>
      </header>
    </div>
  )
}

export default App
```

import 함수를 사용하면 `promise`를 반환한다.
이렇게 import를 함수로 사용하는 문법은 비록 아직 표준 자바스크립트가 아니지만, stage-3 단계에 있는 `dynamic import`라는 문법이다.

## React.laze 와 Suspense를 통한 컴포넌트 코드 스플리팅

리액트에 내장된 기능으로 유틸 함수인 `React.lazy`와 컴포넌트인 `Suspense`가 있다.
이 기능은 `React 16.6` 버전부터 도입되었다.

`React.lazy`는 컴포넌트를 렌더링하는 시점에서 비동기적으로 로딩할 수 있게 해주는 유틸 함수이다.

```js
const SplitMe = React.lazy(() => import('./SplitMe'))
```

`Suspense`는 리액트 내장 컴포넌트로서 코드 스플리팅된 컴포넌트를 로딩하도록 발동시킬 수 있고, 로딩이 끝나지 않았을때 보여 줄 UI를 설정할 수 있다.

```js
import React, { Suspense } from 'react';

(...)
<Suspense fallback={<div>loading...</div>}>
    <SplitMe />
</Suspense>

```

`Suspense`에서 `fallback props`를 통해 로딩 중에 보여줄 JSX를 지정할 수 있다.

```js
import React, { useState, Suspense } from 'react'
import logo from './logo.svg'
import './App.css'

const SplitMe = React.lazy(() => import('./SplitMe'))

function App() {
  const [visible, setVisible] = useState(false)
  const onClick = () => {
    setVisible(true)
  }
  return (
    <div className="App">
      <header className="App-header">
        <img src={logo} className="App-logo" alt="logo" />
        <p onClick={onClick}>Hello React!</p>
        <Suspense fallback={<div>Loading...</div>}>
          {visible && <SplitMe />}
        </Suspense>
      </header>
    </div>
  )
}

export default App
```

## Loadable Components를 통한 코트 스플리팅

`Loadable Components`는 코드 스플리팅을 편하게 도와주는 서드파티 라이브러리 이다.
이 라이브러리의 이점은 **서버 사이드 렌더링**을 지원한다는 것이다.
또한. 렌더링하기 전에 필요할 때 스플리팅된 파일을 미리 불러올 수 있는 기능도 있다.

`server side rendering`이란 웹 서비스의 초기 로딩 속도 개선, 캐싱 및 검색 엔진 최적화를 가능하게 해 주는 기술이다.

`server side rendering`을 사용하면 웹 서비스의 초기 렌더링을 사용자의 브라우저가 아닌 **서버 쪽에서** 처리한다.

사용자는 서버에서 랜더링한 html 결과물을 받아 와서 그대로 사용하기 때문에 초기 로딩 속도도 개선되고, 검색 엔진에서 `크롤링`할 때도 문제가 없다.

라이브러리 설치

```sh
$ yarn add @lodable/component
```

```js
import React, { useState } from 'react'
import logo from './logo.svg'
import './App.css'
import loadable from '@lodable/component'

const SplitMe = loadable(() => import('./SplitMe'), {
  fallback: <div>loading...</div>,
})

function App() {
  const [visible, setVisible] = useState(false)
  const onClick = () => {
    setVisible(true)
  }
  return (
    <div className="App">
      <header className="App-header">
        <img src={logo} className="App-logo" alt="logo" />
        <p onClick={onClick}>Hello React!</p>
        {visible && <SplitMe />}
      </header>
    </div>
  )
}

export default App
```

이번에는 컴포넌트를 미리 불러오는(preload)방법을 알아보자.

```js
import React, { useState } from 'react'
import logo from './logo.svg'
import './App.css'
import loadable from '@loadable/component'

const SplitMe = loadable(() => import('./SplitMe'), {
  fallback: <div>loading...</div>,
})

function App() {
  const [visible, setVisible] = useState(false)
  const onClick = () => {
    setVisible(true)
  }
  const onMouseOver = () => {
    SplitMe.preload()
  }
  return (
    <div className="App">
      <header className="App-header">
        <img src={logo} className="App-logo" alt="logo" />
        <p onClick={onClick} onMouseOver={onMouseOver}>
          Hello React!
        </p>
        {visible && <SplitMe />}
      </header>
    </div>
  )
}

export default App
```
