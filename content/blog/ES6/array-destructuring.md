---
title: Array destructuring
date: 2019-10-07 23:10:72
category: ES6
---

## 배열 디스트럭처링 (Array destructuring)

```js

const arr = [1, 2, 3];

const [one, two, three] = arr;
//디스트럭처링을 사용할 때는 반드시 initializer(초기화)를 할당해야 함
// const [one, two, three]; // SyntaxError: Missing initializer in destructuring declaration


console.log(one, two, three);  //1 2 3

```

## 객체 디스트럭처링 (Object destructuring)

ES6의 갹체 디스트럭처링은 객체의 각 프로퍼티를 객체로부터 추출하여 변수 리스트에 할당한다. 이때 할당 기준은 프로퍼티 이름(키)이다.

```js

const obj = { firstName: 'Duvis' , lastName: 'Lee' };

const {firstName, lastName} = obj;

console.log(firstName, lastName);

```

```js

const todos = [
    { id: 1, content: 'HTML', complated: true },
    { id: 2, content: 'CSS', complated: false },
    { id: 3, content: 'JS', complated: false }
];

const complatedTodos = todos.filter(({complated}) => complated);
console.log(completedTodos); // [ { id: 1, content: 'HTML', completed: true } ]
```

```js

const person = {
    name: 'Lee',
    address: {
        zipCode: '60603',
        city: 'Seoul'
    }
};


//console.log(person.address.city);
const { address: {city} } = person;
console.log(city);


```
