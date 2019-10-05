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


## Spread 문법

Spread 문법(Spread Syntax, ...)는 대상을 개별 요소로 분리한다. Spread문법의 대상은 이터러블 이어야 한다

```js

// ...[1, 2, 3]는 [1, 2, 3]을 개별 요소로 분리한다(→ 1, 2, 3)
console.log(...[1, 2, 3]); // 1, 2, 3

// 문자열은 이터러블이다
console.log(...'Hello'); // H e l l o

//Map 과 Set은 이터러블이다.
console.log(...new Map([['a', '1'], ['b', '2']]));// [ 'a', '1' ] [ 'b', '2' ]
console.log(...new Set([1, 2, 4])) // 1 2 4

//이터러블이 아닌 일반 객체는 Spread 문법의 대상이 될 수 없다.
console.log(... {a: 1, a: 2});
// TypeError: Found non-callable @@iterator



```


### 함수의 인수로 사용하는 경우

```js

// ES6
function foo(x, y, z) {
  console.log(x); // 1
  console.log(y); // 2
  console.log(z); // 3
}

// 배열을 foo 함수의 인자로 전달하려고 한다.
const arr = [1, 2, 3];

/* ...[1, 2, 3]는 [1, 2, 3]을 개별 요소로 분리한다(→ 1, 2, 3)
   spread 문법에 의해 분리된 배열의 요소는 개별적인 인자로서 각각의 매개변수에 전달된다. */
foo(...arr);


```

Rest 파리미터와 Spread 문법을 혼동하지 말자

Rest 파라미터는 반드시 마지막 파라미터이어야 하지만 Spread 문법을 사용한 인수는 자유롭게 사용할 수 있다.

```js

//ES6

function foo(v, w, x, y, z) {
    console.log(v);
    console.log(w);
    console.log(x);
    console.log(y);
    console.log(z);
}

foo(1, ...[2, 3], 4, ...[5]);

```

### 배열에서 사용하는 경우



```js

//Concat
const arr = [1, 2, 3];

console.log([...arr, 4, 5, 6]);

//Push

const arr1 = [1, 2, 3];
const arr2 = [4, 5, 6];

arr1.push(...arr2);
console.log(arr1);


//Splice
const arr1 = [1, 2, 3, 6];
const arr2 = [4, 5];

arr1.splice(3,0, ...arr2);

console.log(arr1);


//Copy

const arr = [1, 2, 3];

const copy = [...arr];

console.log(copy);

copy.push(4);

console.log(copy); // [ 1, 2, 3, 4 ]

// arr은 변경되지 않는다.
console.log(arr);  // [ 1, 2, 3 ]


//원본 배열의 각 요소를 얕은 복사(shallow copy)하여 새로운 복사본을 생성한다

const todos = [
  { id: 1, content: 'HTML', completed: false },
  { id: 2, content: 'CSS', completed: true },
  { id: 3, content: 'Javascript', completed: false }
];

// shallow copy
// const _todos = todos.slice();
const _todos = [...todos];
console.log(_todos === todos); // false

// 배열의 요소는 같다. 즉, 얕은 복사되었다.
console.log(_todos[0] === todos[0]); // true

```

>Spread 문법과 Object.assign는 원본을 shallow copy한다.


Spread 사용법은 사용하여 유사 배열 객체를 배열로 손쉽게 변경할 수 있다

```js

const htmlCollection = document.getElementByTagName('li');

conse newArry = [...hemlCollection];


// ES6의 Array.from 메소드를 사용할 수도 있다.
// const newArray = Array.from(htmlCollection);


```

## Rest/Spread 프로퍼티

 - 2019년 5월 현재 Rest/Spread 프로퍼티는 TC39 프로세스의 stage 4(Finished) 단계이다.
 - 2019년 1월 현재 객체 리터럴 Rest/Spread 프로퍼티를 Babel로 트랜스파일링하려면 @babel/plugin-proposal-object-rest-spread 플러그인을 사용해야 한다.

```js

//Spread 프로퍼티
const n = { x: 1, y: 2, ...{ a: 3, b: 4 } }; // { x:1, y:2, a:3, b:4}

//Rest 프로퍼티
const { x, y, ...z } = n;
console.log(x, y, z); // 1 2 { a: 3, b: 4}


```

```js

// 객체의 병합
const merged = { ...{ x: 1, y: 2 }, ...{ y: 10, z: 3 } };
console.log(merged); // { x: 1, y: 10, z: 3 }

// 특정 프로퍼티 변경
const changed = { ...{ x: 1, y: 2 }, y: 100 };
// changed = { ...{ x: 1, y: 2 }, ...{ y: 100 } }
console.log(changed); // { x: 1, y: 100 }

// 프로퍼티 추가
const added = { ...{ x: 1, y: 2 }, z: 0 };
// added = { ...{ x: 1, y: 2 }, ...{ z: 0 } }
console.log(added); // { x: 1, y: 2, z: 0 }


// 객체의 병합(assign)
const merged = Object.assign({}, { x: 1, y: 2 }, { y: 10, z: 3 });
console.log(merged); // { x: 1, y: 10, z: 3 }

// 특정 프로퍼티 변경
const changed = Object.assign({}, { x: 1, y: 2 }, { y: 100 });
console.log(changed); // { x: 1, y: 100 }

// 프로퍼티 추가
const added = Object.assign({}, { x: 1, y: 2 }, { z: 0 });
console.log(added); // { x: 1, y: 2, z: 0 }

```

