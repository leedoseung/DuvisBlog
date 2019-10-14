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
## 적적 메소드

static 키워드를 사용한다.
정적메소드는 클래스의 인스턴스가 아닌 클래스 이름으로 호출한다.

```js

class Azo {
    constructor(prop) {
        this.prop = prop;
    }

    static staticMethod(){
        /* 
        정적 메소드는 this를 사용할 수 없다.
        정적 메소드는 내부에서 this는 클래스의 인스턴스가 아닌 클래스 자신을 가리킨다.
         */
        return 'staticMethod'
    }

    prototypeMethod(){
        return this.prop;
    }

    console.log(Azo.staticMethod());

    const azo = new Azo(232);

    // 정적 메소드는 인스턴스로 호출할 수 없다  
    console.log(azo.staticMethod());// Uncaught TypeError: foo.staticMethod is not a function
}


```

클래스의 정적 메소드는 인스턴스로 호출할 수 없음.
this를 사용할 수 없다.
메소드 내부에서 this를 사용할 필요가 없는 메소드를 만들때 쓰인다.
어플리케이션 전역에서 사용하는 유틸리티 함수를 생성할 때 주로 사용.

## 클래스 상속
클래스 상속은 코드 재사용 관점에서 매우 유용.

### extends 키워드

```js

class Circle {
    constructor(radius){
        this.radius = radius; //반지름
    }

    //원의 지름
    getDiameter(){
        return 2 * this.radius;
    }
    //원의 둘레
    getPerimeter(){
        return 2* Math.PI * this.radius
    }
    //원의 넓이
    getArea(){
        return Math.PI * Math.pow(this.radius, 2);
    }

    class Cylinder extends Circle {
        constructor(radius, height) {
            super(radius);
            this.height = height;
        }

        getArea() {
            // (원통의 높이 * 원의 둘레) + (2 * 원의 넓이)
            return (this.height * super.getPerimeter()) + (2* super.getArea());
        }

        getVolume() {
            // 원통의 부피
            return super.getArea() * this.height;
        }

    }

    // 반지름이 2, 높이가 10인 원통
const cylinder = new Cylinder(2, 10);

// 원의 지름
console.log(cylinder.getDiameter());  // 4
// 원의 둘레
console.log(cylinder.getPerimeter()); // 12.566370614359172
// 원통의 넓이
console.log(cylinder.getArea());      // 150.79644737231007
// 원통의 부피
console.log(cylinder.getVolume());    // 125.66370614359172

// cylinder는 Cylinder 클래스의 인스턴스이다.
console.log(cylinder instanceof Cylinder); // true
// cylinder는 Circle 클래스의 인스턴스이다.
console.log(cylinder instanceof Circle);   // true



}

```