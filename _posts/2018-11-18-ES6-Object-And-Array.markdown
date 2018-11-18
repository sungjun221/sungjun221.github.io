---
title: "ES6 객체와 배열"
tags:
  - JavaScript
  - ES6
last_modified_at: 2018-11-18T12:04:24-04:00
toc: true
---
구조 분해 할당(Destructuring assignment)
-
배열이나 객체의 속성을 해체하여 그 값을 개별 변수에 담을 수 있게 하는 자바스크립트 표현식(expression)입니다.
```javascript
// 배열 구조 분해
var a, b, rest;
[a, b, ...rest] = [10, 20, 30, 40, 50];
console.log(a); // 10
console.log(b); // 20
console.log(rest); // [30, 40, 50]

// 객체 구조 분해
({a, b, ...rest} = {a: 10, b: 20, c: 30, d: 40});
console.log(a); // 10
console.log(b); // 20
console.log(rest); //{c: 30, d: 40}
```
<br>
<br>

개선된 객체 리터럴(Enhanced object literal)
-

- 리터럴 표기법(literal notation)이란?
 
객체를 생성하는 방법은 new Object, Object.create와 리터럴 표기법이 있습니다. 리터럴 표기법은 new를 생략하여 객체를 생성하는 것입니다. 내부적으로는 new Object와 동일하게 동작하나 더 간결한 코드를 작성할 수 있습니다.
```javascript
var str = 'sungjun'  // var str = new String('sungjun');
var num = 221;       // var num = new Number(221);
```

<br>

### 속성 초기화 축약 (Shorthand for Initializing Properties)
속성명와 변수명이 같으면 속성명을 생략할 수 있습니다.
```javascript
function getOrder(customer, product, price) {
    return {
        customer,    // customer: customer
        product,     // product: product  
        price        // price: price
    }
}

getOrder("김성준", "냉장고", 1000000);
```

<br>

### 함수 작성 축약 (Shorthand for writing Methods)
속성에 함수를 작성할 때 function 예약어를 생략할 수 있습니다.
```javascript
function getOrder(customer, product, price) {
    return{
        sayProduct() {		// sayModel: function() { ...
            return product;
        }
    }
}

getOrder("김성준", "냉장고", 1000000);
```

<br>

스프레드 연산자 (Spread Operator)
-

<br>
<br>

프라미스 (Promise)
-

- - -
* 참고
  - [MDN WEB Docs - destructuring assignment](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Operators/Destructuring_assignment)
  - [DEV.to - Enhanced Object Literals in ES6](https://dev.to/sarah_chima/enhanced-object-literals-in-es6-a9d)