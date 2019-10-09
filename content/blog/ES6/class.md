---
title: Class
date: 2019-10-10 00:10:69
category: ES6
---

자바스크립트는 프로토타입 기반 객체지향 언어이다.

프로토타입 기반 프로그래밍은 클래스가 필요없는 객체지향 프로그래밍 스타일로 프로토타입 체인과 클로저 등으로 객체지향 언어의 상속, 캡슐화 등의 개념을 구현할 수 있다.

```js
//ES5

var Person = (function(){
    //Constructor
    function Person(name){
        this._name = name;
    }

    //public method
    Person.prototype.sayHi = function() {
        console.log('Hello' + this_name);
    };

    //return constructor
    return Person;
}());


var me = new Person('Duvis');
me.sayHi();

console.log(me instance of Person); // true

```

Java 개발자인 내게 프로토타입 기반 프로그래밍 방식은 좀 난해했다.

```js
//ES6

class Person {
    
    constructor(name) {
        this._name = name;
    }

    sayHi() {
        console.log(`Hello ${this._name}`);
    }
}

const me = new Person('Duvis');
me.sayHi();

console.log(me instance of Person); // true

```

## Constructor

constructor은 인스턴스를 생성하고 클래스 필드를 초기화하기 위한 특수한 메소드 이다.

> 클래스 필드(class field)
> 클래스 내부의 캡슐화된 변수. 데이터 멤버 또는 멤버 변수하고도 부름
> 자바스크립트의 생성자 함수에서 this에 추가한 프로퍼티를 클래스 기반 객체지향 언어에서는 클래스 필드라 부름

```js

// 클래스 선언문
class Person {

  constructor(name) {
    // this는 클래스가 생성할 인스턴스를 가리킨다.
    // _name은 클래스 필드이다.
    this._name = name;
  }
}

// 인스턴스 생성
const me = new Person('Duvis');
console.log(me); // Person {_name: "Duvis"}


```