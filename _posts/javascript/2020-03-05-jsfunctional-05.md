---
layout: post
title: 함수형 프로그래밍과 JavaScript ES6+ 5. 코드를 값으로 다루어 표현력 높이기
categories : javascript
tags : javascript functional-programming ES6
comments : true
---

> inflearn의 [함수형 프로그래밍과 JavaScript ES6+](https://www.inflearn.com/course/functional-es6)를 보고 공부한 것을 정리합니다.

## go
- 함수형 프로그래밍에서는 코드를 값으로 다루는 아이디어를 많이 사용한다.
- 이렇게 하면 어떤 함수가 코드인 함수를 받아서 평가하는 시점을 원하는 대로 지정할 수 있기 때문에, 코드의 표현력을 높이는 등의 다양하고 좋은 아이디어를 많이 가지고 있다.

```JavaScript
const add = (a, b) => a+b;

log(
    reduce(
        add,
        map(p => p.price,
        filter(p => p.price < 20000, products))
    )
);
```

- 위의 코드는 함수가 중첩되어 있다 보니 읽기에 불편하다.

```JavaScript
const go = () => { };

go(
    0,
    a => a + 1,
    a => a + 10,
    a => a + 100,
    log);
```

- `go`라는 함수를 생성한다.
- 우리가 이 함수에 대하여 기대하는 것은 초기값 0을 시작으로 해서 연속적으로 실행되며, 원하는 값이 마지막으로 111로 찍히는 것과 같은 동작이다.
- 즉, 인자들을 받아서 하나의 값으로 `축약`해나가는 것이다.
- 인자들이 들어왔다고 했을 때 첫 번째 인자를 다음 함수에, 다음 함수의 결과를 다시 그 다음 함수에게 전달하는 식으로 계속해서 연속적으로 하나의 일을 수행해야 한다.
- 축약하는 로직은 `reduce`!
- 그러므로 `go` 함수를 다음과 같이 작성할 수 있다.

```JavaScript
const go = (...args) => reduce((a, f) => f(a), args);
```

- args를 특정 함수로 축약해서 하나의 값으로 만들어가는 것이다.
- `reduce`를 사용하면 특정 리스트를 축약해나가는 코드를 작성할 때 쉽고 재밌게 만들 수 있다.

## pipe
- go 함수와 다르게 함수를 리턴하는 함수
- go 함수는 즉시 함수들과 인자를 전달하여 어떤 값을 평가하는 데에 사용된다면
- pipe 함수는 함수들이 나열되어 있는 합성된 함수를 만드는 함수이다.

```JavaScript
const f = pipe(
  a => a + 1,
  a => a + 10,
  a => a + 100
);
```
- 이런 식으로 새로운 함수를 리턴할 것을 예상하면서 `pipe` 함수를 작성해 보자.

```JavaScript
const pipe = (...fs) => (a) => go(a, ...fs);
```
- 함수를 리턴해주고
- 함수들인 `fs`를 인자로 받아준다.
- 인자는 나중에 받을 것이므로 리턴하는 함수의 인자 `a`로 넣고,
- `go(a, ...fs)`를 실행하는 함수를 리턴해주면 된다!
- 하지만 이렇게 pipe 함수를 만들게 되면 인자가 하나인 경우밖에 처리할 수 없다.
- 예를 들어,

```javascript
const f = pipe(
    (a, b) => a + b,
    a => a + 1,
    a => a + 10,
    a => a + 100
);

log(f(0, 1));
```

- 이런 식으로 사용할 수 없다.
- 이렇게 동작하도록 하고 싶다면

```JavaScript
const f = pipe(
    a => a + 1,
    a => a + 10,
    a => a + 100
);

log(f(add(0, 1)));
```

- 함수를 호출할 때 `add()` 함수를 사용해야 한다.
- 첫 번째 함수에 한하여 여러 개의 인자도 받도록 `pipe` 함수를 변경하면 다음과 같다.

```JavaScript
const pipe = (f, ...fs) => (...as) => go(f(...as), ...fs);
```
- `f` : 첫 번째 함수
- `...as` : 여러 개의 인자를 받아서
- `f(...as)` : `go` 함수의 첫 번째 인자에 하나의 값으로 평가되도록 하는 함수를 넣어 준다.

## go를 사용하여 읽기 좋은 코드로 만들기

```JavaScript
go(
    products, // products 로 시작해
    products => filter(p => p.price < 20000, products), // 1. filter
    products => map(p => p.price, products), // 2. map
    prices => reduce(add, prices), // 3. reduce
    log // 4. log
);
```
- 코드는 좀 더 복잡해졌지만
- 좀 더 읽기에 편해짐!

## go+curry를 사용하여 더 읽기 좋은 코드로 만들기

- `curry`  : 역시 코드를 값으로 다루면서, 받아둔 함수를 내가 원하는 시점에 평가시키는 함수
1. 함수를 받아서 함수를 리턴
2. 인자를 받아서 원하는 개수만큼의 인자가 들어왔을 때 받아둔 함수를 평가시킨다.
- 인자가 두 개 이상이라면 (length 프로퍼티가 있다면) 받아둔 함수를 실행하고 아니라면 함수를 다시 리턴한 후, 그 이후에 받은 인자들을 합쳐서 실행하는 함수

```JavaScript
const curry = f => // 함수를 받아서
    (a, ..._) => _.length ? f(a, ..._) : (..._) => f(a, ..._); // a : 첫번째 인자, _ : 나머지 인자
```

```JavaScript
const mult = (a, b) => a * b;
```
이 함수를 만들 때 `curry`로 한 번 감싸면

```JavaScript
const mult = curry((a, b) => a * b);
```

위와 같이 된다.
이를 순차적으로 출력해보자.

```JavaScript
log(mult); // (a, ..._) => _.length ? f(a, ..._) : (..._) => f(a, ..._) 함수가 리턴됨
log(mult(1)); // (..._) => f(a, ..._)
log(mult(1)(2)); // 2
log(mult(1, 2)); // 2
```

아래와 같은 응용도 가능하다.

```JavaScript
const mult3 = mult(3);
log(mult3(10));
log(mult3(5));
log(mult3(1));
```

이를 활용해 코드를 좀 더! 간결하게 만들어본다.
우선, map, filter, reduce를 전부 curry로 감싼다.

```JavaScript
const curry = f =>
    (a, ..._) => _.length ? f(a, ..._) : (..._) => f(a, ..._);

const map = curry((f, iter) => {
    let res = [];
    for (const a of iter) {
        res.push(f(a));
    }
    return res;
});

const filter = curry((f, iter) => {
    let res = [];
    for (const a of iter) {
        if (f(a)) res.push(a);
    }
    return res;
});

const reduce = curry((f, acc, iter) => {
    if (!iter) {
        iter = acc[Symbol.iterator]();
        acc = iter.next().value;
    }
    for (const a of iter) {
        acc = f(acc, a);
    }
    return acc;
});
```

이제 이 모든 함수들이 인자를 하나만 받으면 이후의 인자를 받도록 기다리는 함수가 된다!

```JavaScript
go(
    products, // products 로 시작해
    products => filter(p => p.price < 20000)(products), // 1. filter
    products => map(p => p.price)(products), // 2. map
    prices => reduce(add)(prices), // 3. reduce
    log // 4. log
);
```

이렇게 쓸 수 바꿀 수 있다.
여기서 다음과 같이 생각해볼 수 있다.

**`a => f(a)` 는 `f`와 같다.**
즉, 인자 하나를 받아 거기에 1을 더하는 함수 `add1`이 있다고 하자.
이것을 `a => add1(a)` 라고 하는 것과,
`add1`이라고 하는 것의 본질적 의미는 같다!

그렇다면 위의 코드를 다음과 같이 간결하게 만들 수 있다.

```JavaScript
go(
    products, // products 로 시작해
    filter(p => p.price < 20000), // 1. filter
    map(p => p.price), // 2. map
    reduce(add), // 3. reduce
    log // 4. log
);
```

함수를 부분적으로 실행하는 curry 함수를 통해 보다 간결한 표현이 가능해진다.

## 함수 조합으로 함수 만들기

다음의 두 코드에는 중복이 존재한다. (map, reduce)

```JavaScript
go(
    products,
    filter(p => p.price < 20000),
    map(p => p.price),
    reduce(add),
    log
);

go(
    products,
    filter(p => p.price >= 20000),
    map(p => p.price),
    reduce(add),
    log
);
```

이를 새 함수를 만듦으로써 쉽게 제거할 수 있다.

```JavaScript
const total_price = pipe(
    map(p => p.price),
    reduce(add));

go(
products,
filter(p => p.price < 20000),
total_price,
log
);

go(
products,
filter(p => p.price >= 20000),
total_price,
log
);
```

다음과 같은 방식으로도 가능하다.

```JavaScript
const base_total_price = predi => pipe(
    filter(predi),
    total_price
);

go(
    products,
    base_total_price(p => p.price < 20000),
    log
);

go(
    products,
    base_total_price(p => p.price >= 20000),
    log
);
```

함수형 프로그래밍에서는 고차함수들을 함수의 조합으로 계속해서 잘게 나누어가면서 중복을 제거하는 식으로, 또 재사용성을 높이는 방식으로 사용할 수 있다.
