---
title: Arrow Function
date: 2019-10-01 21:10:23
category: ES6
---

## 화살표 함수의 선언

```js

//매개변수 지정 방법

    () => { ... } //매개변수가 없을 경우
     x => { ... } //매개변수가 한 개인 경우, 소괄호를 생략할 수 있다.
(x, y) => { ... } // 매개변수가 여러 개인 경우, 소괄호 생략 불가

x => { return x * x}
x => x * x //두개 동일한 표현

() => { return { a: 1} ; }
() => ({a : 1}) //위 표현과 동일. 객체 반환시 소괄호를 사용

() => {
    const x = 10;
    return x * x;
};

```


## 화살표 함수의 호출

```js

//ES5

var arr = [1, 2, 3];
var pow = arr.map(function (num) {
    return num * num ;
});

console.log(pow);

```

```js

//ES6
const arr = [1, 2, 3];
const pow = arr.map(num => num * num);

console.log(pow); 

```


## this

function 키워드로 생성한 일반 함수와 화살표 함수의 가장 큰 차이점은 this이다


### 일반함수의 this
함수를 선언할 때 this에 바인딩할 객체가 정적으로 결정되는 것이 아니고, **함수를 호출할 때 함수가 어떻게 호출되었는지에 따라** this에 바인딩할 객체가 동적으로 결정된다.

콜백 함수 내부의 this는 전역 객체 window를 가리킨다.

```js

function Prefixer(prefix) {
    this.prefix = prefix;
}

Prefixer.prototype.prefixArray = function (arr) {

    return arr.map(function (x) {

        // 콜백함수의 this 는 전역 객체 this를 가리킴
        return this.prefix + '' + x ;
    });
};

var pre = new Prefixer('Hi');
console.log(pre.prefixerArray(['Lee', 'Kim']));


```

```js

function Prefixer(prefix) {
    this.prefix = prefix;
}

Prefixer.prototype.prefixArray = function (arr) {

    return arr.map(function (x) {
        //map(func, this)
        // this: Prefixer 생성자 함수의 인스턴스
        return this.prefix + '' + x ;
    }, this);
};

var pre = new Prefixer('Hi');
console.log(pre.prefixerArray(['Lee', 'Kim']));


```

```js

function Prefixer(prefix) {
    this.prefix = prefix;
}

Prefixer.prototype.prefixArray = function(arr) {
    //this는 상위 스코프인 prefixArray 메소드 내의 this를 가르킴
    return arr.map( x => `${this.prefix}  ${x}` );
}

const pre = new Prefixer('Hi');
console.log(pre.prefixArray(['Lee', 'Kim']));

```

## 화살표 함수를 사용해서는 안되는 경우
1. 메소드
2. prototype
3. 생성자 함수
4. addEventListener 함수의 콜백함수