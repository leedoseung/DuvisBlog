---
title: graphql
date: 2019-11-12 23:11:43
category: GraphQL
---

# GraphQL

GraphQL은 API 쿼리 언어이며, 데이터 유형을 정의하여 쿼리하기 위한 서버 런타임 시스템이다.

RESTFUL API VS GraphQL

RESTFUL API은 Request마다 각각의 Endpoint를 사용
GraphQL은 단일 Endpoint만 사용

RESTFUL API에서 발생하는 Over-Fetching, Under-Fetching 문제를 해결

## Over-Fetching 이란?
필요한 정보 영역보다 더 많은 정보를 서버로부터 받는 것

## Under-Fetching 이란?
필요한 정보가 여러개일때 서버에 여러번 요청해야한다.

# 필요 NPM 모듈 설치

## GraphQL 설치

graphql-yoga 설치

```bash

$ yarn add graphql-yoga


```

## Babel 설치

```bash

$ yarn global add babel-cli

$ yarn add babel-preset-env babel-preset-stage-3 --dev

```

.bablerc 파일 생성

```js
{
    "presets": ["env", "stage-3"]
}

```

## nodemon 설치

nodemon은 js가 수정되면 서버를 자동으로 재시작해줌

```bash

$ yarn  global add nodemon

```

package.json 추가

```json

 "scripts": {
     "start" : "nodemon --exec babel-node index.js"
 }

```

# index.js 파일 생성하고 실행

index.js

```js
import { GraphQLServer } from 'graphql- yoga'

const server = new GraphQLServer({})
server.start( () => console.log(' Server is running localhost:4000'));

```

서버 실행하면, 어직은 에러 발생

```bash

$ yarn start

```


# Scheme 생성

scheme는 쿼리에 사용할 데이터 유형을 정의.

폴더구조
```
graphql
  └─ scheme.graphql

```

scheme.graphql에서 type을 추가

```js

type Query {
    name: String!
}

```
name은 요청값, 응답유형은 String, !은 필수값(required)

# resolvers 생성

파일구조
```
graphql
├─ scheme.graphql
└─ resolvers.js

```

resolvers.js에서는 Query를 위한 resolvers를 작성.
name으로 요청 시 응답값을 리턴

```js

export const resolvers = {
    Query : {
        name : `Duvis`,
    }
}


```

index.js 파일에 typeDefs, resolvers 추가

```js
import { GraphQLServer } from 'graphql- yoga';
import resolvers from './graphql/resolvers';

const server = new GraphQLServer({
    typeDefs: "graphql/scheme.graphql",
    resolvers
})
server.start( () => console.log(' Server is running localhost:4000'));

```

