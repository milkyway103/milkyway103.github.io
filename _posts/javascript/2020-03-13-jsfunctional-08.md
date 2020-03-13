---
layout: post
title: 함수형 프로그래밍과 JavaScript ES6+ 7. 지연성 1 (2)
categories : javascript
tags : javascript functional-programming ES6
comments : true
---

> inflearn의 [함수형 프로그래밍과 JavaScript ES6+](https://www.inflearn.com/course/functional-es6)를 보고 공부한 것을 정리합니다.

## range, map, filter, take, reduce 중첩 사용

하나의 문제를 지연성을 가진 함수와, 그렇지 않은 함수들을 통해 해결해보며 둘 간의 차이를 명확히 하고 평가 시점에 대하여 알아볼 것이다.

```javascript
go(range(10),
    map( n => n + 10),
    filter(n => n % 2),
    take(2),
    log);
```

<center>
   <figure>
   <img src="/assets/post-img/javascript/jsfunctional8-image1.png" alt="views">
   <figcaption></figcaption>
   </figure>
</center>

브레이크 포인트를 찍어서 확인한다.

<center>
   <figure>
   <img src="/assets/post-img/javascript/jsfunctional8-image2.png" alt="views">
   <figcaption></figcaption>
   </figure>
</center>

인자로 받은 10이 지역변수로 표시된다.

<center>
   <figure>
   <img src="/assets/post-img/javascript/jsfunctional8-image3.png" alt="views">
   <figcaption></figcaption>
   </figure>
</center>

while문 안으로 들어와, 현재 `i`값은 0이고, 그 0이 push된다.

<center>
   <figure>
   <img src="/assets/post-img/javascript/jsfunctional8-image4.png" alt="views">
   <figcaption></figcaption>
   </figure>
</center>

반복함에 따라 `i`가 바뀌고 `res`에도 값이 담긴다.
`i++`한 값이 `l`보다 작은 때까지만 동작하기 때문에 해당하는 값까지만 담고 종료한다.

map, filter, reduce의 `for of` 코드는 숨겨져 있는 내용이 많다.
그 안에서 정확히 어떤 일이 일어나는지에 대해 확인하기 위해, 세세하게 명령형으로 작성하여 대체한 뒤 브레이크포인트를 찍을 것이다.

```javascript
    const range = l => {
        let i = -1;
        let res = [];
        while(++i < l) {
            res.push(i);
        }
        return res;
    };

    const map = curry((f, iter) => {
        let res = [];
        iter = iter[Symbol.iterator]();
        let cur;
        while (!(cur = iter.next()).done) {
            const a = cur.value;
            res.push(f(a));
        }
        return res;
    });

    const filter = curry((f, iter) => {
        let res = [];
        iter = iter[Symbol.iterator]();
        let cur;
        while (!(cur = iter.next()).done) {
            const a = cur.value;
            if (f(a)) res.push(a);
        }
        return res;
    });

    const reduce = curry((f, acc, iter) => {
        if (!iter) {
            iter = acc[Symbol.iterator]();
            acc = iter.next().value;
        } else {
            iter = iter[Symbol.iterator]();
        }
        let cur;
        while (!(cur = iter.next()).done) {
            const a = cur.value;
            acc = f(acc, a);
        }
        return acc;
    });

    const take = curry((l, iter) => { // limit, iterable
        let res = [];
        iter = iter[Symbol.iterator]();
        let cur;
        while (!(cur = iter.next()).done) {
            const a = cur.value;
            res.push(a);
            if(res.length == l) return res;
        }
        return res;
    });

    const L = {};
    L.range = function *(l) {
        let i = -1;
        while(++i < l) {
            yield i;
        }
    };

    L.map = function *(f, iter) {
        iter = iter[Symbol.iterator]();
        let cur;
        while (!(cur = iter.next()).done) {
            const a = cur.value;
            yield f(a);
        }
    };

    L.filter = function *(f, iter) {
        iter = iter[Symbol.iterator]();
        let cur;
        while (!(cur = iter.next()).done) {
            const a = cur.value;
            if (f(a)) {
                yield a;
            }
        }
    };
```

<center>
   <figure>
   <img src="/assets/post-img/javascript/jsfunctional8-image5.png" alt="views">
   <figcaption></figcaption>
   </figure>
</center>

`range` 함수에서 return되면 `range(10)` 자리에 리턴된 값으로 평가된다.
평가되면 그 코드는 `go`를 통해서 `map` 함수로 들어간다.

<center>
   <figure>
   <img src="/assets/post-img/javascript/jsfunctional8-image6.png" alt="views">
   <figcaption></figcaption>
   </figure>
</center>

현재 `iter`는 배열이고, 이터러블한 값이다.

<center>
   <figure>
   <img src="/assets/post-img/javascript/jsfunctional8-image7.png" alt="views">
   <figcaption></figcaption>
   </figure>
</center>

반복문 안으로 들어가게 되면 위의 `iter`는 `Array Iterator`라는 값으로 바뀐다.

<center>
   <figure>
   <img src="/assets/post-img/javascript/jsfunctional8-image8.png" alt="views">
   <figcaption></figcaption>
   </figure>
</center>

`next`를 통해 꺼낸 값을 `cur`에 `value`로 참조하여 `a`에 담는다.
여기세서는 앞에서 range를 통해 만들었던 배열의 첫 번째 값인 `0`이 들어왔다.

<center>
   <figure>
   <img src="/assets/post-img/javascript/jsfunctional8-image9.png" alt="views">
   <figcaption></figcaption>
   </figure>
</center>

반복되면서 계속해서 `res`에 `push`한다.
배열의 크기였던 10개만큼 반복한다.

<center>
   <figure>
   <img src="/assets/post-img/javascript/jsfunctional8-image10.png" alt="views">
   <figcaption></figcaption>
   </figure>
</center>

반복이 끝나면 해당하는 결과를 `go`에서 받아 다음 함수에 넘겨준다.

<center>
   <figure>
   <img src="/assets/post-img/javascript/jsfunctional8-image11.png" alt="views">
   <figcaption></figcaption>
   </figure>
</center>

`filter`에는 `map`을 통해 모든 원소에 10이 더해진 값의 배열인 `iter`가 들어온다.

<center>
   <figure>
   <img src="/assets/post-img/javascript/jsfunctional8-image12.png" alt="views">
   <figcaption></figcaption>
   </figure>
</center>

역시 이터레이터로 변환하고 그를 통해 반복한다.

<center>
   <figure>
   <img src="/assets/post-img/javascript/jsfunctional8-image13.png" alt="views">
   <figcaption></figcaption>
   </figure>
</center>

`filter`에서는 `f`를 실행하여 확인해보고 조건이 맞으면 `res`에 `push`한다.

<center>
   <figure>
   <img src="/assets/post-img/javascript/jsfunctional8-image14.png" alt="views">
   <figcaption></figcaption>
   </figure>
</center>

array를 순회하면서 원하는 조건의 값만 `res`에 담는다.

<center>
   <figure>
   <img src="/assets/post-img/javascript/jsfunctional8-image15.png" alt="views">
   <figcaption></figcaption>
   </figure>
</center>

역시 `go`를 통해 `take` 함수에 전달된다.
`take` 함수에서는 다섯 개의 원소로 이루어진 배열을 받는다.
이터레이터를 만들어 반복한다.

<center>
   <figure>
   <img src="/assets/post-img/javascript/jsfunctional8-image16.png" alt="views">
   <figcaption></figcaption>
   </figure>
</center>

`l` 값으로 `2`를 받았으므로, `res`의 `length`가 같아질 때까지 담는다.
`a` 값을 `push`하고 확인하는 과정을 반복한다.
12까지 `res`에 들어가면 `l`과 `length`가 같아지기 때문에 `if`문이 `True`로 평가된다. -> return!

<center>
   <figure>
   <img src="/assets/post-img/javascript/jsfunctional8-image17.png" alt="views">
   <figcaption></figcaption>
   </figure>
</center>

결과가 떨어지면 `log`가 찍힌다.

## L.range, L.map, L.filter, take의 평가 순서

```javascript
    go(L.range(10),
        L.map(n => n + 10),
        L.filter(n => n % 2),
        take(2),
        log);
```

이 경우에는 다른 순서로 코드가 평가된다.
실행되었을 때, 어떤 함수에 가장 먼저 들어갈까? (마찬가지로 브레이크포인트로 확인)
나는 `L.map`일 거라고 예상했는데, `take`였다.

<center>
   <figure>
   <img src="/assets/post-img/javascript/jsfunctional8-image18.png" alt="views">
   <figcaption>take가 가장 먼저 실행됨</figcaption>
   </figure>
</center>

코드에서는 `L.range`를 가장 먼저 실행했지만, take에 가장 먼저 들어갔다.
다시 말하면, `L.range`, `L.map`, `L.filter` 함수 안쪽의 어떤 연산도 하지 않고 바로 `take` 함수로 들어간 것이다.

<center>
   <figure>
   <img src="/assets/post-img/javascript/jsfunctional8-image19.png" alt="views">
   <figcaption></figcaption>
   </figure>
</center>

`take` 함수에 들어온 `iter`를 확인해 보면 `generator`라고 되어 있는, 이전과는 다른 값이다.

<center>
   <figure>
   <img src="/assets/post-img/javascript/jsfunctional8-image20.png" alt="views">
   <figcaption></figcaption>
   </figure>
</center>

well-formed iterator는 본인이 이터레이터이면서, `symbol.iterator` 함수를 가지고 있고, 이것을 실행했을 때 이터레이터인 자기 자신을 다시 리턴한다.
그렇기 때문에 `symbol.iterator` line을 지나도 여전히 이터레이터이다.
그래서 `take` 함수는 제너레이터가 만든 이터레이터도 여전히 잘 이터레이터로 만들 수 있게 된다.
자바스크립트는 이런 방식으로 다형성을 잘 유지할 수 있도록 만들어져 있다.

<center>
   <figure>
   <img src="/assets/post-img/javascript/jsfunctional8-image21.png" alt="views">
   <figcaption></figcaption>
   </figure>
</center>

while문 안쪽으로 들어가려고 하면, `L.filter` 함수 안으로 들어오게 된다.
즉, `iter.next()`를 호출했떠니 `L.filter`에 들어온 것이다.
이유는 `L.range`를 한 결과, 즉 안쪽의 코드들이 평가되기를 미뤄둔 제너레이터가 바로 `L.map`으로 들어가게 되고, `L.map` 역시도 바로 평가되기를 미뤄둔 이터레이터를 리턴하기 때문에 `L.filter` 역시 이터레이터를 리턴한다.
그래서 여기에서 `take`를 실행했을 때에는 `L.filter`가 리턴한 이터레이터를 `take`가 받고 있고, `take`가 받아둔, `L.filter`가 리턴한 이터레이터에 처음 `next()`를 실행했을 때, `L.filter`함수 안쪽에서 평가가 시작된 것이다.

<center>
   <figure>
   <img src="/assets/post-img/javascript/jsfunctional8-image22.png" alt="views">
   <figcaption></figcaption>
   </figure>
</center>

`L.filter` 함수 역시 제너레이터로 만들어진 이터레이터를 받고 있다.

<center>
   <figure>
   <img src="/assets/post-img/javascript/jsfunctional8-image23.png" alt="views">
   <figcaption></figcaption>
   </figure>
</center>

여기서 while 문 안쪽으로 들어가려고 하면 또 `iter.next()`를 실행하게 된다.
그럼 다시 `L.map`으로 넘어간다. (`L.map` 함수가 만들어준 이터레이터이기 때문에!)

<center>
   <figure>
   <img src="/assets/post-img/javascript/jsfunctional8-image24.png" alt="views">
   <figcaption></figcaption>
   </figure>
</center>

`L.map`에서 `iter`로 받은 것 역시 `L.range`가 만든 이터레이터이다.

<center>
   <figure>
   <img src="/assets/post-img/javascript/jsfunctional8-image25.png" alt="views">
   <figcaption></figcaption>
   </figure>
</center>

다시 while문으로 들어가려고 하면 역시 `L.range`가 만든 이터레이터에 `next()`를 실행하기 때문에 `L.range`로 가게 된다.
즉, 위에서부터 평가되는 것이 아니라 `go`로 들어가서 `take`함수가 마치 먼저 실행되는 것처럼 보이고, 순회를 하려고 하자 역으로 `L.filter`, `L.map`, `L.range`의 순으로 올라간다.

<center>
   <figure>
   <img src="/assets/post-img/javascript/jsfunctional8-image26.png" alt="views">
   <figcaption></figcaption>
   </figure>
</center>

`L.range` 함수에서 계속 진행하게 되면 **드디어** while문 안쪽으로 들어가서, `0`을 `yield`한다.

<center>
   <figure>
   <img src="/assets/post-img/javascript/jsfunctional8-image27.png" alt="views">
   <figcaption></figcaption>
   </figure>
</center>

이 값은 `L.map`에서 `next`를 통해 얻고자 했던 값이기 때문에 `map`의 while문으로 넘어가게 된다. `L.range`로 만들어진 이터레이터의 `next`를 통해 해당하는 0이라는 값을 받아서 `map`의 `n => n + 10` 함수를 적용하고 또 `yield`하게 되면

<center>
   <figure>
   <img src="/assets/post-img/javascript/jsfunctional8-image28.png" alt="views">
   <figcaption></figcaption>
   </figure>
</center>

마찬가지로 `L.filter`에서 `L.map`을 통해 만들어진 이터레이터에 `next()`를 요청했기 때문에 `L.filter`로 넘어가게 된다. 이제 `L.filter`에서 `a`가 홀수인지 확인해 보고, 아니기 때문에 다시 `L.range`로 돌아간다.

<center>
   <figure>
   <img src="/assets/post-img/javascript/jsfunctional8-image29.png" alt="views">
   <figcaption></figcaption>
   </figure>
</center>

즉시 평가되는 `map`, `filter`, `take`의 경우에는 `range`에서 10개짜리 배열을 먼저 다 만든다. 그 후에 `map`을 하면서 10개짜리 배열에 모두 10을 더하고, 모두 확인하면서 `filter`한 값을 만들어 그 값이 `take`에 들어간다.

그러나 이 경우에는 똑같은 `take` 함수임에도 불구하고, `제너레이터 이터레이터 방식`으로 만들어진 함수들을 통해 `take` 함수의 `next`를 했을 때 순서가 반대로 된다. `take`를 하나 하고자 했떠니 반대로 위로 올라가면서 `filter`할 것, `map`할 것을 달라고 `range`에게 요청하면 0을 `map`이 받아서 10을 더한 것을 적용하고 `filter`가 받아서 조건을 확인한다.
즉, 가로로 진행되는 것이 아니라 세로로 진행되는 것이라고 할 수 있다.

```javascript
0       1     ...
10      11
false   true
```

올라갔다가 내려오고 하는 식으로 동작한다!
아까의 `iter`에 `next()`를 했을 때 0이었던 값은 `take`로 내려오지 않았고, 1이 들어왔을 때 `L.filter`가 처음으로 `yield`를 했기 때문에 그 전 값은 `take`가 아예 확인조차 하지 않았다. `L.filter`에서 `True`로 내려온 값만 확인한다. 그리고 `l`과 비교해본 후 작기 때문에 while문을 돌면서 다시 `iter.next()`를 진행한다. 그럼 다시 `L.range`로부터 값이 내려오게 된다.

<center>
   <figure>
   <img src="/assets/post-img/javascript/jsfunctional8-image30.png" alt="views">
   <figcaption></figcaption>
   </figure>
</center>
