---
title: Asynchronous processing model
date: 2019-09-28 22:09:92
category: Javascript
---

동기식 처리 모델(Synchronous processing model)은 직렬적으로 태스크를 수행한다. 즉, 태스크는 순차적으로 실행되며 어떤 작업이 수행 중이면 다음 작업은 대기하게 된다.

```js

function task1(){
    console.log('task 1');
    task2();
}

function task2(){
    console.log('task 2');
    task3();
}

function task3(){
    console.log('task 3')
}

task1();

```


비동기식 처리 모델(Asynchronous processing model 또는 Non-Blocking processing model)은 병렬적으로 태스크를 수행한다.즉, 태스크가 종료되지 않은 상태라 하더라도 대기하지 않고 다음 태스크를 실행한다. 예를 들어 서버에서 데이터를 가져와서 화면에 표시하는 태스크를 수행할 때, 서버에 데이터를 요청한 이후 서버로부터 데이터가 응답될 때까지 대기하지 않고(Non-Blocking) 즉시 다음 태스크를 수행한다. 이후 서버로부터 데이터가 응답되면 이벤트가 발생하고 이벤트 핸들러가 데이터를 가지고 수행할 태스크를 계속해 수행한다.

자바스크립트의 대부분의 DOM 이벤트 핸들러와 Timer 함수(setTimeout, setInterval), Ajax 요청은 비동기식 처리 모델로 동작한다.

```js

function task1(){
    console.log('task 1');
    task2();
}

function task2(){
    setTimeout(function(){
        console.log('task 2');
    },0);

    task3();
}

functin task3(){
    console.log('task 3');
}

task1();

//task 1
//task 3
//task 2 

```


함수 task1이 호출되면 함수 task1은 Call Stack에 쌓인다. 그리고 함수 task1은 함수 task2을 호출하므로 함수 task2가 Call Stack에 쌓이고 setTimeout가 호출된다. setTimeout의 콜백함수는 즉시 실행되지 않고 지정 대기 시간만큼 기다리다가 “tick” 이벤트가 발생하면 태스크 큐로 이동한 후 Call Stack이 비어졌을 때 Call Stack으로 이동되어 실행된다.

