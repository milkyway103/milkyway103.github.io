---
layout: post
title: 자바스크립트 전개 연산자
categories : javascript
tags : javascript spread_syntax ES6 전개연산자
comments : true
---

## 전개 연산자 (Spread syntax, ...)
- 배열 또는 객체를 하나하나 넘기는 용도로 사용된다.

```JavaScript
const arr = [1, 2, 3];
let test_arr = [4, 5, 6];
let test_arr2 = [4, 5, 6];

test_arr.push(arr);
console.log(test_arr); // [4, 5, 6, [1, 2, 3]]

// ES6
test_arr2.push(...arr);
console.log(test_arr2); // [4, 5, 6, 1, 2, 3]
```

push 메서드를 사용할 때 전개 연산자를 사용하지 않은 코드는 array 전체가 들어가 2차원 배열이 되었지만, 전개 연산자를 사용하게 되면 array 내부의 요소 하나하나가 삽입된다.

## 전개 구문의 활용
- 전개 구문을 사용하면 배열이나 문자열과 같이 반복 가능한 문자를 0개 이상의 인수 (함수로 호출할 경우) 또는 요소 (배열 리터럴의 경우)로 확장하여, 0개 이상의 키-값의 쌍으로서 객체로 확장시킬 수 있다. (Mozilla 공식 문서의 설명)
- 즉, 인수로서 어떠한 객체를 전달할 때, 해당 객체를 인수들의 모음으로써 분해하여 함수에 전달할 수 있다는 뜻이다.

1. 함수 호출
#### apply() 대체
- 숫자로만 이루어진 배열의 원소들의 합을 구하기 위해, `sum` 메서드를 사용한다고 생각해보자. 직관적으로 생각할 수 있는 메서드의 사용은 다음과 같다.

```JavaScript
function sum(x, y, z) {
  return x + y + z;
}

const numbers = [1, 2, 3];

console.log(sum(numbers));
```

위와 같이 코드를 실행하면
`1,2,3undefinedundefined`가 출력된다.

원하는 결과값 6을 얻기 위한 일반적인 방법은 `Function.prototype.apply()` 메서드를 사용하는 것이다.

> apply() 메서드는 일반적으로 특정 함수나 메서드를 호출하기 위해 객체를 원하는 값으로 명시적으로 매핑하기 위해 사용된다. 이 경우에는 arr 배열을 인수 리스트로서 넘긴다.

```JavaScript
console.log(sum.apply(null, numbers)); // output : 6
```

이를 좀 더 직관적으로 만들어주는 것이 전개 연산자이다.

```JavaScript
console.log(sum(...numbers)); // 6
```

인수 목록의 모든 인수는 전개 구문을 사용할 수 있으며, 여러 번 사용될 수도 있다.

```JavaScript
function myFunction(v, w, x, y, z) { }
var args = [0, 1];
myFunction(-1, ...args, 2, ...[3]);
// myFunction(-1, 0, 1, 2, 3) 과 같은 의미
```

### new에 적용

`new`를 사용해 생성자를 호출할 때, 배열과 `apply`를 직접 사용하는 것은 불가능했다. 하지만, 전개 구문을 통해 배열을 `new`와 함께 쉽게 사용할 수 있다.

```JavaScript
var dataFields = [1970, 0, 1]; // 1 Jan 1970
var d = new Date(...dataFields);
log(d); // Thu Jan 01 1970 00:00:00 GMT+0900 (대한민국 표준시)
```

2. 배열 리터럴에서의 전개
### 더 강력한 배열 리터럴
이미 존재하는 배열을 일부로 하는 새로운 배열을 생성하는 데에도 전개 연산자가 사용될 수 있다.

```JavaScript
var parts = ['shoulders', 'knees'];
var lyrics = ['head', ...parts, 'and', 'toes'];
log(lyrics); // ["head", "shoulders", "knees", "and", "toes"]
```

`...`은 배열 리터럴의 어디에서든 사용될 수 있으며 여려 번 사용될 수도 있다.

### 배열 복사
```JavaScript
var arr = [1, 2, 3];
var arr2 = [...arr]; // arr.slice()와 유사
arr2.push(4);

log(arr); // [1, 2, 3]
log(arr2); // [1, 2, 3, 4]
```

arr는 영향을 받지 않고, arr2만 변경된다.
하지만 `...`로서 배열을 복사할 때에, 1레벨 깊이에서만 효과적으로 동작한다. (얕은 복사) 그러므로, 다차원 배열을 복사하는 것에는 적합하지 않다.

```javascript
var a = [[1], [2], [3]];
var b = [...a];
b.shift().shift();

log(a); // [[], [2], [3]]
log(b); // [[2], [3]]
```

### 배열 연결
일반적으로 배열을 존재하는 배열의 끝에 이어 붙이는 데에는 `Array.prototype.concat()`이 사용되었다.

```JavaScript
var arr1 = [0, 1, 2];
var arr2 = [3, 4, 5];
// arr2 의 모든 항목을 arr1 에 붙임
arr1 = arr1.concat(arr2);
log(arr1); // [0, 1, 2, 3, 4, 5]
```

전개 구문을 사용하면 다음과 같다.

```JavaScript
var arr1 = [0, 1, 2];
var arr2 = [3, 4, 5];
arr1 = [...arr1, ...arr2];
log(arr1); // [0, 1, 2, 3, 4, 5]
```

전개 연산자는 `unshift()` (존재하는 배열의 시작 지점에 배열의 값들을 삽입) 역시 대체할 수 있다.

```JavaScript
var arr1 = [0, 1, 2];
var arr2 = [3, 4, 5];
Array.prototype.unshift.apply(arr1, arr2);
log(arr1); // [3, 4, 5, 0, 1, 2]
```

```JavaScript
var arr1 = [0, 1, 2];
var arr2 = [3, 4, 5];
arr1 = [...arr2, ...arr1];
log(arr1) // [3, 4, 5, 0, 1, 2]
```

3. 객체 리터럴에서의 전개 연산자 활용
전개 연산자는 배열은 물론, 객체에서도 사용될 수 있다.
그러나 역시 1차원 깊이에서만 제대로 작동한다.

```JavaScript
const obj = {
  "Name": "AJu",
  "Git":"zoz0312"
}

const test_obj = {
  "test1":1,
  "test2":2
}

const a_merge = {obj, test_obj}
const b_merge = {...obj, ...test_obj}

console.log(a_merge);
/*
{
    obj: {
        "Name":"AJu",
        "Git":"zoz0312"
    },
    test_obj: {
        "test1":1,
        "test2":2
    }
}
*/

console.log(b_merge);
/*
{
    "Name":"AJu",
    "Git":"zoz0312",
    "test1":1,
    "test2":2
}
*/
```

이처럼 객체의 복사를 하는 데에도 전개 연산자가 활용될 수 있다.

## 주의
전개 구문 (spread 프로퍼티인 경우 제외) 은 이터러블 객체에만 적용된다.

```JavaScript
var obj = {'key1': 'value1'};
var array = [...obj]; // TypeError: obj is not iterable
```

함수 호출에서 전개 구문을 사용할 때, 자바스크립트 엔진의 인수 길이 제한을 초과하지 않도록 주의해야 한다.


## 참고
- [전개 연산자](https://blog.naver.com/zoz0312/221622159150)
- [Mozilla - 전개 구문](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Operators/Spread_syntax)
- [Mozilla - apply()](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Function/apply)
