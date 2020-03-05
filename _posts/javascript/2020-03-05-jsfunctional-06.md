---
layout: post
title: 함수형 프로그래밍과 JavaScript ES6+ 6. 장바구니 예시
categories : javascript
tags : javascript functional-programming ES6
comments : true
---

> inflearn의 [함수형 프로그래밍과 JavaScript ES6+](https://www.inflearn.com/course/functional-es6)를 보고 공부한 것을 정리합니다.

## 총 수량

```javascript
const products = [
    { name: '반팔티', price: 15000, quantity: 1 },
    { name: '긴팔티', price: 20000, quantity: 2 },
    { name: '핸드폰케이스', price: 15000, quantity: 3 },
    { name: '후드티', price: 30000, quantity: 4 },
    { name: '바지', price: 25000, quantity: 5 },
];
```

```javascript
const add = (a, b) => a + b;

const total_quantity =  pipe(
    map(p => p.quantity),
    reduce(add));

log(total_quantity(products)); // 15
```

## 총 가격
```javascript
const total_price = pipe(
    map(p => p.quantity * p.price),
    reduce(add));

log(total_price(products)); // 345000
```

## 추상화 레벨 높이기
위의 두 함수는 products라는 도메인에 대해서만 사용 가능하다. 다른 도메인에 대해서도 범용적으로 사용 가능하도록 다음과 같은 함수를 만들 수 있다.

```javascript
const sum = curry((f, iter) => go(
    iter,
    map(f),
    reduce(add)
));
```

위의 함수는 함수와 이터러블을 입력받아서, 해당 이터러블에 대해 함수에 맞게 추출 (map) 하고 그것에 대한 합산을 계산하는 것이다.
이를 토대로 위의 함수들은 다음과 같이 바뀔 수 있다.

```javascript
const total_quantity2 = sum(p => p.quantity);
const total_price2 = sum(p => p.quantity * p.price);

log(total_quantity2(products));
log(total_price2(products));
```

다른 도메인에 대해서도 사용 가능하다.

```javascript
log(sum(u => u.age, [
    {age: 30},
    {age: 20},
    {age: 10}
])); // 60
```

`sum`은 이제 도메인에 구애받지 않으므로, 추상화 레벨이 훨씬 높다고 할 수 있다!

## HTML로 출력하기

### 초기 템플릿
```javascript
document.querySelector('#cart').innerHTML= '
    <table>
      <tr>
        <th>상품 이름</th>
        <th>가격</th>
        <th>수량</th>
        <th>총 가격</th>
      </tr>
      <tr>
        <td>반팔티</td>
        <td>10000</td>
        <td><input type="number" value="3"></td>
        <td>30000</td>
      </tr>
      <tr>
        <td>반팔티</td>
        <td>10000</td>
        <td><input type="number" value="3"></td>
        <td>30000</td>
      </tr>
';
```

### 총 수량, 총 가격 추가
```javascript
document.querySelector('#cart').innerHTML= '
    <table>
      <tr>
        <th>상품 이름</th>
        <th>가격</th>
        <th>수량</th>
        <th>총 가격</th>
      </tr>
      <tr>
        <td>반팔티</td>
        <td>10000</td>
        <td><input type="number" value="3"></td>
        <td>30000</td>
      </tr>
      <tr>
        <td>반팔티</td>
        <td>10000</td>
        <td><input type="number" value="3"></td>
        <td>30000</td>
      </tr>
      <tr>
        <td colspan="2">합계</td>
        <td>${total_quantity(products)}</td>
        <td>${total_price(products)}</td>
      </tr>
';
```

<center>
 <figure>
 <img src="/assets/post-img/javascript/jsfunctional6-image1.png" alt="views">
 <figcaption>초기 템플릿</figcaption>
 </figure>
 </center>

 ### 상품들의 목록을 출력하도록
 ```javascript
 document.querySelector('#cart').innerHTML= '
     <table>
       <tr>
         <th>상품 이름</th>
         <th>가격</th>
         <th>수량</th>
         <th>총 가격</th>
       </tr>
       <tr>
       ${go(products,
              map(p => `
                  <tr>
                    <td>${p.name}</td>
                    <td>${p.price}</td>
                    <td><input type="number" value="${p.quantity}"</td>
                  </tr>
              `)
          )}
       </tr>
       <tr>
         <td colspan="2">합계</td>
         <td>${total_quantity(products)}</td>
         <td>${total_price(products)}</td>
       </tr>
 ';
 ```

 <center>
  <figure>
  <img src="/assets/post-img/javascript/jsfunctional6-image2.png" alt="views">
  <figcaption></figcaption>
  </figure>
  </center>

원하는 대로 출력은 됐는데, 앞에 `````가 생겼다.
그 이유는 이것이 배열의 형태로 출력이 되었기 때문이다. log를 찍어서 확인해보면 다음과 같다.

<center>
   <figure>
   <img src="/assets/post-img/javascript/jsfunctional6-image3.png" alt="views">
   <figcaption>배열의 형태</figcaption>
   </figure>
</center>

그래서, 다음과 같이 `add` 코드를 추가하여 이 배열을 문자열로 만들어준다.

```javascript
document.querySelector('#cart').innerHTML= '
    <table>
      <tr>
        <th>상품 이름</th>
        <th>가격</th>
        <th>수량</th>
        <th>총 가격</th>
      </tr>
      <tr>
      ${go(products,
             map(p => `
                 <tr>
                   <td>${p.name}</td>
                   <td>${p.price}</td>
                   <td><input type="number" value="${p.quantity}"</td>
                 </tr>
             `),
             reduce(add))}
      </tr>
      <tr>
        <td colspan="2">합계</td>
        <td>${total_quantity(products)}</td>
        <td>${total_price(products)}</td>
      </tr>
';
```

그런데 위의 형태는 이전에 만들었던 `sum`과 같다. 그러므로 다음과 같이 바꿀 수 있다.

```javascript
document.querySelector('#cart').innerHTML= '
    <table>
      <tr>
        <th>상품 이름</th>
        <th>가격</th>
        <th>수량</th>
        <th>총 가격</th>
      </tr>
      <tr>
      ${go(products,
             sum(p => `
                 <tr>
                   <td>${p.name}</td>
                   <td>${p.price}</td>
                   <td><input type="number" value="${p.quantity}"</td>
                 </tr>
             `))}
      </tr>
      <tr>
        <td colspan="2">합계</td>
        <td>${total_quantity(products)}</td>
        <td>${total_price(products)}</td>
      </tr>
';
```

<center>
   <figure>
   <img src="/assets/post-img/javascript/jsfunctional6-image4.png" alt="views">
   <figcaption>로그로 찍은 것</figcaption>
   </figure>
</center>

### 선택된 상품들만 출력되도록

이는 장바구니이므로, 다음과 같이 선택할 수 있도록 한다.
우선 products에 `is_selected` 옵션을 추가한다.

```javascript
const products = [
    { name: '반팔티', price: 15000, quantity: 1, is_selected: true },
    { name: '긴팔티', price: 20000, quantity: 2, is_selected: false },
    { name: '핸드폰케이스', price: 15000, quantity: 3, is_selected: true },
    { name: '후드티', price: 30000, quantity: 4, is_selected: false },
    { name: '바지', price: 25000, quantity: 5, is_selected: false },
];
```

```javascript
document.querySelector('#cart').innerHTML= '
    <table>
      <tr>
        <th></th>
        <th>상품 이름</th>
        <th>가격</th>
        <th>수량</th>
        <th>총 가격</th>
      </tr>
      <tr>
      ${go(products,
             sum(p => `
                 <tr>
                   <td><input type="checkbox" ${p.is_selected ? 'checked' : ''}></td>
                   <td>${p.name}</td>
                   <td>${p.price}</td>
                   <td><input type="number" value="${p.quantity}"</td>
                 </tr>
             `))}
      </tr>
      <tr>
        <td colspan="2">합계</td>
        <td>${total_quantity(filter(p => p.is_selected, products))}</td>
        <td>${total_price(filter(p => p.is_selected, products))}</td>
      </tr>
';
```

<center>
   <figure>
   <img src="/assets/post-img/javascript/jsfunctional6-image5.png" alt="views">
   <figcaption>최종 모습</figcaption>
   </figure>
</center>
