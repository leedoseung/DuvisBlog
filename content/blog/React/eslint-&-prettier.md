---
title: EsLint & Prettier
date: 2020-03-05 19:03:87
category: react
---

# CRA 에서 ESLint 와 Prettier 설정하기

`CRA` 기반으로 ESLint 와 Prettier 설정방법

## ESLint

`Javascript`의 느슨한 타입과 유연함은 장점이기도 하지만 올바르게 개발하지 않는다면 그만큼 실수를 유발하기도 한다. 특히, 여러 사람이 같이 개발하는 경우 구성원내에서 지켜야 할 스타일 가이드나 코딩 컨벤션이 필수다.

`ESLint`는 개발자들이 지켜야 할 규칙들을 만들수 있도록 해주고 규칙을 지켜서 코딩하는지 체크해 준다.

### Install EsLint

`CRA`를 통해 프로젝트 생성 시 ESLint 가 기본으로 포함되어 있으므로 별도 설치하지 않아도 된다.

### ESLint Extension 설치

`VSCODE` 에디터에서 바로바로 결과를 볼 수 있도록 하려면 ESLint Extension을 설치

### ESLint Setting

- `.eslintrc`.\* 파일을 이용한 설정 방법
- `package.json` 파일 내의 eslintConfig를 이용한 설정 방법

.eslintrc.\* 파일은 Javascript, JSON, YAML 형태로 작성 가능하고
프로젝트 루트 디렉토리에 위치한다.

```json

.eslintrc.json

{
  "extends": "react-app"
}

```

## Prettier

`Prettier`는 파일 저장 시점이나 Git에 커밋할 때 코드를 자동으로 포멧팅 해줌으로서 일관된 코딩 형태를 유지해준다.

`Prettier`은 포맷팅 기능 담당

`ESLint`는 코딩 컨벤션 처리 담당

### Prettier 설치

```shell

npm install --save-dev --save-exact prettier


```

# Prettier Setting

`ESLint`와 마찬가지로 두 가지 설정 방법을 제공한다

- `.prettierrc` 같은 설정 파일을 이용한 설정 방법
- `package.json` 파일 내의 `prettier`를 이용한 설정 방법

```json
{
  "trailingComma": "es5",
  "tabWidth": 4,
  "semi": false,
  "singleQuote": true
}
```

## Combine ESLint and Prettier

`ESLint`로는 스타일을 체크하고 `Prettier`로는 포멧팅을 처리를 하려면 두 개의 패키지를 설치해야 한다

```shell

npm i --save-dev eslint-config-prettier eslint-plugin-prettier

```

위에 만든 .eslintrc.json 파일을 다음과 같이 변경

```json
{
  "extends": ["react-app", "prettier"],
  "plugins": ["prettier"],
  "rules": {
    "prettier/prettier": ["error"]
  }
}
```

## ESLint-config-airbnb

`ESLint`의 규칙들을 일일이 설정하기 보다는 잘 되있는 airbnb 쓴다

```shell

nsx install -peerdeps --dev eslint-config-airbnb

```

```json

.eslintrc.json

{
  "extends": ["react-app", "prettier", "airbnb"],
  "plugins": ["prettier"],
  "rules": {
    "prettier/prettier": ["error"],
    "react/jsx-filename-extension": 0
  }
}

```
