---
title: Context API
date: 2019-12-11 21:12:13
category: React
---

`Context API`는 리액트 프로젝트에서 `전역적`으로 사용할 데이터가 있을 때 유용한 기능이다.
`사용자 로그인 정보`,`애플리케이션 환경 설정`,`테마`등

Context API는 `React v16.3`부터 사용하기 쉽게 개선되었다.

## Context API를 사용한 전역상태 관리 흐름 이해하기

전역적으로 필요한 상태를 관리해야 할때는 어떻게 해야 될까?

리액트 애플리케이션은 컴포넌트 간의 데이터를 `props`로 전달하기 때문에 컴포넌트 여기저기에 필요한 데이터가 있을 때는 주로 최상위 컴포넌트 App의 `state`에 넣어 관리한다.

하지만 복잡한 애플리케이션에서 상태 업데이트 함수를 최상단에서 최하단까지 많은 단계를 거쳐 전달 하여야 한다.
이렇게 되면 유지 보수성이 낮아질 가능성이 있다.

이런 단점 때문에 나온 라이브러리가 `Redux`와 `Mobx`등 이다.

`React v16.3` 업데이트 이후에는 `Context API`가 많이 개선되었기 때문에 별도의 라이브러리를 사용하지 않아도 전역 상태를 손쉽게 관리할 수 있다.

## Context API 사용법

### 새 Context 만들기

```js
import { createContext } from 'react'

const ColorContext = createContext({ color: 'black' })

export default ColorContext
```

### Consumer 사용하기

```js
import React from 'react'
import ColorContext from '../contexts/color'

const ColorBox = () => {
  return (
    <ColorContext.Consumer>
      {value => (
        <div
          style={{
            width: '64px',
            height: '64px',
            background: value.color,
          }}
        />
      )}
    </ColorContext.Consumer>
  )
}
```

`Consumer` 사이에 중괄호를 열어서 그 안에 함수를 넣어 주었다. 이러한 패턴을 `Function as a child` 혹은 `Render Props`라고 한다.
컴포넌트의 children이 있어야 할 자리에 일반 `JSX` 혹은 문자열이 아닌 함수를 전달 하는 것

**Render Props** 예제

```js
import React from 'react'

const RenderPropsSample = ({ children }) => {
  return <div>결과: {children(5)}</div>
}

export default RenderPropsSample
```

만약 위와 같은 컴포넌트가 있다면 추후 사용할 때 다음과 같이 사용할 수 있다.

```js
<RenderPropsSample>{value => 2 * value}</RenderPropsSample>
```

## Provider

`Provider`를 사용하면 `Context`의 value를 변경할 수 있다.

```js
import React from 'react'
import ColorBox from './components/ColorBox'
import ColorContext from './contexts/color'

const App = () => {
  return (
    <ColorContext.Provider value={{ color: 'red' }}>
      <div>
        <ColorBox />
      </div>
    </ColorContext.Provider>
  )
}

export default App
```

`Provider`를 사용할 때는 value 값을 명시해 주어야 제대로 작동한다는 것을 꼭 기억할 것!
