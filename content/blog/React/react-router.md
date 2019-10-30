---
title: React Router
date: 2019-10-30 16:10:11
category: Redux
---

# location

```js
{ 
    "pathname" : "/posts/3",
    "search" : "",
    "hash" : "",
    "key" : "xmsczi"
}

```

주로 search 값에서 URL Query를 읽는 데 사용하거나 주소가 바뀐 것을 감지하는 데 사용

주소가 바뀐 것을 감지하려면 다음과 같이 컴포넌트 라이프사이클 메서드에서 location을 비교

```js

componentDidUpdate(preProps, preState) {
    if(preProps.location !== this.props.location ) {
        //주소가 바뀜
    }
}

```

# match

<Route> 컴포넌트에서 설정한 path와 관련된 데이터들을 조회할 때 사용한다. 현재 URL이 같을지라도 다른 라우트에서 사용된 match는 다른 정보를 알려 준다.

match 객체는 주로 param를 조회하거나 서브 라우트를 만들 때 현재 path를 참조하는 데 사용

# history

현재 라우터를 조작할 때 사용. 예를 들어 뒤쪽 페이지로 넘어가거나, 다시 앞쪽 페이지로 가거나, 새로운 주소로 이동해야 할 때 이 객체가 지닌 함수들을 호출한다.

block 함수는 페이지에서 벗어날 때, 사용자에게 정말 페이지를 떠나겠냐고 묻는 창을 띄웁니다.

```js
const unblock = history.block('정말 떠나시겠습니까?');

// 막는 작업을 취소 할떄
unblock();

```

