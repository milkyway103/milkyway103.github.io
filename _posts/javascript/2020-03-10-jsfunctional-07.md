---
layout: post
title: 함수형 프로그래밍과 JavaScript ES6+ 7. 지연성 1 (1)
categories : javascript
tags : javascript functional-programming ES6
comments : true
---

> inflearn의 [함수형 프로그래밍과 JavaScript ES6+](https://www.inflearn.com/course/functional-es6)를 보고 공부한 것을 정리합니다.

## range와 느긋한 L.range

### range
숫자 하나를 받고, 그 숫자의 크기만큼 배열을 리턴하는 함수

```javascript
const range = l => {
    let i = -1;
    let res = [];
    while(++i < l) {
        res.push(i);
    }
    return res;
};

log(range(5)); // [0, 1, 2, 3, 4]
log(range(2)); // [0, 1]
```

이 안에 있는 모든 값들을 더하는 코드를 구현

```javascript
const add = (a, b) => a + b;

var list = range(4);
log(reduce(add, list)); // 6
```

### L.range

똑같은 일을 다르게 구현해서 해본다.
`느긋한` range를 만든다.

```javascript
const L = {};
L.range = function *(l) {
    let i = -1;
    while(++i < l) {
        yield i;
    }
};

var list = L.range(4);
log(reduce(add, list));
```

제너레이터를 통해 만든 range

중간에 log(list)를 통해 출력해보면 다음과 같다.

<center>
   <figure>
   <img src="/assets/post-img/javascript/jsfunctional7-image1.png" alt="views">
   <figcaption>log(list)</figcaption>
   </figure>
</center>

`range`는 배열이, `L.range`는 다른 것이 출력된다.
이를 좀 더 자세히 보게 되면 이터레이터라는 것을 알게 된다.

<center>
   <figure>
   <img src="/assets/post-img/javascript/jsfunctional7-image2.png" alt="views">
   <figcaption>이터레이터</figcaption>
   </figure>
</center>

그런데 둘 다 같은 결과를 만든 이유는 무엇일까?
`reduce`라는 함수가 이터러블을 받기 때문이다.
배열과 이터레이터 모두 이터러블이기 때문에,
안에서 이터러블을 이터레이터로 만든 후 안에 있는 값을 하나씩 조회하면서 결과를 만들게 된다.

### 느긋한 L.range와 그냥 range의 차이

코드를 다음과 같이 약간 바꿔서 내부 과정을 확인해보자.

```javascript
const range = l => {
    let i = -1;
    let res = [];
    while(++i < l) {
        log(i, 'range');
        res.push(i);
    }
    return res;
};

const L = {};
L.range = function *(l) {
    let i = -1;
    while(++i < l) {
        log(i, 'L.range');
        yield i;
    }
};
```
`range`의 경우에는 reduce에 전달하기 전에, 그러니까 이미 range(4)가 실행된 시점에 즉시 이 부분의 코드가 배열로 완전히 평가된다.

<center>
   <figure>
   <img src="/assets/post-img/javascript/jsfunctional7-image3.png" alt="views">
   <figcaption></figcaption>
   </figure>
</center>

그러나 `L.range`는 이 함수의 어떤 부분도 실행되지 않았다는 것을 확인할 수 있다.
그렇다면 언제 처음 이 코드가 평가되는 것일까?
-> 이 이터레이터의 내부를 `순회`할 때마다 하나씩 값이 평가된다.
다음과 같이 `next()`를 해 보면 알 수 있다.

```javascript
var list = L.range(4);
log(list);
log(list.next());
```
<center>
   <figure>
   <img src="/assets/post-img/javascript/jsfunctional7-image4.png" alt="views">
   <figcaption>next()의 결과</figcaption>
   </figure>
</center>

<center>
   <figure>
   <img src="/assets/post-img/javascript/jsfunctional7-image5.png" alt="views">
   <figcaption>next()를 더 많이 해보자</figcaption>
   </figure>
</center>

그냥 `range`는 `range()`를 실행했을 때 이미 모든 부분이 평가되면서 값이 만들어진다. 하지만 `L.range`는 `L.range()`를 실행한 시점에서는 아무 값도 평가되지 않는다.

```javascript
var a = [0, 1, 2]
```
라고 선언이 되어 있다면 이 a는 아직 필요한 값이라고 볼 수 없다. (엄밀히 말하자면!) a의 값을 순회하여 사용자에게 필요한 어떤 값을 만들어낼 때까지는 그저 메모리만 차지하고 있게 되는 것이다.
이러한 이유로 `L.range`는 배열 형태가 되지 않은 채로, 다시 말하면 실제로 평가가 완벽하게 되지 않은 상태로 있다가 reduce 안에서 값이 필요할 때까지 기다렸다가 평가가 이루어지면서 값을 꺼내도록 한 것이다.

정리하면 다음과 같다.
`range`는 array를 만들어 놓은 뒤, 배열로 전달되어 동작한다.
`L.range`는 array를 만들지 않고 하나씩 값을 꺼내기만 한다.

한편 이 `range`로부터 생성된 array가 reduce 안에서 처리될 때, 생략된 과정이 있다. reduce 안에서 이 array를 이터레이터로 만들고 `next`하면서 순회하게 되는 과정이 그것이다. 하지만 `L.range`는 실행됐을 때 이미 이터레이터를 만든다. 이 이터레이터는 자기 자신을 그대로 리턴하는 이터러블이다. 그리하여 해당하는 함수를 실행하면 이미 만들어져 있는 이터레이터를 그냥 리턴하고 순회하기 때문에 조금 더 효율적이라고 말할 수 있다.


## range와 느긋한 L.range 테스트

## take

## 제너레이터/이터레이터 프로토콜로 구현하는 지연 평가

## L.map

## L.filter

## range, map, filter, take, reduce 중첩 사용

## L.range, L.map, L.filter, take의 평가 순서

## 엄격한 계산과 느긋한 계산의 효율성 비교

## map, filter 계열 함수들이 가지는 결합 법칙

## ES6의 기본 규약을 통해 구현하는 지연 평가의 장점
