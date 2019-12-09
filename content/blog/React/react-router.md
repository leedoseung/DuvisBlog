---
title: React Router
date: 2019-10-30 16:10:11
category: React
---

## route

```js

<Route path="주소규칙" component={보여 줄 컴포넌트}>

```

```js
import React from 'react'
import { Route } from 'react-router-dom'
import Home from './Home'
import About from './About'

const App = () => {
  return (
    <div>
      <Route path="/" component={Home} exact={true}></Route>
      <Route path="/about" component={About}></Route>
    </div>
  )
}

export default App
```

`exact`라는 `props`를 `true`로 엄격한 설정이 가능

## multi Route

`React-router V5`부터 적용된 기능

```js

const App = () => {
    return (
        <Route path="/" component={Home} exact={true}>
        <Route path={['/about', '/info']} comopnent={About}>
    )
}

```

## location

```js
{
    "pathname" : "/about",
    "search" : "?detail=true",
    "hash" : "",
    "key" : "xmsczi"
}

```

주로 `search` 값에서 URL Query를 읽는 데 사용하거나 주소가 바뀐 것을 감지하는 데 사용

주소가 바뀐 것을 감지하려면 다음과 같이 컴포넌트 라이프사이클 메서드에서 `location`을 비교

`search` 값에서 특정 값을 읽어 오기 위해서는 이 문자열을 객체 형태로 변환해 주어야 한다. 쿼리 문자열을 객체로 변환할 때는 `qs`라는 라이브러리를 사용한다.

```sh

$ yarn add qs

```

```js
About.js

import React from 'react'
import qs from 'qs'

const About = ({ location }) => {
  const query = qs.parse(location.search, {
    ignoreQueryPrefix: true, // 이 설정을 통해 문자열 맨 앞의 ?를 생략한다.
  })

  const showDetail = query.detail === 'true'
  return (
    <div>
      <h1>About</h1>
      <p>이 프로젝트는 리액트 라우터 기초를 실습해 보는 예제 프로젝트입니다.</p>
      {showDetail && <p>detail 값을 true로 설정하셨군요!</p>}
    </div>
  )
}

export default About
```

![](./capture.png)

```js

componentDidUpdate(preProps, preState) {
    if(preProps.location !== this.props.location ) {
        //주소가 바뀜
    }
}

```

## link

`Link` 컴포넌트는 클릭하면 다른 주소로 이동시켜 주는 컴포넌트이다.
이 컴포넌트를 사용하요 페이지를 전환하면, 페이지를 새로 불러오지 않고 애플리케이션은 그대로 유지한 상태에서 `HTML5 History API`를 사용하여 페이지의 주소만 변경해 준다.

```js
import React from 'react'
import { Route, Link } from 'react-router-dom'
import Home from './Home'
import About from './About'

const App = () => {
  return (
    <div>
      <ul>
        <li>
          <Link to="/">Home</Link>
        </li>
        <li>
          <Link to="/about">About</Link>
        </li>
      </ul>
      <Route path="/" component={Home} exact={true}></Route>
      <Route path="/about" component={About}></Route>
    </div>
  )
}

export default App
```

## match

`<Route>`컴포넌트에서 설정한 path와 관련된 데이터들을 조회할 때 사용한다. 현재 URL이 같을지라도 다른 라우트에서 사용된 `match`는 다른 정보를 알려 준다.

`match` 객체는 주로 `param`를 조회하거나 서브 라우트를 만들 때 현재 path를 참조하는 데 사용

```js
Profile.js

import React from 'react'

const data = {
  duvis: {
    name: '이두승',
    description: '리액트를 사랑하는 개발자',
  },
  gopas: {
    name: '김덕배',
    description: '덕질을 사랑하는 일반인',
  },
}

const Profile = ({ match }) => {
  const { username } = match.params
  const profile = data[username]

  if (!profile) {
    return <div>존재하지 않는 사용자입니다.</div>
  }
  return (
    <div>
      <h3>
        {username} ({profile.name})
      </h3>
      <p>{profile.description}</p>
    </div>
  )
}

export default Profile
```

```js
App.js

import React from 'react'
import { Route, Link } from 'react-router-dom'
import Home from './Home'
import About from './About'
import Profile from './Profile'

const App = () => {
  return (
    <div>
      <ul>
        <li>
          <Link to="/">Home</Link>
        </li>
        <li>
          <Link to="/about">About</Link>
        </li>
        <li>
          <Link to="/profile/duvis">Duvis Profile</Link>
        </li>
        <li>
          <Link to="/profile/gopas">Gopas Profile</Link>
        </li>
      </ul>
      <Route path="/" component={Home} exact={true}></Route>
      <Route path="/about" component={About}></Route>
      <Route path="/profile/:username" component={Profile}></Route>
    </div>
  )
}

export default App
```

## history

현재 라우터를 조작할 때 사용. 예를 들어 뒤쪽 페이지로 넘어가거나, 다시 앞쪽 페이지로 가거나, 새로운 주소로 이동해야 할 때 이 객체가 지닌 함수들을 호출한다.

`block` 함수는 페이지에서 벗어날 때, 사용자에게 정말 페이지를 떠나겠냐고 묻는 창을 띄웁니다.

```js
const unblock = history.block('정말 떠나시겠습니까?')

// 막는 작업을 취소 할떄
unblock()
```
