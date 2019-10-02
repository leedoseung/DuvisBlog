---
title: Extended Parameter Handling 
date: 2019-10-02 22:10:74
category: es6
---

## 매개변수 기본값 (Default Parameter value)

```js

function sum(x, y) {
    
    x = x || 0;
    y = y || 0;

    return x + y ;
}

console.log(sum(1)); // 1
console.log(sum(1, 2);) // 3

```


```js

function sum(x = 0, y = 0) {
    return x + y;
}

console.log(sum(1)); // 1
console.log(sum(1, 2);) // 3


```

## Rest 파라미터

Rest 파라미터는 함수에 전달된 인수들의 목록을 배열로 전달 받는다
argument와 같지만 argument는 유사배열이다.
```js

function foo(...rest){
    console.log(Array.isArray(rest)); //true
    console.log(rest); // [1, 2, 3, 4, 5]
}

foo(1, 2, 3, 4, 5);

```


함수에 전달된 인수들은 순차적으로 파라미터와 Rest 파라미터에 할당

```js

function foo(param, ...rest) {
    console.log(param); //1
    console.log(rest); // [2, 3, 4, 5]
}

foo(1, 2, 3, 4, 5);

function bar(param1, param2, ...rest){
    console.log(param1); // 1
    console.log(param2); // 2
    console.log(rest);   //[3, 4, 5]
}

bar(1, 2, 3, 4, 5);

```

**Rest 파라미터는 반드시 마지막 파라미터이어야 한다**

```js

function foo( ...rest, param1, param2){}

foo(1, 2, 3, 4, 5);
// SyntaxError: Rest Parameter must be last formal parameter


```


### arguments와 rest파라미터

ES5에서는 인자의 개수를 사전에 알 수 없는 가변 인자 함수의 경우, arguments 객체를 통해 인수를 확인한다. arguments 객체는 함수 호출 시 전달된 인수들의 정보를 담고 있는 순회가능한(iterable) 유사 배열 객체(array-like object)이며 함수 내부에서 지역 변수처럼 사용할 수 있다.

> arguments 프로퍼티는 일부 브라우저에서 지원하고 있지만 ES3부터 표준에서 deprecated 되었다
> Function.argument 사용방법은 권장하지 않으며 함수 내부에서 지역변수처럼 사용

```js

// ES5
var foo = function () {
    console.log(arguments);
};


foo(1, 2, 3, 4); // { '0' : 1 , '1' : 2, '2' : 3, '3' : 4 }


```


aguments 객체는 유사배열 객체이므로 배열 메소드를 사용하려면 Function.prototype.call을 사용해야하는 번거러움이 있음

```js

//ES5
function sum() {
     /*
    가변 인자 함수는 arguments 객체를 통해 인수를 전달받는다.
    유사 배열 객체인 arguments 객체를 배열로 변환한다.
    */
    var array = Array.prototype.silce.call(arguments);
    return array.reduce(function (pre, cur) {
        return pre + cur;
    });
}


console.log(sum(1, 2, 3, 4, 5)); //15

```


```js

//ES6

function sum(...args) {
    console.log(arguments); // Arguments(5) [1, 2, 3, 4, 5, callee: (...), Symbol(Symbol.iterator): ƒ]
    console.log(Array.isArray(args));
    return args.reduce((pre, cur) => pre + cur );
}

console.log(sum(1, 2, 3, 4, 5));


```

ES6 화살표 함수 객체의 arguments 프로퍼티가 없다. 따라서 rest 파라미터를 사용해야 한다.

```js

var normalFunc = function () {};
console.log(normalFunc.hasOwnProperty('arguments')); // true

const arrowFunc = () => {};
console.log(arrowFunc.hasOwnProperty('arguments')); // false


```






