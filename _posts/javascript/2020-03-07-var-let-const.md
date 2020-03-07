---
layout: post
title: var, let, const
categories : javascript var let const ES6
comments : true
---

let과 const는 ES6 업데이트 이후 추가된 변수 선언 방식
# 1. var, let, const의 할당
## var

`var`을 이용하여 변수를 선언할 경우, 유연한 변수 선언이 가능하다.

```javascript
var name = "galaxy"
console.log(name); // galaxy

var name = "milkyway"
console.log(name); // milkyway
```

변수를 한 번 더 선언해도 에러가 나오지 않고 출력 시점마다 각기 다른 값이 출력된다.
이는 간단한 테스트에는 편리하지만, 코드량이 어마어마하게 많아질 경우, 변수가 어디에서 어떻게 사용되는지 정확하게 파악하기가 힘들고, 값이 바뀔 우려가 있다.

## let, const : 변수 재할당 불가

`let`과 `const`는 이러한 `var`의 단점을 보완하기 위해 만들어진 변수 선언 방식이다.
`let`을 사용해 위의 예제를 바꾸어 보면 에러가 발생한다.

```javascript
let name = "galaxy"
console.log(name); // galaxy

let name = "milkyway"
console.log(name);
// Uncaught SyntaxError: Identifier 'name' has already been declared
```
`name`이 이미 선언되었다는 에러 메시지가 발생한다.
`const`로 바꾸어 해 보아도 마찬가지의 에러 메시지가 발생한다.
즉, `let`과 `const`는 변수 재할당이 되지 않는다.

## const : immutable한 변수

`let`과 `const`의 차이점은 `immutable`한 변수가 선언되는가의 여부이다. 우선 다음과 같은 예제를 보면

```javascript
let name = "galaxy"
console.log(name); // galaxy

let name = "milkyway"
console.log(name);
// Uncaught SyntaxError: Identifier 'name' has already been declared

name = "surfin"
console.log(name); // surfin
```

`let`은 변수에 담긴 값을 변경할 수 있다. 즉, mutable한 변수를 선언한다.
그러나 `const`의 경우에는 변수 재선언, 변수 재할당 모두 불가능하다.

```javascript
const name = "galaxy"
console.log(name); // galaxy

const name = "milkyway"
console.log(name);
// Uncaught SyntaxError: Identifier 'name' has already been declared

name = "surfin"
console.log(name);
// Uncaught TypeError: Assignment to constant variable.
```

# 2. 호이스팅
호이스팅(Hoisting)이란, var 할당문이나 function 선언문 등을 해당 스코프의 선두로 옮긴 것처럼 동작하는 특성을 말한다. 즉, 다음과 같은 동작에서 에러가 발생하지 않는다.

```javascript
const hoisting = () => {
  console.log("First-Name:", name);
  var name = "Kim";
  console.log("Last-Name:", name);
}

hoisting();
// First Name : undefined
// Last Name : Kim
```

아직 선언되지 않은 변수인 name을 출력하는데, `undefined`가 출력될 뿐 에러는 발생하지 않는다. 즉, 지역변수 `name`의 선언이 함수의 최사단에서 선언된 것처럼 호이스트되었기 때문이다.

자바스크립트는 ES6에서 도입된 let, const를 포함하여 모든 선언(var, let, const, function, function*, class)을 호이스팅한다.

하지만 `var`로 선언된 변수와는 달리 `let`으로 선언된 변수를 선언문 이전에 참조하면 참조 에러 (ReferenceError) 가 발생한다.

```javascript
console.log(foo); undefined
var foo;

console.log(bar);
// Error: Uncaught ReferenceError: bar is not defined
let bar;
```

이는 `let`으로 선언된 변수는 스코프의 시작에서 변수의 선언까지 일시적 사각지대 (`Temporal Dead Zone; TDZ`)에 놓이기 때문이다.

## 변수의 생성 단계
자바스크립트에서 변수는
1. 선언 단계
2. 초기화 단계
3. 할당 단계
에 걸쳐 생성된다.

`var`으로 선언된 변수는 선언 단계와 초기화 단계가 한 번에 이루어진다.
따라서 변수 선언문 이전에 변수를 참조해도 ReferenceError가 발생하지 않는다.

```javascript
console.log(name); // undefined

var name;
console.log(name); // undefined

name = "milkyway" // 할당문에서 할당 단계가 실행된다.
console.log(name); // milkyway
```

`let`으로 선언된 변수는 선언 단계와 초기화 단계가 분리되어 진행된다.

```javascript
// 스코프의 선두에서 선언 단계가 실행됨
// 아직 변수가 초기화(메모리 공간 확보 및 undefined로 초기화)되지 않은 상태
// 따라서 변수 선언문 이전에 변수를 참조할 수 없다.

console.log(name); // ReferenceError: name is not defined

let name; // 변수 선언문에서 초기홛 단계가 실행된다.
console.log(name); // undefined

name = "milkyway"; // 할당문에서 할당 단계가 실행된다.
console.log(name); // milkyway
```

`let`과 `const`로 선언된 변수는 호이스팅되지 않는 것이 아니다.
스코프에 진입할 때 변수가 만들어지고 TDZ가 생성되지만, 코드 실행이 변수가 실제 있는 위치에 도달할 때까지 액세스할 수 없는 것이다. (초기화되지 않았기 때문에) `let`과 `const` 변수가 선언된 시점에서 제어 흐름은 TDZ를 떠난 상태가 되며, 변수를 사용할 수 있게 된다.

# 변수의 유효범위
`var`는 기본적으로 function scope를 가지게 되고
`let`과 `scope`는 block scope를 가지게 된다.

## `var`
```javascript
var name = "This is milkyway";
if (typeof name === 'string'){
    var result = true;
} else {
    var result = false;
}

console.log(result); // result : true
```

## 'let'과 `const`
```javascript
var name = "This is milkyway";
if (typeof name === 'string'){
    const result = true;
} else {
    const result = false;
}

console.log(result); // result : result is not defined
```

# 정리
변수 선언에는 기본적으로 `const`를 사용하고, 재할당이 필요한 경우에 한정하여 `let`을 사용하는 것이 좋다.
그리고 객체를 재할당하는 경우는 생각보다 흔하지 않다. `const`를 사용하면 의도치 않은 재할당을 방지해 주기 때문에 보다 안전하다.

1. 재할당이 필요한 경우에 한정해 `let`을 사용한다. 이때, 변수으 ㅣ스코프는 최대한 좁게 만든다.
2. 재할당이 필요 없는 상수와 객체에는 `const`를 사용한다.

## 참고
- [Javascript Hoisting](https://velog.io/@marcus/Javascript-Hoisting)
- [Javascript var-let-const 차이](https://velog.io/@marcus/2019-02-10-1702-%EC%9E%91%EC%84%B1%EB%90%A8)
- [var, let, const 차이점](https://velog.io/@bathingape/JavaScript-var-let-const-%EC%B0%A8%EC%9D%B4%EC%A0%90)
