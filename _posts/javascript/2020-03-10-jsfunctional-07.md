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

```javascript
function test(name, time, f) {
    console.time(name);
    while (time--) f();
    console.timeEnd(name);
}

test('range', 10, () => reduce(add, range(1000000)));
test('L.range', 10, () => reduce(add, L.range(1000000)));
```

그렇게까지 드라마틱한 차이는 나지 않지만 `L.range`가 `range`에 비해 좀더 효율적이라는 점을 알 수 있다.

<center>
   <figure>
   <img src="/assets/post-img/javascript/jsfunctional7-image6.png" alt="views">
   <figcaption>test 결과</figcaption>
   </figure>
</center>

## take

이터레이터를 순회하는 또다른 함수 take

```javascript
const take = curry((l, iter) => { // limit, iterable
    let res = [];
    for (const a of iter) {
        res.push(a);
        if(res.length == l) return res;
    }
    return res;
});
```
이터러블 프로토콜을 따른다.
이터러블 안의 값을 `next`를 통해 순회하고 꺼내서 push만 하는 단순한 로직을 가지고 있다.

```javascript
log(take(5, range(100))); // [0, 1, 2, 3, 4]
log(take(5, L.range(100))); // [0, 1, 2, 3, 4]
```

L.range같이 `지연성`을 가지는 값을 이터레이터로 만들게 되면 전혀 다른 함수가 이터레이터 프로토콜만 따를 때, 얼마든지 조합이 가능하다. 자바스크립트 고유의 프로토콜을 통해서 가능해지는 것이기 때문에 조합성이 높고, 잘 구현할 수 있다. 효율성적인 측면에서도 마찬가지로 훨씬 낫다.

```javascript
console.time('');
log(take(5, range(100000)));
console.timeEnd('');

console.time('');
log(take(5, L.range(100000)));
console.timeEnd('');
```
<center>
   <figure>
   <img src="/assets/post-img/javascript/jsfunctional7-image7.png" alt="views">
   <figcaption>효율성의 차이</figcaption>
   </figure>
</center>

이러한 효율성의 차이가 발생하는 이유는, `range` 같은 경우에는 100000 크기의 array를 만들고 5개를 뽑지만, `L.range`는 5개만 만들고 뽑기 때문이다.
이를 활용하면 다음과 같은 것도 가능하다.

```javascript
log(take(5, L.range(Infinity)));
```

<center>
   <figure>
   <img src="/assets/post-img/javascript/jsfunctional7-image8.png" alt="views">
   <figcaption>beyond the infinity</figcaption>
   </figure>
</center>

무한수열로 표현하게 되더라도 어차피 5개의 값만 만들기 때문에 똑같은 시간이 소요된다.
하지만 `range` 안에 무한수열을 넣게 되면 브라우저가 뻗는다.

다음과 같은 작업에서도 `L.range`가 더 효율적이다.

```javascript
console.time('');
go(
    range(10000),
    take(5),
    reduce(add),
    log
);
console.timeEnd('');

console.time('');
go(
    L.range(10000),
    take(5),
    reduce(add),
    log
);
console.timeEnd('');
```

<center>
   <figure>
   <img src="/assets/post-img/javascript/jsfunctional7-image9.png" alt="views">
   <figcaption></figcaption>
   </figure>
</center>

`L.range`의 지연성은 `take`나 `reduce`와 같은 함수를 만날 때 연산이 시작되게 된다. 제너레이터로 이터레이터를 리턴하는 함수를 실행했을 때에는 해당하는 연산이 안에서 이루어지지 않는다. `reduce`와 같이 배열 안의 첫번째 값과 두번째 값을 꺼내서 연산을 필요로 하는 함수나 몇 개의 길이를 가지는지 모르는 어레이에서 두 개의 결과만 뽑는 것과 같은 함수에서, `최대한 연산을 미루다가 그 때 연산을 처음 하는 기법`인 것이다.

## 이터러블 중심 프로그래밍에서의 지연 평가 (Lazy Evaluation)
= 제때 계산법, 느긋한 계산법
제너레이터/이터레이터 프로토콜을 기반으로 구현된다.

지연 평가는 게으른 평가라고도 이야기하지만 영리하다는 뜻이 함께 들어 있다.
즉, 그냥 게으르기만 한 것이 아니라 최대한 게으르면서도 가장 영리하게 평가한다는 뜻이다.

제때 계산법이라고도 부르는 이유는 이 지연 평가가 가장 필요할 때까지 평가를 미루다가 가장 필요할 때 해당하는 코드들을 평가하면서 값들을 만들어나가는 기법이기 때문이다. `L.range` 함수에서 봤던 것처럼 큰 크기의 배열을 미리 만들어 놓는 것이 아니라 그 이후에 필요한 연산이었던 reduce를 하면서 add를 한다고 할 때, 즉 필요한 값을 뽑을 때만 그 값을 만들어내면서 값을 만드는 것을 최소화하고 연산을 좀더 효율적으로 줄이는 아이디어이다.

그러나 이전의 자바스크립트에서는 이러한 구현이 어려웠다. 엄밀히 말하면, 구현 자체는 가능했지만 공식적인 구현이라고 보기에는 어려웠다. 언어 자체에서 해당하는 로직을 구현할 수 있도록 기반이 되는 프로토콜이 있지 않았기 때문이다.

그러므로 지연 평가를 위한 별개의 연산이 많이 추가되어야 했다. 이것이 큰 연산은 아니지만, 공식적인 자바스크립트의 값이나 프로토콜이 아니었기 때문에 서로 다른 라이브러리나 함수에서 함께 사용되기는 어려웠다.

그러나 ES6에서는 `이터러블/이터레이터 프로토콜`로 인해 지연성으로 코드의 평가를 미루고 코드를 값으로 다루는 프로그래밍을 할 때 보다 공식적인 자바스크립트의 일반 값으로써 구현할 수 있게 되었다. 나아가 서로 다른 라이브러리나 함수들이 안전한 조합성, 합성성 등에서 향상되었다.

그래서 이 강의에서는 제너레이터 기반으로 이터러블 중심 프로그래밍에서의 지연 평가를 구현할 것이다. (= 리스트 중심 프로그래밍, 컬렉션 중심 프로그래밍) 이터러블 중심 프로그래밍이란 map, filter, reduce, take 같은 함수들을 기반으로 프로그래밍하는 것을 말한다.

정리하자면, 이터터블 중심 프로그래밍을 할 때 어떻게 `지연성`을 구현할 수 있고 어떻게 지연성에 대해 보다 공식적인 값으로서 `조합성`을 만들어갈 수 있는지에 대해 알아볼 것이다.

## L.map

앞서 만들었던 map을 지연성을 가진 `L.map`으로 만들되 제너레이터/이터레이터 프로토콜을 통해 구현할 것이다.

좀더 구체적으로 표현하면 `L.map`은
1. 제너레이터 함수
2. 이터레이터를 반환
3. 이 반환되는 이터레이터는 평가를 미루는 성질을 가지고 평가 순서를 달리 조작할 수 있는 준비가 되어 있음 (지연성)

```javascript
L.map = function *(f, iter) {
    for (const a of iter) yield f(a);
};

var it = L.map(a => a + 10 , [1, 2, 3]);
log(it.next());
log(it.next());
log(it.next());
```

<center>
   <figure>
   <img src="/assets/post-img/javascript/jsfunctional7-image10.png" alt="views">
   <figcaption>L.map</figcaption>
   </figure>
</center>

next()를 통해 평가하는 만큼의 값만 얻어올 수 있다.

해당하는 이터레이터를 전개 연산자를 사용하여 평가할 수도 있다.

<center>
   <figure>
   <img src="/assets/post-img/javascript/jsfunctional7-image11.png" alt="views">
   <figcaption>[...it]</figcaption>
   </figure>
</center>

`L.map` 자체에서는 새로운 array를 만들지도 않고 값 하나하나마다 순회하면서 yield를 통해 함수가 적용된 값을 next를 통해 하나씩 전달하게 된다. 지연성을 가지는 이터레이터 객체를 내가 원하는 방법으로 평가할 수 있게 되는 것이다.
`it.next().value`와 같이 평가할 수도 있다.

<center>
   <figure>
   <img src="/assets/post-img/javascript/jsfunctional7-image12.png" alt="views">
   <figcaption>it.next().value</figcaption>
   </figure>
</center>

## L.filter

```javascript
L.filter = function *(f, iter) {
    for (const a of iter) if (f(a)) yield a;
};

var it = L.filter(a => a % 2, [1, 2, 3, 4]);
log(it.next());
log(it.next());
log(it.next());
```

`yield`가 총 네 번 되는 것이 아니라 원하는 상황에서만 yield된다.
next를 여러 번 했을 때 `done: true`가 되기 전까지, 해당하는 값을 필터링하여 next했을 때 value가 꺼내지도록 구성하였다.

<center>
   <figure>
   <img src="/assets/post-img/javascript/jsfunctional7-image13.png" alt="views">
   <figcaption>L.filter</figcaption>
   </figure>
</center>
