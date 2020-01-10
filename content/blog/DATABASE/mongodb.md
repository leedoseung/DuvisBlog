---
title: MongoDB
date: 2020-01-08 16:36:69
category: DATABASE
---

## Mongo DB

기존에는 `RDBMS`(관계형 데이터베이스) MYSQL,OracleDB,PostgreSQL 사용

`RDBMS` 는 아래와 같은 한계가 있음.

1. 데이터 스키마가 고정적 이다.

스키마가 변경 되면 데이터 양이 많을 때 작업이 번거러워질 수 있다.

2. 확장성이 좋지않다.

`RDBMS`는 저장하고 처리해야 할 데이터양이 늘어나면 여러 컴퓨터에 분산시키는 것이 아니라,
해당 데이터베이스 성능을 업그레이드하는 방식으로 확장해 주어야 한다.

`MongoDB`는 이런 한계를 극복한 문서 지향적 `NoSQL` 데이터베이스 이다.
스키마가 유동적이고, 여러 컴퓨터로 분산 처리가 가능하다.

`MongoDB`무조건 `RDBMS`보다 좋은 것은 아니다.
상황별로 적합한 데이터베이스가 다를 수 있다.
예를 들어 데이터의 구조가 자주 바뀐다면 MongoDB가 유리하고, 까다로운 조건으로 데이터를 필터링 해야 하거나,
`ACID` 특성을 지켜야 한다면 `RDBMS`가 유리하다.

## 문서란?

여기서 말하는 `document`는 `RDBMS`의 `Record`와 개념이 비슷하다. 문서의 데이터 구조는 한 개 이상의 Key-Value 쌍으로 되어 있다.

```json
{
    "_id": ObjectId("2938482dfwe12231231"),
    "username": "duvis",
    "name": { first: "D.S." lase:"LEE"}
}
```

`document`는 `BSON`(바이너리 형태의 JSON) 형태로 저장된다. 그렇기 때문에 나중에 JSON 형태의 객체를 데이터베이스에 저장할 때,
큰 공수를 들이지 않고도 데이터를 데이터베이스에 등록할 수 있다.

\_Id는 고윳값으로 시간,머신 아이디, 프로세스 아이디, 순차 번호로 되어 있어 값의 고유함을 보장한다.

여러 문서가 들어 있는 곳은 `컬렉션`이라고 한다. 기존 `RDBMS`에서는 테이블 개념을 사용하므로 각 테이블마다 같은 스키마를 가지고 있어야 한다.
새로 등록해야 할 데이터가 다른 스키마를 가지고 있다면, 기존 데이터들의 스키마도 모두 바꾸어 주어야 한다.

반면 `MongoDB`는 다른 스키마를 가지고 있는 문서들이 한 컬렉션에서 공존할 수 있다.

## MongoDB 서버 준비

### 설치

**macOS**

```sh
$brew update
$brew install mongodb
$brew services start mongodb

```

**Windows**

Mongodb 홈페이지에서 설치파일 받아 설치
Complete를 선책하고 기본 설정으로 설치

### mongoose의 설치 및 적용

`mongoose`는 Node.js환경에서 사용하는 MongoDB기반 `ODM`(Object Data Modelling)라이브러리 이다. 이 라이브러리는 데이터베이스 문서들을 자바스크립트 객체처럼 사용할 수 있게 해준다.

```sh
yarn add mongoose dotenv

```

`dotenv`는 환경변수들을 파일에 넣고 사용할 수 있게 하는 개발 도구이다.
민감한 정보(서버주소나 계정 및 비밀번호)는 코드 안에 직접 작성하지 않고, 환경변수로 설정하는 것이 좋다.

`github`,`GitLab`등의 서비스에 올릴 때는 .gitignore를 작성하여 환경변수가 들어 있는 파일은 제외시켜 주어야 한다.

### .env 환경변수 파일 생성

```js
.env

PORT=4000
MONGO_URI=monggodb://localhost:27017/blog

```

```js
require('dotenv').config()

const { PORT } = process.env

const port = PORT || 4000
app.listen(port, () => {
  console.log('Listening to port %d', port)
})
```

### mongoose로 서버와 데이터베이스 연결

```js
const mongoose = require('mongoose')

const { PORT, MONGO_URI } = precess.env

mongoose
  .connect(MONGO_URI, { useNewUrlParser: true, useFindAndModify: false })
  .then(() => {
    console.log('Connected to MongoDB')
  })
  .catch(e => {
    consol.error(e)
  })
```
