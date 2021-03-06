---
title: Redux 상태관리
date: 2019-12-16 16:10:11
category: Redux
---

`Redux` 라이브러리는 전역 상태를 관리할 때 굉장히 효과적이다.
물론 리덕스를 사용하는 것이 유일한 해결책은 아니다. `Context API`를 통해서도 똑같은 작업을 할 수 있다.
`React V1.63`이 릴리즈되면서 `Context API`가 개선되기 전에는 사용방식이 매우 불편했기 때문에 주로 리덕스를 사용해 전역 상태를 관리 했다.

단순히 전역 상태 관리만 한다면 `Context API`를 사용하는 것만으로도 충분하다. 하지만 규모가 큰 프로젝트에서는 리덕스가 효율적이다.
코드의 유지 보수성도 높여 주고 작업 효율도 극대화해 주기 때문. 개발자 도구도 지원하며, 미들웨어라는 기능을 제공하여 비동기 작업을 효율적으로 관리할 수 있게 해준다.

## 개념 미리 정리

### 액션 (Action)

상태에 어떠한 변화가 필요하게 될 땐, 액션을 발생시킴

```js
{
  type: 'Increase'
}
```

액션 객체는 `Type` 값은 필수, 그외는 개발자 마음대로

```js

{
    type: "Increase" ,
    data: {
        number : 0 ,
        text : '더하기'
    }
}

```

### 액션 생성함수 (Action Creator)

액션을 만드는 함수. 단순히 파리미터를 받아와서 액션 객체 형태로 만들어줌

### 리듀서 (Reducer)

변화를 일으키는 함수. 리듀서는 두가지의 파라미터를 받아옴

```js

const reducer = (state, action) => alertedState ;

}

```

현재의 상태와, 전달 받은 액션을 참고하여 새로운 상태를 만들어서 반환

### Store

리덕스에서는 한 애플리케이션 당 하나의 스토어를 가짐. 스토어 안에는 앱 상태, 리듀서가 들어가 있고, 추가적으로 몇가지 내장 함수들이 있음

### Dispatch

스토어의 내장함수. 액션을 발생 시키는 것
`dispatch(action)` 호출 하면 스토어는 리듀서 함수를 실행, 해당 액션을 처리하는 로직이 있다면 액션을 참고하여 새로운 상태 만듬

### Subscribe

스토어 내장 함수 중 하나. 함수 형태의 값을 파라미터로 받아옴
`subscribe` 함수에 특정 함수를 전달해주면, 액션이 디스패치 되었을 때 마다 전달해준 함수가 호출됨

# Redux의 3가지 규칙

1. 하나의 애플리케이션 안에는 하나의 스토어가 있다.
2. 상태는 읽기전용이다.
3. 변화를 일으키는 함수, 리듀서는 순수한 함수여야 한다.

- 리듀서 함수는 이전 상태(state) 와 , 액션 객체를 파라미터를 받는다
- 이전 상태는 건들지 않고, 변화를 일으킨 새로운 상태 객체를 만들어서 반환한다.
- 똑같은 파라미터로 호출된 리듀서 함수는 언제나 똑같은 결과값을 반환해야만 한다.
