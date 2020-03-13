---
layout: post
title: 함수형 프로그래밍과 JavaScript ES6+ 7. 지연성 1 (3)
categories : javascript
tags : javascript functional-programming ES6
comments : true
---

> inflearn의 [함수형 프로그래밍과 JavaScript ES6+](https://www.inflearn.com/course/functional-es6)를 보고 공부한 것을 정리합니다.

## 엄격한 계산과 느긋한 계산의 효율성 비교

즉시 실행되는 코드와 지연적으로 동작하는 코드의 가장 큰 차이는 다음과 같다.

즉시 실행하는 코드는 `map`함수가 배열의 모든 크기만큼 옆으로 다 돈 다음, `filter`에서 역시 결과를 옆으로 다 가면서 만들고 해당하는 값이 `take`에 들어와 두 개의 원소를 담고 끝나게 된다.

지연적으로 동작하는 코드의 경우에는 `take`로 가장 먼저 들어와, 첫 번째 `next()`를 해 봤을 때 `L.map`으로 가서 하나의 값(0)만을 받아서 바로 `yield`를 하면, `L.filter`로 가고, `L.filter`의 조건에 맞지 않기 때문에 `yield`를 하지 않는다. (여기서는 `take`를 실행하지 않는다.) 그 다음에도 역시 올라갔다가 내려왔다가를 반복하면서 필요한 값까지만을 돌고 끝나게 된다.

즉시 평가하는 경우 만약 `range(10000)` 등의 큰 값을 받게 되면 실행시간이 길어진다. 하지만 지연적 동작의 경우에는 아무리 숫자가 커도 원하는 값까지만 받고 동작을 종료하기 때문에 성능상에 아무 문제가 없다.

```javascript
console.time('');
go(range(10000),
    map( n => n + 10),
    filter(n => n % 2),
    take(2),
    log);
console.timeEnd('');

console.time('L');
go(L.range(10000),
    L.map(n => n + 10),
    L.filter(n => n % 2),
    take(2),
    log);
console.timeEnd('L');
```

<center>
   <figure>
   <img src="/assets/post-img/javascript/jsfunctional9-image1.png" alt="views">
   <figcaption></figcaption>
   </figure>
</center>

지연적으로 동작하는 코드의 경우에는 아무리 숫자가 커져도 `take`하는 만큼의 시간이 걸린다. 무한수열 (`range(Infinity)`) 을 넣더라도 정상동작한다.

## map, filter 계열 함수들이 가지는 결합 법칙

map, filter 계열 함수에는 특정한 방식으로 다르게 평가 순서를 바꾸어도 똑같은 결과를 만든다는 `결합 법칙`이 있다.

- 사용하는 데이터가 무엇이든지
- 사용하는 보조가 순수 함수라면 무엇이든지
  - 데이터 추출, for문 순회
- 아래와 같이 결합된다면 둘 다 결과가 같다.

```
[[mapping, mapping], [filtering, filtering], [mapping, mapping]]
=
[[mapping, filtering, mapping], [mapping, filtering, mapping]]
```

## ES6의 기본 규약을 통해 구현하는 지연 평가의 장점

이러한 지연 평가 방식이 이전의 자바스크립트에서는 굉장히 복잡하거나 지저분한 방식으로 구현할 수밖에 없었다. 왜냐하면 공식적인 자바스크립트에 있는 값이 아니라 전혀 다른 형태의 규약들을 만들어, 해당하는 라이브러리 안에서만 동작할 수 있는 방식으로 구현해야 했기 때문이다.

ES6 이후에는 자바스크립트의 공식적인 값을 통해서, 함수와 리턴되는 실제 자바스크립트의 값을 통해서 지연 여부를 확인하고 원하는 시점에 평가하겠다는 등, 자바스크립트와 개발자가 약속된 규약을 가지고 만들어갈 수 있도록 개선되었다.

ES6 이후에는 지연성을 다루는 데에 있어서 자바스크립트의 고유한, 약속된 값을 통해 구현, 합성, 동작이 가능해졌다.

이러한 방식으로 구현된 지연성은 서로 다른 라이브러리 또는 서로 다른 사람들이 만든 함수 등 어디에서든지 자바스크립트의 기본 값과 기본 객체를 통해 소통하기 때문에 `조합성`이 높다.
