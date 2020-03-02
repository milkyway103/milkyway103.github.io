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

## 사용자 정의 이터러블, 이터러블/이터레이터 프로토콜 정의

```JavaScript
  const iterable = { // for문으로 호출했을 때 value로 3, 2, 1을 리턴해주고 끝나는 사용자 정의 이터러블 구현
    [Symbol.iterator]() { // Symbol.iterator 메서드를 구현하고 있어야
      let i = 3;
      return { // 이터레이터를 반환
        next() { // 이터레이터는 next를 메서드로 가지고 있음
          return i == 0 ? { done: true} { value: i--, done: false } // next는 value와 done을 가지고 있는 객체를 리턴
        }
      }
    }
  };
  let iterator = iterable[Symbor.iterator]();
  // log(iterator.next()); // next를 통해 조회 가능
  // log(iterator.next());
  // log(iterator.next());
  // log(iterator.next());
  for (const a of iterable) log(a); // for..of를 통해 순회 가능
```
<center>
 <figure>
 <img src="/assets/post-img/javascript/jsfunctional2-image4.png" alt="views">
 <figcaption>사용자 정의 이터러블에서 next()의 출력</figcaption>
 </figure>
 </center>
 <center>
  <figure>
  <img src="/assets/post-img/javascript/jsfunctional2-image5.png" alt="views">
  <figcaption>사용자 정의 이터러블에서 for...of의 출력</figcaption>
  </figure>
  </center>
- iterable에 Symbor.iterator가 구현되어 있기 때문에 for..of 문에 들어갈 수 있음
- 이터레이터 객체를 반환
- 내부적으로 next()를 실행하면서 value를 담게 됨
- 아직은 자바스크립트 이터레이터 이터러블의 모든 속성을 구현하지는 못했음.

```JavaScript
  const arr2 = [1, 2, 3];
  let arr2[Symbol.iterator]();
  iter2.next(); // 일부 진행했을 때 진행한 이후의 값으로만 순회가 가능
  for (const a of arr2) log(a);

```
- 잘 구현된 이터러블은 이터레이터를 만들었을 때 진행 도중 순회를 할 수도 있고, 이 이터레이터를 그대로 for .. of문에 넣었을 때 그대로 모든 값을 순회할 수 있도록 되어 있음.
- 위의 iter2 역시 Symbol.iterator를 가지고 있음
- 그리고 그를 실행한 결과는 자기 자신
```javascript
log(iter2[Symbol.iterator]() == iter2); // true
```
- 이렇게 이터레이터가 자기 자신을 반환하는 Symborl.iterator를 가지고 있을 때 well-formed iterable, well-formed iterator라고 할 수 있다.

```JavaScript
  const iterable = { // for문으로 호출했을 때 value로 3, 2, 1을 리턴해주고 끝나는 사용자 정의 이터러블 구현
    [Symbol.iterator]() { // Symbol.iterator 메서드를 구현하고 있어야
      let i = 3;
      return { // 이터레이터를 반환
        next() { // 이터레이터는 next를 메서드로 가지고 있음
          return i == 0 ? { done: true} { value: i--, done: false }; // next는 value와 done을 가지고 있는 객체를 리턴
        },
        [Symbol.iterator]() { return this; }
      }
    }
  };
```
- 이 사용자 정의 이터러블이 well-formed iterator를 반환할 수 있도록 하기 위하여, 자기 자신이 이터레이터이면서 Symbor.iterator()를 리턴했을 때 자기 자신을 반환하도록 하여 다시 한 번 for문에 들어간다거나 할 때 이전까지 진행되었던 자기 자신을 기억하여 그곳으로부터 다시 for문이 진행될 수 있도록 한다.
- 이미 많은 오픈소스 라이브러리들이나 자바스크립트에서 순회 가능한 형태의 값을 가진 객체들은 대부분 이 iterable/iterator 프로토콜을 따르기 시작하였음.
- 페이스북에서 만든 immutable js 역시 이 프로토콜을 따른다.
- 자바스크립트가 사용되고 있는 환경인 브라우저들의 Web API... 돔과 같은 곳에서도 이 프로토콜을 따르고 있다.

```html
<script>
  log(document.querySelectorAll('*')); // 엘리먼트들을 조회한 상태에서
  for (const a of document.querySelectorAll('*')) log(a); // 와 같은 식으로 순회가 가능
  log(all); // 이 값이 배열이어가 아니라,
  log(all[Symbol.iterator]()) // 가 구현되어 있으며 이터레이터를 리턴하기 때문
</script>
```

## 전개 연산자
- 마찬가지로 이터레이터/이터러블 프로토콜을 따른다.
```JavaScript
const a = [1, 2];
log([...a, ...[3, 4]]); // [1, 2, 3, 4]로 하나의 array가 됨
```
- 전개연산자 역시 이터러블 프로토콜을 따르고 있는 값들을 펼칠 수 있음
```javascript
log([...a, ...arr, ...set, ...map.keys()]);
```
- 여러 가지를 섞을 수도 있음.
- 이터러블을 정확히 익히고, 이터러블에서 사용된 추상을 정확히 아는 것이 중요.

> [강의참고자료](https://github.com/indongyoo/functional-javascript-01)
