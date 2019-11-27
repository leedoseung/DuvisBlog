---
title: React Component Lifecycle
date: 2019-11-27 23:11:72
category: React
---

## 라이프사이클 메서드의 이해

**Will** 접두사가 붙은 메서드는 어떤 작업을 작동하기 **전**에 실행되는 메서드
**Did** 접두사가 붙은 메서드는 어떤 작업을 작동한 **후**에 실행되는 메서드

Life Cycle Category

1. Mount
2. Update
3. UnMount

**Mount**
DOM이 생성되고 웹 브라우저상에 나타나는 것은 Mount라고 한다.

<마운트할 때 호출하는 메서드>
컴포넌트 만들기 => constructor => getDerivedStateFromProps => render => componentDidMount

<ul>
    <li>constructor: 컴포넌트를 새로 만들 때마다 호출되는 클래스 생성자 메서드</li>
    <li>getDerivedStateFromProps: props에 있는 값을 state에 넣을 때 사용하는 메서드</li>
    <li>render: 우리가 준비한 UI를 렌더링하는 메서드</li>
    <li>componentDidMount: 컴포넌트가 웹 브라우저상에 나타난 후 호출하는 메서드</li>
</ul>

**Update**
아래 네 가지 경우에 업데이트

1. props가 바뀔 때
2. state가 바뀔 때
3. 부모 컴포넌트가 리렌더링될 때
4. this.forceUpdate로 강제로 렌더링을 트리거할 때

<업데이트할 때 호출하는 메서드>
업데이트를 발생기키는 요인 => getDerivedStateFromProps => shouldComponentUpdate => render => getSnapshotBeforeUpdate => componentDidUpdate

<ul>
    <li>shouldComponentUpdate: 컴포넌트가 리렌더링을 해야 할지 말아야 할지를 결정하는 메서드</li>
    <li>getSnapshotBeforeUpdate: 컴포넌트 변화를 DOM에 반영하기 바로 직전에 호출하는 메서드</li>
    <li>componentDidUpdate: 컴포넌트 업데이트 작업이 끝난 후 호출하는 메서드</li>
</ul>

**UnMount**
언마운트하기 => componentWillUnmount

componentWillUnmount : 컴포넌트가 웹 브라우저상에서 사라지기 전에 호출하는 메서드
