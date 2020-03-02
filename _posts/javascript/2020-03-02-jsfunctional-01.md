---
layout: post
title: 함수형 프로그래밍과 JavaScript ES6+ 1. 함수형 자바스크립트 기본기
categories : javascript
tags : javascript functional-programming ES6
comments : true
---

> inflearn의 [함수형 프로그래밍과 JavaScript ES6+](https://www.inflearn.com/course/functional-es6)를 보고 공부한 것을 정리합니다.

# 평가
- 코드가 계산(Evaluation) 되어 값을 만드는 것

# 일급
- 값으로 다룰 수 있다.
- 변수에 담을 수 있다.
- 함수의 인자로 사용될 수 있다.
- 함수의 결과로 사용될 수 있다.

```javascript
const a = 10; // 함수를 값을 담음
const add10 = a => a + 10; // 변수에 함수를 담음
const r = add10(a); // 함수의 결과로 사용됨
log(r); // 함수의 인자로 사용됨
```

# 일급 함수
- 자바스크립트에서 함수는 일급
- 함수를 값으로 다룰 수 있다는 뜻

```javascript
 const add5 = a => a + 5; // 함수를 값으로 다루어 담음
 log(add5); // 함수에게 전달하여 출력 가능 : a => a + 5 가 출력됨
 log(add5(5)); // 함수의 결과로 만들어 출력될 수 있음 : 10이 출력됨

 const f1 = () => () => 1; // 함수의 결과값으로 함수가 사용될 수 있음
 // f1이라는 함수는 실행되었을 때 다시 함수를 리턴할 수 있음
 log(f1()); // () => 1 이 출력됨. 즉 실행 결과가 함수인 것.

 const f2 = f1(); // 다시 실행 결과를 f2라는 변수에 담음
 log(f2); // () => 1 이 출력됨.
 log(f2()); // 담겨진 함수를 내가 원하는 시점에 평가하여 결과를 만들 수 있다.
```

자바스크립트에서 함수가 1급이다.
-> ***조합성과 추상화의 도구로 함수를 사용할 수 있음!***

# 고차 함수
- 함수를 값으로 다루는 함수
- 크게 두 가지
  1. 함수를 인자로 받아서 실행하는 함수
  2. 함수를 만들어 리턴하는 함수 (클로저를 만들어 리턴하는 함수)

## 함수를 인자로 받아서 실행하는 함수

```javascript
 const apply1 = f => f(1); // 함수를 받아서 함수에 1을 적용하는 함수
 const add2 = a => a+2;
 log(apply1(add2)); // 3으로 평가됨
 log(apply1(a => a - 1)); // 0으로 평가됨
```
함수를 인자값으로 다루고 있기 때문에 apply1은 고차 함수

```javascript
 const times = (f, n) => { // 함수 f와 숫자 n을 받아서 n만큼 함수 f를 실행하는 함수
   // 실행하면서 몇 번째 실행되는 중인지도 전달하도록 만들어봄
   let i = -1;
   while (++i < n) f(i);
 }

 times(log, 3); // 3번 실행됨 : 0, 1, 2
 times(a => log(a + 10), 3); // 10, 11, 12
```
함수를 값으로 받아서 또 다른 값을 받아 안에서 실행하면서 자신이 원하는 인자를 적용하는 함수
-> applicative programming

## 함수를 만들어 리턴하는 함수 (클로저를 만들어 리턴하는 함수)

```javascript
  const addMaker = a => b => a + b;
  const add10 = addMaker(10); // b =? b + 10이 저장되어 있음
  log(add10(5)); // 15
  log(add10(10)); // 20
```
- 클로저 : 이 함수가 a를 계속해서 기억하고 있다는 뜻
- 이 함수는 함수이자 a를 기억하는 클로저
- 클로저 : add10이라는 함수가 만들어질 때의 환경인 a(10)와 b => a + b를 통칭해서 말하는 용어
- addMaker는 클로저를 리턴하는 함수이고,
- 함수가 함수를 만들어서 리턴할 때는 결국은 클로저를 만들어서 리턴하기 위해 사용한다.
