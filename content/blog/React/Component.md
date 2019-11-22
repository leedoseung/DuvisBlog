---
title: Component
date: 2019-11-22 16:10:11
category: React
---

## Class Component

- State 기능
- Life cycle 기능
- 임의의 메서드

**render** 함수가 꼭 필요하다.

## Class Component VS Functional Component

### Functional Component 장점

- class 형 컴포넌트보다 선언하기 훨씬 쉽다.
- 메모리 자원도 class 형 컴퍼넌트보다 덜 사용한다.
- 빌드 후 배포할 때도 함수형 컴포넌트를 사용하는 것이 결과물의 크기가 더 작다.

### Functional Component 단점

- state와 라이프사이클 API의 사용 불가
  => 리액트 V16.9 업데이트 이후 Hooks라는 기능이 도입되면서 해결

리액트 공식 문서에서는 함수형 컴포넌트와 Hooks를 사용하도록 권장함

**Reactjs Code Snippet**를 사용하여 코드 생성하기
rsc TAB => 함수형 컴포넌트
rcc TAB => 클래스형 컴포넌트

## Props

### props 기본값 설정 : defaultProps

```js
import React from 'react'

const MyComponent = props => {
  return <div>안녕하세요, 제 이름은 {props.name}입니다.</div>
}

MyComponent.defaultProps = {
  name: '기본 이름',
}

export default MyComponent
```

### 태그 사이의 내용을 보여 주는 Children

```js
App.js

import React from 'react'
import MyComponent from './MyComponent'

const App = () => {
  return <MyComponent>리액트</MyComponent>
}

export default App
```

```js
MyComponent.js

import React from 'react'

const MyComponent = props => {
  return (
    <div>
      안녕하세요, 제 이름은 {props.name}입니다. <br />
      children 값은 {props.children}
      입니다.
    </div>
  )
}

MyComponent.defaultProps = {
  name: '기본 이름',
}

export default MyComponent
```
