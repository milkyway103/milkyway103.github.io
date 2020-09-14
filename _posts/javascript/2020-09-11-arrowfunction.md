---

layout: post
title: 자바스크립트 화살표 함수
categories : javascript
tags : javascript arrowfunction this

comments : true
---

화살표 함수는 단순히 함수를 '짧게' 쓰기 위한 용도로 사용되지 않는다. 자바스크립트를 사용하다 보면 저 멀리 동떨어진 곳에서 실행될 작은 함수를 작성해야 하는 상황을 자주 만나게 된다.

- `arr.forEach(func)` : `func`는 `forEach`가 호출될 때 배열 `arr`의 요소 전체를 대상으로 실행된다.
- `setTimeout(func)`: `func`는 내장 스케줄러에 의해 실행된다.

이처럼 자바스크립트에선 함수를 생성하고 그 함수를 어딘가에 전달하는 것이 아주 자연스럽다. (함수는 일급 객체)

그런데 어딘가에 함수를 전달하게 되면 함수의 컨텍스트를 잃을 수 있다. 이럴 때 화살표 함수를 사용하면 현재 컨텍스트를 잃지 않아 편리하다.

### 화살표 함수에는 'this'가 없다
`this` 값은 런타임에 결정된다. 즉, 호출되는 컨텍스트에 따라 달라진다. 동일한 함수라도 다른 객체에서 호출했다면 `this`가 참조하는 값이 달라진다.

```javascript
let user = { name: "John"};
let admin = { name: "Admin" };

function sayHi() {
  alert( this.name );
}

// 별개의 객체에서 동일한 함수를 사용함
user.f = sayHi;
admin.f = sayHi;

// 'this'는 '점(.) 앞의' 객체를 참조하기 때문에
// this 값이 달라짐
user.f(); // John (this == user)
admin.f(); // Admin (this == admin)

admin['f'](); // Admin (점과 대괄호는 동일하게 동작함)
```

규칙은 간단하다. `obj.f()`를 호출했다면 `this`는 `f`를 호출하는 동안의 `obj`이다. 위 예시에서는 `obj`가 `user`나 `admin`을 참조한다.

#### 객체 없이 호출하기

```javascript
function sayHi() {
  alert(this);
}

sayHi(); // undefined
```

위와 같은 코드를 엄격 모드에서 실행하면, `this`에는 `undefined`가 할당된다. 따라서 `this.name`으로 name에 접근하려고 하면 에러가 발생한다.

그런데 엄격 모드가 아닐 때는 **`this`가 전역 객체를 참조** 한다. **브라우저 환경에서는 `window`라는 전역 객체를 참조한다.**

이런 식의 코드는 대개 실수로 작성된 경우가 많다. 함수 본문에 `this`가 사용되었다면, 객체 컨텍스트 내에서 함수를 호출할 것이라고 예상하면 된다.

#### 자유로운 `this`가 만드는 결과
자바스크립트에서 `this`는 런타임에 결정된다. 메서드가 어디서 정의되었는지에 상관없이 `this`는 '점 앞의' 객체가 무엇인가에 따라 '자유롭게' 결정된다.

이렇게 `this`가 런타임에 결정되면 좋은 점도 있고 나쁜 점도 있다. 함수(메서드)를 하나만 만들어 여러 객체에서 재사용할 수 있다는 것은 장점이지만, 이런 유연함이 실수로 이어질 수 있다는 것은 단점이다.

개발자는 `this`의 동작 방식을 충분히 이해하고 장점을 취하면서 실수를 피하는 데에 집중해야 한다.

#### 객체 리터럴에서 'this' 사용하기

```javascript
function makeUser() {
  return {
    name: "John",
    ref: this
  };
};

let user = makeUser();

alert( user.ref.name ); // TypeError: Cannot read property 'name' of undefined
```

- `this`를 설정할 때 객체 정의가 사용되지 않기 때문에 에러가 발생한다. `this` 값은 호출 시점에 결정된다.
- 위 코드에서 `makeUser()` 내 `this`는 메서드로서 호출된 게 아니라 함수로써 호출되었기 때문에 `undefined`이다.
- `this` 값은 전체 함수이다.

```javascript
function makeUser() {
  return {
    name: "John",
    ref() {
      return this;
    }
  };
};

let user = makeUser();
alert( user.ref().name ); // John
```

- 위처럼 코드를 수정하면 `user.ref()`가 메서드가 되고 `this`는 `.` 앞의 객체가 되기 때문에 에러가 발생하지 않는다.

#### 계산기 만들기

```javascript
let calculator = {
  sum() {
    return this.a + this.b;
  },
  mul() {
    return this.a * this.b
  },
  read() {
    this.a = +prompt("a:", 0);
    this.b = +prompt("b:", 0);
  },
}

calculator.read();
alert( calculator.sum() );
alert( calculator.mul() );
```

#### 체이닝

```javascript
let ladder = {
  step: 0,
  up() {
    this.step++;
    return this;
  },
  down() {
    this.step--;
    return this;
  },
  showStep: function() { // 사다리에서 몇 번째 단에 올라와 있는지 보여줌
    alert( this.step );
    return this;
  }
};

ladder.up().up().down().up().down().showStep(); // 1
```

각 메서드에서 `this`를 반환하도록 하면 체이닝 가능

#### `this`가 없는 화살표 함수
화살표 함수는 일반 함수와는 달리 '고유한' `this`를 가지지 않는다. 화살표 함수에서 `this`를 참조하면, 화살표 함수가 아닌 '평범한' 외부 함수에서 `this` 값을 가져온다.

```javascript
let user = {
  firstName: "보라",
  sayHi() {
    let arrow = () => alert(this.firstName);
    arrow();
  }
};

user.sayHi(); // 보라
```

위 예시에서 함수 `arrow()`의 `this`는 외부 함수 `user.sayHi()`의 `this`가 된다.

별개의 `this`가 만들어지는 건 원하지 않고, 외부 컨텍스트에 있는 `this`를 이용하고 싶은 경우 화살표 함수가 유용하다.

이런 화살표 함수으 ㅣ특징은 객체의 메서드(`showList()`) 안에서 동일 객체의 프로퍼티(`students`)를 대상으로 순회를 하는 데 사용할 수 있다.

```javascript
let group = {
  title: "1모둠",
  students: ["보라", "호진", "지민"],

  showList() {
    this.students.forEach(
      student => alert(this.title + ': ' + student)
    );
  }
};

group.showList();
```

위의 `forEach`에서 화살표 함수를 사용했기 때문에 화살표 함수 본문에 있는 `this.title`은 화살표 함수 바깥에 있는 메서드인 `showList`가 가리키는 대상과 동일하다. 즉 `this.title`은 `group.title`과 같다.

위 예시에서 화살표 함수 대신 '일반' 함수를 사용하면 다음과 같은 에러가 발생한다.

```javascript
'use strict';

let group = {
  title: "1모둠",
  students: ["보라", "호진", "지민"],

  showList() {
    this.students.forEach(function(student) {
      // TypeError: Cannot read property 'title' of undefined
      alert(this.title + ': ' + student)
    });
  }
};

group.showList();
```

에러는 forEach에 전달되는 함수의 `this`가 `undefined`이기 때문에 발생한다. `alert` 함수에서 `undefined.title`에 접근하려 했기 때문에 alert 창에 에러가 출력된다.

그런데 화살표 함수는 `this` 자체가 없기 때문에 이런 에러가 발생하지 않는다.

**화살표 함수는 `new`와 함께 실행할 수 없다.**

`this`가 없기 때문 화살표 함수는 생성자 함수로 사용할 수 없다. 화살표 함수는 `new`와 함께 호출할 수 없다.

### 화살표 함수에는 'arguments'가 없다
화살표 함수는 일반 함수와는 다르게 모든 인수에 접근할 수 있게 해주는 유사 배열 객체 `arguments`를 지원하지 않는다.

이런 특징은 현재 `this` 값과 `arguments` 정보를 함께 실어 호출을 포워딩해 주는 데코레이터를 만들 때 유용하게 사용된다.

아래 예시에서 데코레이터 `defer(f, ms)`는 함수를 인자로 받고 이 함수를 래퍼로 감싸 반환하는데, 함수 `f`는 `ms` 밀리초 후에 호출된다.

```javascript
function defer(f, ms) {
  return function() {
    setTimeout(() => f.apply(this, arguments), ms)
  };
}

function sayHi(who) {
  alert('안녕, ' + who);
}

let sayHiDeferred = defer(sayHi, 2000);
sayHiDeferred("철수"); // 2초 후 "안녕, 철수" 출력
```

화살표 함수를 사용하지 않고 동일한 기능을 하는 데코레이터 함수를 만들면 다음과 같다.

```javascript
function defer(f, ms) {
  return function(...args) {
    let ctx = this;
    setTimeout(function() {
      return f.apply(ctx, args);
    }, ms);
  };
}
```

일반 함수에선 `setTimeout`에 넘겨주는 콜백 함수에서 사용할 변수 `ctx`와 `args`를 반드시 만들어줘야 한다.

### 참고
-[화살표 함수 다시 살펴보기](https://ko.javascript.info/arrow-functions)
-[메서드와 'this'](https://ko.javascript.info/object-methods)
