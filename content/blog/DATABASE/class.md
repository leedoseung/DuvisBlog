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
