---
layout: post
title: 함수형 프로그래밍과 JavaScript ES6+ 4. map, filter, reduce
categories : javascript
tags : javascript functional-programming ES6
comments : true
---

> inflearn의 [함수형 프로그래밍과 JavaScript ES6+](https://www.inflearn.com/course/functional-es6)를 보고 공부한 것을 정리합니다.

## map

```JavaScript
const products = [
    { name: '반팔티', price: 15000 }, // 각각 상품 하나
    { name: '긴팔티', price: 20000 },
    { name: '핸드폰케이스', price: 15000 },
    { name: '후드티', price: 30000 },
    { name: '바지', price: 25000 },
];
```
- 보통 이러한 데이터를 다룰 때 products에 있는 상품의 이름들을 따로 모아서 출력하거나, 가격들을 따로 배열에 담는 등의 로직을 종종 사용한다.

```javascript
let names = [];
for (const p of products) {
    names.push(p.name);
}
log(names);

let prices = [];
for (const p of products) {
    prices.push(p.price);
}
log(prices);
```

- 위의 `log(names)`와 같은 함수 직접적으로 어떠한 명령을 일으켜서 함수 외부의 영역에 변화를 일으킨다.
- 하지만 함수형 프로그래밍에서는 함수가 `인자`와 `리턴값`으로 소통하는 것을 권장한다.
-> 이런 코드를 작성할 때 사용하는 함수가 `map`함수이다.

```javascript
const map = (f, iter) => {
    let res = [];
    for (const a of iter) {
        res.push(f(a));
    }
    return res;
};

log(map(p => p.name, products));
    log(map(p => p.price, products));
```

- `iter` : 상품뿐 아니라 어떤 데이터도 받을 수 있도록, iter이란 map함수가 받는 값이 이터러블 프로토콜을 따른다는 것을 의미한다.
- `f` : 함수를 인자로 받아 어떤 값을 수정할 것인지 그 함수에 위임한다.
- `return`

<center>
 <figure>
 <img src="/assets/post-img/javascript/jsfunctional4-image1.png" alt="views">
 <figcaption>map 함수의 사용</figcaption>
 </figure>
 </center>

- 순서대로 map함수를 사용하지 않은 결과값, map함수를 사용한 결과값이다.
- map이라는 함수의 보조함수를 통해 이 이터러블 안에 있는 값 중 어떠한 것을 수집하겠다고 전달하는 식으로 사용한다.
- map 함수는 고차함수 : 함수를 값으로 다루면서 내가 원하는 시점에 인자를 전달

### 이터러블 프로토콜을 따른 map의 다형성
- map 함수는 이터러블 프로토콜을 따르고 있기 때문에 다형성이 높다.
```javascript
log(document.querySelectorAll('*');
```
<center>
 <figure>
 <img src="/assets/post-img/javascript/jsfunctional4-image2.png" alt="views">
 <figcaption></figcaption>
 </figure>
 </center>

- 이 결과는 array처럼 생겼다.
- 하지만 다음의 코드는 에러가 발생한다.
```JavaScript
log(document.querySelectorAll('*').map(el => el.nodeName));
```
- document.querySelectorAll 은 array를 상속받은 함수가 아니기 때문이다.
- 그래서 프로토타입에 map 함수가 구현이 되어 있지 않다.
- 하지만 앞에서 만들었던 map함수를 사용한 코드는 동작한다!

```javascript
log(map(el => el.nodeName, document.querySelectorAll('*')));
```

```JavaScript
const it = document.querySelectorAll('*')[Symbol.iterator]();
    log(it.next());
    log(it.next());
    log(it.next());
    log(it.next());
    log(it.next());
```

- document.querySelectorAll 이 `이터러블 프로토콜`을 따르고 있기 때문이다.
- map 함수는 이터러블 프로토콜을 따르는 `for of` 문을 사용했기 때문에 동작한다.

- 이터러블 프로토콜을 따르는, 이미 만들어져 있는 모든 값에도 map을 사용할 수 있지만, 제너레이터 함수의 결과물에 대해서도 map을 할 수 있다.

```JavaScript
function *gen() {
        yield 2;
        yield 3;
        yield 4;
    }
    log(map(a => a * a, gen())); // 이것도 동작! output : [4, 9, 16]
```

<center>
 <figure>
 <img src="/assets/post-img/javascript/jsfunctional4-image3.png" alt="views">
 <figcaption></figcaption>
 </figure>
</center>

- 사실상 모든 것들에 대해 `map` 을 할 수 있다고 볼 수 있다!
- 앞으로 만들어지는 Web API (자바스크립트에 있는 것이 아니라 브라우저의 값, 헬퍼 함수들) 들이 이터러블 프로토콜을 따르고 있고, 계속해서 그렇게 만들어질 것이기 때문에 이터러블 프로토콜을 따르는 함수들을 사용한다는 것은 앞으로 많은 헬퍼 함수들과의 조합성이 좋아진다는 이야기이다.
- 프로토타입 기반, 혹은 클래스 기반으로 어떤 뿌리를 가진 카테고리 안에 있는 값만을 사용할 수 있는 기법보다 훨씬 다양성이 높고 유연하다.


####Map
- 이터러블 가운데 `Map` 존재
```javascript
    let m = new Map();
    m.set('a', 10);
    m.set('b', 20);
    const it2 = m[Symbol.iterator]();
    log(it2.next());
    log(it2.next());
    log(it2.next());
```

- map 함수의 이터러블 값으로도 사용할 수 있다.
```JavaScript
log(new Map(map(([k, a]) => [k, a * 2], m)));
```
<center>
 <figure>
 <img src="/assets/post-img/javascript/jsfunctional4-image4.png" alt="views">
 <figcaption>map 함수를 통해 new Map 생성</figcaption>
 </figure>
</center>

- key와 value를 나눠서 받을 수 있다.
- 다시 Map 객체를 만들 수도 있다.
- 안쪽에 있는 값이 바뀐 Map 객체를 만드는 데에 이런 방식으로 조합할 수도 있다.

## filter
- 특정 조건을 만족하는 데이터들을 걸러내는 코드
```JavaScript
    let under20000 = [];
    for (const p of products) {
        if (p.price < 20000) under20000.push(p);
    }
    log(...under20000);
```
- 이런 식의 작업을 수행한다.
- 그러나, 위처럼 코딩하게 되면 다른 조건의 작업을 수행한다고 했을 때 코드의 변경과 중복이 발생한다.

```JavaScript
let over20000 = [];
for (const p of products) {
    if (p.price >= 20000) over20000.push(p);
}
log(...over20000);
```

```javascript
const filter = (f, iter) => {
    let res = [];
    for (const a of iter) {
        if (f(a)) res.push(a);
    }
    return res;
};
log(...filter(p => p.price < 20000, products));
log(...filter(p => p.price >= 20000, products));
```
- 어떤 조건으로 필터링할 것인지는 보조함수에 위임한다.

<center>
 <figure>
 <img src="/assets/post-img/javascript/jsfunctional4-image5.png" alt="views">
 <figcaption>filter</figcaption>
 </figure>
</center>

- array에 대해서도 가능하다.
```JavaScript
log(filter(n => n % 2, [1, 2, 3, 4]));
```
<center>
 <figure>
 <img src="/assets/post-img/javascript/jsfunctional4-image6.png" alt="views">
 <figcaption>array에 filter 적용</figcaption>
 </figure>
</center>

- 즉시실행함수와의 결합도 가능하다.
```JavaScript
log(filter(n => n % 2, function *() {
    yield 1;
    yield 2;
    yield 3;
    yield 4;
    yield 5;
} ()));
```
<center>
 <figure>
 <img src="/assets/post-img/javascript/jsfunctional4-image7.png" alt="views">
 <figcaption>즉시실행함수에 filter 적용</figcaption>
 </figure>
</center>
- 내부에 있는 값의 다형성은 보조함수를 통해 지원하고
- 외부적 다형서은 이터러블 프로토콜을 따르는 것으로 지원한다.
- 그럼으로ㅓ써 filter 역시 다양한 것들을 걸러낼 수 있게 된다.

## reduce
- reduce는 이터러블 값을 하나의 다른 값으로 축약하는 함수
```JavaScript
const nums = [1, 2, 3, 4, 5];

let total = 0;
for (const n of nums) {
    total = total + n;
}
log(total);
```
- 특정한 값을 순회해나가면서 하나의 값으로 누적해나갈 때, 이런 코드와 이런 패턴을 사용한다.

```JavaScript
const add = (a, b) => a + b;
log(reduce(add, 0, [1, 2, 3, 4, 5]));
```
- 위와 같이 reduce 함수를 사용한다고 했을 때, 내부적으로는 다음과 같이 동작한다.
```javascript
log(add(add(add(add(add(0, 1), 2), 3), 4), 5));
```
- 즉, 내부적으로는 초기값에서부터 시작하여 재귀적으로 보조함수를 실행하면서 하나의 값으로 누적해나간다.
```javascript
const reduce = (f, acc, iter) => {
    for (const a of iter) {
        acc = f(acc, a);
    }
    return acc;
};
```
- `acc` : 누적값
- 'return' : 외부세상을 직접 변경하는 것이 아니라 리턴
- 자바스크립트에서의 reduce는 `acc`를 optional하게 사용할 수 있도록 구현되어 있다.
```JavaScript
log(reduce(add, [1, 2, 3, 4, 5]));
```
- 위와 같이 시작하는 값을 생략했을 경우,
```javascript
log(reduce(add, 1, [2, 3, 4, 5]));
```
- 이렇게 받은 것처럼 동작한다.
- 즉, 이터러블의 첫번째 값 시작값으로 하여 동작한다.
- 이를 반영한 코드는 다음과 같다.
```JavaScript
const reduce = (f, acc, iter) => {
    if (!iter) { // iter 값이 없다면 acc이 생략된 것
        iter = acc[Symbol.iterator](); // 이터러블 값으로 변경
        acc = iter.next().value; // 이터러블의 첫 번째 값으로 초기화
    }
    for (const a of iter) {
        acc = f(acc, a);
    }
    return acc;
};
```

- 숫자만 들어 있는 데이터 외에, 복잡한 데이터 역시 축약할 수 있다.
- products에 있는 모든 데이터를 `reduce`를 이용해 더하는 코드
```javascript
log(
    reduce(
        (total_price, product) => total_price + product.price,
    0,
    products));
```
- reduce 함수의 경우에도 역시 보조함수를 통해 내부적 다형성을, 이터러블을 통하여 외부적 다형성을 지원한다.

## map+filter+reduce 중첩 사용과 함수형 사고
- 20000원 미만의 모든 상품들의 가격을 합친 값을 도출하는 코드 작성
```javascript
    const add = (a, b) => a + b;

    log(
        reduce(
            add,
            map(p => p.price,
                filter(p => p.price < 20000, products))));
```
- 이 코드를 오른쪽에서 왼쪽으로 읽어나가면 쉽다!
- filter -> map -> add로 reduce하여 출력(log)

```javascript
log(
    reduce(
        add,
        filter(n => n < 20000,
            map(p => p.price, products))));
```
- 순서를 바꾸어 작성할 수도 있다.
- 함수들을 중첩하고 함수를 연속적으로 실행하면서 어떤 값으로부터 출발하여 필터링, 그 다음 사용하기 좋은 값으로 추출하고 reduce를 통해 축약해나가면서 원하는 일을 하는 식으로 작성한다.
- 이렇게 작성할 때 좀더 함수적으로 이 코드를 읽거나 사고하려면?
  1) reduce를 하고 싶은데 add를 하는 reduce를 하고 싶다
  ```JavaScript
  log(
    reduce(
      add,
      [10, 20, 30, 40]
    )
  );
  ```
  2) 숫자가 들어 있는 배열이 평가될 것으로 생각하면서 코드를 작성한 상태에서 이 자리에 숫자가 들어 있는 배열로 평가되도록 코드를 작성해 나간다.
  ```JavaScript
  log(
    reduce(
      add,
      map(p => p.price, products)
    )
  );
  ```
  3) 마찬가지로, `map(p => p.price, products)` 에서의 `products`가 특정 조건들만 남겨져 있을 것으로 평가되도록 작성하면 ok
  ```JavaScript
  log(
    reduce(
      add,
      map(
        p => p.price,
        filter(p => p.price < 20000, products)
      )
    )
  );
  ```
