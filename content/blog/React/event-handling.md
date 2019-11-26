---
title: Event Handling
date: 2019-11-25 22:11:52
category: React
---

## useState를 사용하여 함수형 컴포넌트 구현

```js
import React, { useState } from 'react'

const EventPractice = () => {
  const [form, setForm] = useState({
    username: '',
    message: '',
  })

  const { username, message } = form
  const onChange = e => {
    const nextForm = {
      ...form, //기존의 form 내용을 이 자리에 복사한 뒤
      [e.target.name]: e.target.value, // 원하는 값을 덮어 씌우기
    }

    setForm(nextForm)
  }

  const onClick = () => {
    setForm({
      username: '',
      message: '',
    })
  }

  const onKeyPress = e => {
    if (e.key === 'Enter') {
      onClick()
    }
  }

  return (
    <div>
      <h1>이벤트연습</h1>
      <input
        type="text"
        name="username"
        placeholder="사용자명"
        value={username}
        onChange={onChange}
      />
      <input
        type="text"
        name="message"
        placeholder="메시지"
        value={message}
        onChange={onChange}
        onKeyPress={onKeyPress}
      />

      <button onClick={onClick}>확인</button>
      <div>
        {username}
        {message}
      </div>
    </div>
  )
}

export default EventPractice
```

form객체를 사용하여 이벤트 핸들링 하자.
