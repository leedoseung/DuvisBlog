---
title: NodeJS Koa Framework
date: 2019-10-13 00:10:11
category: NodeJS
---


# Koa

Koa 프레임워크는 Express의 기존 개발 팀이 소유권을 IBM에 넘기기 전부터 개발해 오던 프로젝트. 이 프로젝트는 Express를 리펙토링한 결과물.
Koa는 Express보다 훨씬 가볍고 , Node v7.6부터 정식으로 지원하는 async/await문법을 아주 편하게 사용할 수 있다. 콜백 지옥에서 벗어날 수 있다.


```bash

$ yarn add koa


```

hello world 띄우기

```js

const Koa = require('koa');

const app = new Koa();

app.use((ctx) => {
    ctx.body = 'Hello World';
});

app.listen(4000, () => {
    console.log('listening to port 4000');
});

```

# 미들웨어

Koa의 미들웨어 함수에서는 두 가지 파라미터를 받느다. 첫 번째는 ctx이며, 두 번째는 next이다.

ctx는 웹 요청과 응답 정보를 지닌다. 그리고 next는 현재 처리 중인 미들웨어의 다음 미들웨어를 호출하는 함수이다. 미들웨어를 등록하고 next를 실행하지 않으면 그다음 미들웨어를 처리하지 않는다.

## next()는 프로미스다.

```js

app.use((ctx, next) => {
    console.log(1);
    next().then( () => {
        console.log('bye');
    });
});


```

## async/await 

```js

app.use(async (ctx, next) => {
    console.log(1);
    await next();
    console.log('bye');
});

```

