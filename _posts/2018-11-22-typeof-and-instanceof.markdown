---
title: "JavaScript - typeof, instanceof"
categories: 
  - JavaScript
tags:
  - JavaScript
  - typeof
  - instanceof
last_modified_at: 2018-11-22T12:04:24-04:00
toc: true
---

Primitive Type을 구분할 때엔 typeof를 사용하고, Class Type을 구분할 때엔 instaceof를 사용하면 됩니다.

typeof
-

단항연산자(unary operator)입니다. 결과값으로 인자의 Primitive Type을 String으로 반환합니다. 타입의 종류는 아래와 같습니다.
- 'undefined'
- 'boolean'
- 'number'
- 'string'
- 'object'
- 'function'

### 주의사항
null의 경우 'object'로 반환하기 때문에 null값일 경우 아래의 경우는 true가 되는 문제가 생깁니다.

~~~javascript
typeof variable
~~~

따라서 typeof를 사용할 경우 아래와 같이 처리해주어야 합니다.

~~~javascript
if(variable != null && typeof === 'object'){}
if(!variable && typeof === 'object'){}
~~~

<br>

instanceof
-

비교연산자입니다. object가 class의 인스턴스일 경우 true를 반환합니다. class가 개체의 프로토타입 체인에 있는 경우 true를 반환합니다. object가 class의 인스턴스가 아니거나 null인 경우에는 false를 반환합니다.

~~~javascript
result = object instanceof class
~~~

<br>

- - -
* 참고
  - [MDN web docs - typeof](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Operators/typeof)
  - [MSDN - instanceof](https://msdn.microsoft.com/ko-kr/library/zh0zb36z(v=vs.94).aspx)
  - [http://unikys.tistory.com](http://unikys.tistory.com/260)