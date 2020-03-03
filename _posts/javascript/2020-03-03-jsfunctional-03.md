---
layout: post
title: 함수형 프로그래밍과 JavaScript ES6+ 3. 제너레이터와 이터레이터
categories : javascript
tags : javascript functional-programming ES6
comments : true
---

> inflearn의 [함수형 프로그래밍과 JavaScript ES6+](https://www.inflearn.com/course/functional-es6)를 보고 공부한 것을 정리합니다.

## 제너레이터/이터레이터
- 제너레이터 : 이터레이터이자 이터러블을 생성하는 함수
-> 즉, 이터레이터를 리턴하는 함수

```JavaScript
    const log = console.log;
    function *gen() { // 일반함수에 별을 붙여서
        yield 1;
        if (false) yield 2; // 제너레이터는 순회할 값을 문장으로 표현하는 것이라고도 말할 수 있다
        yield 3;
        return 100;
    }
    let iter = gen(); // 제너레이터를 실행한 결과는 이터레이터이자 이터러블
    log(iter[Symbol.iterator]() == iter); // true -> 제너레이터는 well-formed iterator를 리턴
    log(iter.next()); // Object -> {value: 1, done: false}
    log(iter.next());
    log(iter.next());
    log(iter.next()); // 리턴값은 done일 때에 출력됨
    // 제너레이터를 통해 쉽게 이터레이터를 생성 가능

    for (const a of gen()) log(a); // 순회할 때는 리턴값 없이 순회가 이루어짐
```

- 일반함수에 `*`를 붙여서 제너레이터를 생성
- 제너레이터를 실행한 결과는 이터레이터이자 이터러블 (well-formed)
-> 제너레이터를 통해 쉽게 이터레이터를 생성 가능
- 리턴값은 done일 때에 출력됨
- 다만 순회할 때에는 리턴 값 없이 순회가 이루어진다.
- 제너레이터는 순회할 값을 문장으로 표현하는 것이라고 말할 수 있다.
- 이 제너레이터를 통해 어떠한 값도 순회할 수 있는 형태로 조작할 수 있다.
-> **자바스크립트의 높은 다형성**

## odds

제너레이터를 활용하여 홀수만 계속해서 발생시키는 이터레이터를 만들어 순회하는 연습

1. 가장 간단한 홀수 발생 제너레이터
```JavaScript
    function *odds(limit) {
        for (let i = 0; i< limit; i++) {
            if (i % 2) yield i;
        }
    }
    let iter2 = odds(10);
    log(iter2.next());
    log(iter2.next());
    log(iter2.next());
    log(iter2.next());
    log(iter2.next());
```

2. 무한 발생 제너레이터
```javascript
    function *infinity(i = 0) {
        while (true) yield i++;
    }
    let iter3 = infinity();
    log(iter3.next());
    log(iter3.next());
    log(iter3.next());
    log(iter3.next());
    log(iter3.next());
```
- 무한하긴 하지만 next할 때만 동작하기 때문에 메모리를 많이 사용하지 않는다.

3. 중첩된 제너레이터
```JavaScript
    function *odds(limit) {
        for (const a of infinity(1)) {
            if (a % 2) yield a;
            if (a == limit) return;
        }
    }
    // 중첩도 가능
    let iter4 = odds(10);
    log(iter4.next());
    log(iter4.next());
    log(iter4.next());
    log(iter4.next());
    log(iter4.next());
```

4. limit과 또다른 이터러블을 인자로 받는 제너레이터
```JavaScript
    function *limit(limit, iter) { // iterable을 받아서 계속해서
        for (const a of iter) { // iter안의 값을 yield하다가
            yield a;
            if (a == limit) return; // limit과 같은 값을 만나면 더이상 돌지 않도록
        }
    }
    let iter5 = limit(4, [1, 2, 3, 4, 5, 6]);
    log(iter5.next());
    log(iter5.next());
    log(iter5.next());
    log(iter5.next());
    log(iter5.next());
```

5. `*limit`을 활용한 홀수 생성 제너레이터
```javascript
    function *odds(l) {
        for (const a of limit(l, infinity(1))) {
            if (a % 2) yield a;
        }
    }

    for (const a of odds(40)) log(a);
```

제너레이터를 활용하여 문장으로 만들어진 것들을 순회한다거나 할 수 있도록 값으로 문장을 다룰 수 있다.

## for of, 전개 연산자, 구조 분해, 나머지 연산자
제너레이터도 이터레이터/이터러블 프로토콜을 따르고 있기 때문에 그 프로토콜을 따르는 문법들, 라이브러리, 함수들과 함께 잘 사용될 수 있다.

### 전개 연산자
```JavaScript
log(...odds(10));
log([...odds(10), ...odds(20)]);
```

### 구조 분해
```JavaScript
const [head, ...tail] = odds(5);
log(head);
log(tail);
```

### 나머지 연산자
```JavaScript
    const [a, b, ...rest] = odds(10);
    log(a);
    log(b);
    log(rest);
```

이처럼 제너레이터, 이터레이터를 활용하여 좀더 조합성이 높은 프로그래밍을 할 수 있음
