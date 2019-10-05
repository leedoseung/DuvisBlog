---
title: Promise
date: 2019-10-05 23:10:03
category: es6
---

## Promise?

자바스크립트는 비동기 처리를 위한 하나의 패턴으로 콜백 함수를 사용한다. 하지만 전통적인 콜백 패턴은 가독성이 나쁘고 비동기 처리 중 발생한 에러의 예외 처리가 곤란하여 여러 개의 비동기 처리 로직을 한꺼번에 처리하는 것도 한계가 있다.

## Callback Hell

```html

<!DOCTYPE html>
<html>
<body>
  <script>
    // 비동기 함수
    function get(url) {
      // XMLHttpRequest 객체 생성
      const xhr = new XMLHttpRequest();

      // 서버 응답 시 호출될 이벤트 핸들러
      xhr.onreadystatechange = function () {
        // 서버 응답 완료가 아니면 무시
        if (xhr.readyState !== XMLHttpRequest.DONE) return;

        if (xhr.status === 200) { // 정상 응답
          console.log(xhr.response);
          // 비동기 함수의 결과에 대한 처리는 반환할 수 없다.
          return xhr.response; // ①
        } else { // 비정상 응답
          console.log('Error: ' + xhr.status);
        }
      };

      // 비동기 방식으로 Request 오픈
      xhr.open('GET', url);
      // Request 전송
      xhr.send();
    }

    // 비동기 함수 내의 readystatechange 이벤트 핸들러에서 처리 결과를 반환(①)하면 순서가 보장되지 않는다.
    const res = get('http://jsonplaceholder.typicode.com/posts/1');
    console.log(res); // ② undefined
  </script>
</body>
</html>


```

만일 비동기 함수의 처리 결과를 가지고 다른 비동기 함수를 호출해야 하는 경우, 함수의 호출이 중첩(nesting)이 되어 복잡도가 높아지는 현상이 발생하는데 이를 Callback Hell이라 한다.


```js

try {
  setTimeout(() => { throw new Error('Error!'); }, 1000);
} catch (e) {
  console.log('에러를 캐치하지 못한다..');
  console.log(e);
}

```

## 프로미스의 생성

```js

const promise = new Promise((resolve, reject) => {

    if(/* 비동기 작업 수행 성공 */){
        resolve('result');
    }else(/* 비동기 작업 수행 실패 */){
        reject('fail');
    }
});

|  상태 |  의미 |  구현 |
|:--------|:--------|:--------:|
|pending | 비동기 처리가 아직 수행되지 않은 상태 | resolve 또는 reject가 아직 호출되지 않은 상태 |
|**fulfilled**    | 비동기 처리가 수행된 상태(성공) | resolve 함수가 호출된 상태 |
|**rejected**  | 비동기 처리가 수행된 상태(실패) | reject 함수가 호출된 상태 |
|settle | 비동기 처리가 수행된 상태(성공 또는 실패) | resolve 또는 reject 함수가 호출된 상태 |


```

Promise를 사용하여 비동기 함수 정의

```js

const promiseAjax = (method, url, payload) => {
    return new Promise((resolve, reject) => {
        const xhr = new XMLHttpRequest();
        xhr.open(method, url);
        xhr.setRequsetHeader('Content-type', 'application/json');

        xhr.onreadystatechange = function () {
            //서버 응답 완료가 아니면 무시
            if(xhr.readyState !=== XMLHttpRequest.DONE) return;

            if(xhr.state >= 200 && xhr.status < 400) {
                // resolve 메소드를 호출하면서 처리 결과 전달
                resolve(xhr.response);
            }else{
                // reject 메소드를 호출하면서 에러 메지를 전달
                reject(new Error(xhr.status));
            }
        }
    })
}

```