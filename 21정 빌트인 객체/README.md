# 21장. 빌트인 객체

## 21.1 자바스크립트 객체의 분류

- **표준 빌트인 객체** : ECMAScript 사양에 정의된 객체를 말하며, 자바스크립트 실행환경과 관계없이 언제나 사용할 수 있다. 별도의 선언 없이 전역 변수처럼 언제나 참조할 수 있다.
- **호스트 객체** : 자바스크립트 실행 환경에서 추가로 제공하는 객체를 말한다.
- **사용자 정의 객체** : 사용자가 직접 정의한 객체를 말한다.

<br>

## 21.2 표준 빌트인 객체

자바스크립트는 Object, String, Number, Boolean, Symbol, Date, Math, RegExp, Array, Map/Set, WeakMap/WeakSet, Function, Promise, Reflect, Proxy, JSON, Error 등 40여 개의 표준 빌트인 객체를 제공한다.

- **생성자 함수 객체인 표준 빌트인 객체** : 프로토타입 메서드와 정적 메서드 제공. 인스턴스 생성 가능.
  ex) Math, Reflect, JSON을 제외한 표준 빌트인 객체  

- **생상자 함수 객체가 아닌 표준 빌트인 객체** : 정적 메서드만 제공.

<br>

```jsx
// 표준 빌트인 객체인 String, Number, Boolean, Function, Array, Date는
// 생성자 함수로 호출하여 인스턴스를 생성할 수 있다.

// String 생성자 함수에 의한 String 객체 생성
const strObj = new String('Lee'); // String {"Lee"}
console.log(typeof strObj); // object

// Number 생성자 함수에 의한 Number 객체 생성
const numObj = new Number(123); // Number {123}
console.log(typeof numObj); // object

// Boolean 생성자 함수에 의한 Boolean 객체 생성
const boolObj = new Boolean(true); // Boolean {true}
console.log(typeof boolObj); // object

// Function 생성자 함수에 의한 Function 객체(함수) 생성
const func = new Function('x', 'return x * x'); // ƒ anonymous(x )
console.log(typeof func); // function

// Array 생성자 함수에 의한 Array 객체(배열) 생성
const arr = new Array(1, 2, 3); // (3) [1, 2, 3]
console.log(typeof arr); // object

// RegExp 생성자 함수에 의한 RegExp 객체(정규 표현식) 생성
const regExp = new RegExp(/ab+c/i); // /ab+c/i
console.log(typeof regExp); // object

// Date 생성자 함수에 의한 Date 객체 생성
const date = new Date(); // Fri May 08 2020 10:43:25 GMT+0900 (대한민국 표준시)
console.log(typeof date); // object
```

<br>

표준 빌트인 객체인 String을 생성자 함수로서 호출하여 생성한 String 인스턴스의 프로토타입은 String.prototype이다.

```jsx
// String 생성자 함수에 의한 String 객체 생성
const strObj = new String('Lee'); // String {"Lee"}

// String 생성자 함수를 통해 생성한 strObj 객체의 프로토타입은 String.prototype이다.
console.log(Object.getPrototypeOf(strObj) === String.prototype); // true
```

<br>

Number.prototype은 다양한 기능의 빌트인 프로토타입 메서드를 제공한다.

- **toFixed()** : Number.prototype의 프로토타입 메서드다. **소수점 자리를 반올림하여 문자열로 반환**한다.
- **isInteger()** : Number의 정적 메서드다. **인수가 정수인지 검사**하여 그 결과를 Boolean으로 반환한다.

<br>

```jsx
// Number 생성자 함수에 의한 Number 객체 생성
const num = new Number(1.5); // Number {1.5}

// toFixed()
console.log(num.toFixed()); // 2

// isInteger()
console.log(Number.isInteger(0.5)); // false
```

**아래 코드의 출력 결과는 어떻게 나올까?**

```jsx
const own = new Number(1);
const two = Number(2);
const three = 3;

console.log(Number.isInteger(own)); // ?
console.log(Number.isInteger(two)); // ?
console.log(Number.isInteger(three)); // ?
```

new 생성자를 사용하지 않고 생성할 경우 객체를 반환하지 않고 원시 타입을 반환한다.

즉, own의 타입은 Object이고 two, three는 Number 타입이기 때문에 `Number.isInteger(own)` 는 false 가 맞다.

<br>

## 21.3 원시값과 래퍼 객체

문자열이나 숫자, 불리언 등의 원시값이 있는데도 표준 빌트인 생성자 함수가 존재하는 이유는?

- 원시값은 객체가 아니므로 프로퍼티나 메서드를 가질 수 없는데도 원시값인 문자열이 마치 객체처럼 동작한다.

```jsx
const str = 'hello';

// 원시 타입인 문자열이 프로퍼티와 메서드를 갖고 있는 객체처럼 동작한다.
// 원시 타입인 문자열이 래퍼 객체인 String 인스턴스로 변환된다.
console.log(str.length); // 5
console.log(str.toUpperCase()); // HELLO
```

- 마침표 표기법으로 접근하면 자바스크립트 엔진이 일시적으로 원시값과 연관된 객체로 변환 해주기 때문이다.
- 원시값을 객체처럼 사용하면 자바스크립트 엔진은 암묵적으로 연관된 객체를 생성하여 생성된 객체로 프로퍼 티에 접근하거나 메서드를 호출하고 다시 원시값으로 되돌린다.
- 이처럼 객체처럼 접근하면 생성되는 임시 객체를 **래퍼 객체**라 한다으로 되돌린다.

```jsx
// 1. 식별자 str은 문자열을 값으로 가지고 있다.
const str = 'hello';

// 2. 식별자 str은 암묵적으로 생성된 래퍼 객체를 가리킨다.
// 식별자 str의 값 'hello'는 래퍼 객체의 [[StringData]] 내부 슬롯에 할당된다.
// 래퍼 객체에 name 프로퍼티가 동적 추가된다.
str.name = 'Lee';

// 3. 식별자 str은 새롭게 암묵적으로 생성된 래퍼 객체를 가리킨다.
// 새롭게 생성된 래퍼 객체에는 name 프로퍼티가 존재하지 않는다.
console.log(str.name); // undefined

// 4. 식별자 str은 다시 원래의 문자열, 즉 래퍼 객체의 [[StringData]] 내부 슬롯에 할당된 원시값을 갖는다.
// 이때 4. 에서 생성된 래퍼 객체는 아무도 참조하지 않는 상태이므로 가비지 컬렉션의 대상이 된다.
console.log(typeof str, str); // string hello
```

<br>

## 21.4 전역 객체

- **전역 객체**는 코드가 실행되기 이전 단계에 자바스크립트 엔진에 의해 어떤 객체보다도 먼저 생성되는 특수한 객체, 어떤 객체에도 속하지 않은 모든 빌트인 객체(표준 빌트인 객체와 호스트 객체)의 최상위 객체다.
- **전역 객체**는 개발자가 의도적으로 생성할 수 없다.
- **전역 객체**의 프로퍼티를 참조할 때 window(또는 global)를 생략할 수 있다.
  - 브라우저 환경에서의 전역 객체 : window(또는 self, this,frames)
  - Node.js 환경에서의 전역 객체 : global
  - globalThis : ES11 에서 도입된 브라우저 환경과 Node.js 환경에서 전역 객체를 가리키던 다양한 식별자를 통일한 식별자다.
- **전역 객체**는 모든 표준 빌트인 객체를 프로퍼티로 가지고 있다.
- 자바스크립트 실행 환경에 따라 추가적으로 프로퍼티와 메서드를 갖는다.
- var 키워드로 선언한 전역 변수와 선언하지 않은 변수에 값을 할당한 암묵적 전역. 그리고 전역 함수는 전역 객체의 프로퍼티가 된다.

```jsx
// var 키워드로 선언한 전역 변수
var foo = 1;
console.log(window.foo); // 1

// 선언하지 않은 변수에 값을 암묵적 전역. bar는 전역 변수가 아니라 전역 객체의 프로퍼티다.
bar = 2;
console.log(window.bar); // 2

// 전역 함수
function a() {
  return 3;
}
console.log(window.a()); // 3
```

- let 이나 const 키워드로 선언한 전역 변수는 전역 객체의 프로퍼티가 아니다.

```jsx
let foo = 123;
console.log(window.foo); // undefined
```

- 전역 객체의 프로퍼티와 메서드는 전역 객체를 가리키는 식별자(window, global) 을 생략하여 참조/호출할 수 있으므로 전역 변수와 전역 함수처럼 사용할 수 있다.

<br>

### 21.4.1 빌트인 전역 프로퍼티

**빌트인 전역 프로퍼티**는 전역 객체의 프로퍼티를 의미한다.

- **Infinity 프로퍼티** : 무한대를 나타내는 숫자값 Infinity를 갖는다.
- **NaN 프로퍼티** : 숫자가 아님을 나타내는 숫자값 NaN을 갖는다.
- **undefined 프로퍼티** : 원시타입 undefined를 값으로 갖는다.

<br>

### 21.4.2 빌트인 전역 함수

**빌트인 전역 함수**는 애플리케이션 전역에서 호출할 수 있는 빌트인 함수로서 전역 객체의 메서드다.

- **eval 함수** : 주어진 문자열 코드를 런타임에 평가 또는 실행한다. (추천하지 않음)
- **isFinite 함수** : 전달받은 인수가 정상적인 유한수인지 확인하고 그 결과를 반환한다.
- **isNaN 함수** : 전달받은 인수가 NaN인지 확인하고 그 결과를 반환한다.
- **parseFloat 함수** : 전달받은 문자열 인수를 실수로 해석하여 반환한다.
- **parseInt 함수** : 전달받은 문자열 인수를 정수로 해석하여 반환한다.
- **encodeURI 함수** : 완전한 URI를 문자열로 전달받아 이스케이프 처리를 위해 인코딩한다.
- **decodeURI 함수** : 인코딩된 URI를 인수로 전달받아 이스케이프 처리 이전으로 디코딩한다.
- **encodeURIComponent 함수** : URI 구성 요소를 인수로 전달받아 인코딩한다.
- **decodeURIComponent 함수** : 매개변수로 전달된 URI 구성 요소를 디코딩한다.
