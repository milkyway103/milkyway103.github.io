---
layout: post
title: 함수형 프로그래밍과 JavaScript ES6+ 2. ES6에서의 순회와 이터러블:이터레이터 프로토콜
categories : javascript
tags : javascript functional-programming ES6
comments : true
---

> inflearn의 [함수형 프로그래밍과 JavaScript ES6+](https://www.inflearn.com/course/functional-es6)를 보고 공부한 것을 정리합니다.

## 기존과 달라진 ES6에서의 리스트 순회
- 대부분의 프로그래밍에서 (실무에서도) 리스트 순회는 굉장히 중요
- 자바스크립트가 ES6이 되면서 리스트 순회에 많은 변경점 존재
- 깊이 들여다보면 언어적으로 큰 발전
- for i++
- for of

기존 ES5에서의 리스트 순회
```javascript
  const list = [1, 2, 3];
  for (var i = 0; i < list.length; i++) {
    log(list[i]);
  }
```
array의 length라는 프로퍼티에 의존하여 숫자라는 key로 순회하도록 i를 증가시켜주면서 숫자 key로 접근하여 array 안의 값을 순회

유사배열 역시 동일한 방식으로 순회
```javascript
  const str = 'abc';
  for (var i = 0; i < str.length; i++) {
    log(str[i]);
  }
```

ES6에서의 리스트 순회는 이렇게 바뀜
```javascript
  const list = [1, 2, 3];
  for (const a of list) {
    log(a);
  }
  const str = 'abc';
  for (const a of str) {
    log(a);
  }
```
- 간결해진 문법
- 어떻게 순회하는지를 명령적으로 기술하기보다 보다 선언적으로
- 복잡한 for문을 좀더 간결하게 만든 것 이상의 의미들이 있음
- ES6이라는 언어가 순회에 대해 추상화한 것

## Array, Set, Map을 통해 알아보는 이터러블/이터레이터 프로토콜
- array, set, map을 모두 for문으로 순회 가능
- 각 공통점, 차이점을 살펴보면서 ES6에서 for문이 어떻게 동작하고 어떻게 추상화되어 있는지 알아봄

### Array를 통해 알아보기
```JavaScript
 const arr = [1, 2, 3];
 for (const a of arr) log(a);
```

### Set을 통해 알아보기
```javascript
  const set = new Set([1, 2, 3]);
  for (const a of set) log(a);
```

### Map을 통해 알아보기
```javascript
  const map = new Map([['a', 1], ['b', 2], ['c', 3]]);
  for (const a of map log(a);
```

- array는 key로 안에 있는 값을 조회할 수 있다.
- 그러나 set과 map은 key로 조회가 불가능
- 즉 for of의 내부문이 `for (var i = 0; i < str.length; i++)` 이렇게 생기지 않았다는 뜻

## `Symbol.iterator`
- ES6에서 추가된, 말 그대로 Symbol
- 이 Symbol은 어떤 객체의 key로 사용될 수 있다.
- `log(arr[Symbol.iterator])`를 출력해 보면, 어떤 함수가 출력된다.
- `arr[Symbol.iterator] = null;` 함으로써 이 함수를 비워보고 다시 for of 순회를 하게 되면 `Uncaught TypeError: arr is not iterable at ...` 이라는 에러가 뜬다.
- 즉, arr가 iterable할 수 없다는 에러.
  -> for of문과 Symbol.iterator에 담겨져 있는 함수가 연관이 있다는 뜻
- Set이나 Map도 마찬가지!

## 이터러블/이터레이터 프로토콜
- Array, Set, Map은 자바스크립트 내장 객체로서 이 프로토콜을 따른다.
- 이터러블 : 이터레이터를 리턴하는 [Symbol.iterator]() 를 가진 값
  - 즉 `arr`는 이터러블이라고 할 수 있음
  - arr의 `Symbol.iterator`를 지워서 에러가 날 때 arr가 이터러블이 아니라는 에러가 출력되었었음.
  - 에러가 난 이유는 이터러블을 충족시키는 이터레이터 메서드를 지웠기 때문임!
  - 그리고 이 `arr[Symbol.iterator]()` 메서드를 실행했을 때는 Array 이터레이터를 리턴한다.
- 이터레이터 : { value, done } 객체를 리턴하는 next() 를 가진 값
  - 이터레이터는 next() 를 가졌음
  - next() 를 실행시켰을 때 value와 done을 가지는 객체를 리턴
  - 위의 Array iterator에 대해서 next()를 출력하면 다음과 같음
  <center>
   <figure>
   <img src="/assets/post-img/javascript/jsfunctional2-image1.png" alt="views">
   <figcaption>Array에서 iterator의 출력</figcaption>
   </figure>
   </center>
   - 계속해서 next()를 출력하게 되면 어느 시점부터는 done이 true이고 value가 undefined로 출력된다.
- 이터러블/이터레이터 프로토콜: 이터러블을 for...of, 전개 연산자 등과 함께 동작하도록 한 규약
  - for...of문에서는 value에 들어오는 값을 a에 담아서 계속 출력하다가, done이 true가 되면 이 for문에서 빠져나오도록 되어 있다.

```javascript
  const arr = [1, 2, 3];
  let iter1 = arr[Symbol.iterator]();
  iter1.next(); // 1
  iter1.next(); // 2
  iter1.next(); // 3
```

```javascript
  const arr = [1, 2, 3];
  let iter1 = arr[Symbol.iterator]();
  for (const a of iter1) log(a); // 1, 2, 3
```

Set이 key값이 없음에도 for of 문에서 동작하는 이유는 Set이 이터러블/이터레이터 프로토콜을 따르고 있기 때문.

<center>
 <figure>
 <img src="/assets/post-img/javascript/jsfunctional2-image2.png" alt="views">
 <figcaption>Set에서 iterator의 출력</figcaption>
 </figure>
 </center>

 <center>
  <figure>
  <img src="/assets/post-img/javascript/jsfunctional2-image3.png" alt="views">
  <figcaption>Map에서 iterator의 출력</figcaption>
  </figure>
  </center>
- Map의 iterator에는 value에 또다른 Array에 담겨 있음
```JavaScript
const map = new Map([['a', 1], ['b', 2], ['c', 3]]);
var iter2 = map[Symbro.iterator]();
for (const a of iter2) log(a);
```
-> for...of map 문과 동일한 출력

- 'map.keys()' -> MapIterator를 출력 -> map의 key만 출력 가능 ('a', 'b', 'c')
- 'map.values()' -> MapIterator를 출력 -> value만 출력
- 'map.entries()' -> key와 value를 함께 출력
- MapIterator 역시 Symbol.iterator를 가지고 있음.
- 즉 keys, values, entries 메서드는 이 Symbol.iterator가 또다시 반환하는 이터레이터를 가지고 순환한다.
