지난번 살펴 보았던 객체 리터럴에 의한 객체 생성 방식 외에 객체를 생성하는 다양한 방법을 알아보자.

# 17.1 Object 생성자 함수

`new` 연산자와 함께 `Object` 생성자 함수를 호출하면 빈 객체를 생성하여 반환한다. 생성 후 동적으로 프로퍼티, 메서드를 추가하여 객체를 완성한다.

```jsx
const person = new Object();

person.name = 'Lee';
person.sayHello = function () {
  console.log('Hi');
};
```

자바스크립트는 `Object` 생성자 함수 이외에도 `String` , `Number` , `Boolean` , `Function` , `Array` , `Date` , `RegExp` , `Promise` 등의 빌트인 생성자 함수를 제공한다.

```jsx
const strObj = new String('Lee');
const numObj = new Number(123);
const boolObj = new Boolean(true);
const func = new Function('x', 'return x * x');
const arr = new Array(1, 2, 3);
const regExp = new RegExp("/ab+c/i");
const date = new Date();

console.log(typeof strObj); // string이 아니라 object
console.log(typeof numObj); // number가 아니라 object
```

<br/><br/>

# 17.2 생성자 함수

생성자 함수(constructor)란 `new` 연산자와 함께 호출하여 객체(인스턴스)를 생성하는 함수를 말한다. 생성자 함수에 의해 생성된 객체를 인스턴스(instance)라 한다.

<br/>

## 17.2.1 객체 리터럴에 의한 객체 생성 방식의 문제점

객체 리터럴에 의한 생성 방식은 직관적이고 간편하다. 하지만 동일한 프로퍼티를 갖는 객체를 여러 개 생성해야 하는 경우는 매번 새로 똑같이 작성해줘야 하기 때문에 비효율적이다.

```jsx
const circle1 = {
  radius: 5,
  getDiameter() {
    return 2 * this.radius;
	}
};

const circle2 = {
  radius: 10,
  getDiameter() {
    return 2 * this.radius;
	}
};
```

객체 리털에 의해 객체를 생성하면 프로퍼티 구조가 동일함에도 불구하고 매번 같은 프로퍼티와 메서드를 기술해야 한다. 객체가 1~2개라면 넘어갈 수 있겠지만 수십 개의 객체를 생성해야 한다면 문제가 크다.

<br/>

## 17.2.2 생성자 함수에 의한 객체 생성 방식의 장점

생성자 함수에 의한 객체 생성 방식은 인스턴스를 생성하기 위한 템플릿처럼 생성자 함수를 사용하여 프로퍼티 구조가 동일한 객체 여러 개를 간편하게 생성할 수 있다.

```jsx
function Circle(radius) {
  // 생성자 함수 내부의 this는 생성자 함수가 생성할 인스턴스를 가리킨다.
  this.radius = radius;
  this.getDiameter() {
    return 2 * this.radius;
	};
}

const circle1 = new Circle(5);
const circle1 = new Circle(10);
```

자바스크립트에서 생성자는 자바와 같은 클래스 기반 객체지향 언어의 생성자와는 다르게 그 형식이 정해져 있는 것이 아니라 일반 함수와 동일한 방법으로 생성자 함수를 정의한다.

그리고 `new` 연산자와 함께 호출하면 생성자 함수로 동작하고, `new` 연산자와 함께 호출하지 않으면 일반 함수로 동작한다.

```jsx
// 일반 함수로 호출
const circle3 = Circle(15);

// return 문이 없으므로 undefinded 반환
console.log(circle3);

// 일반 함수로 호출된 Circle의 this는 전역 객체를 가리킨다.
console.log(radius); // 15
```

### this

`this` 는 객체 자신의 프로퍼티나 메서드를 참조하기 위한 자기 참조 변수다. `this` 는 함수 호출 방식에 따라 동적으로 결정된다.

| 함수 호출 방식 | this가 가리키는 값(this 바인딩) |
| --- | --- |
| 일반 함수로서 호출 | 전역 객체 |
| 메서드로서 호출 | 메서드를 호출한 객체 |
| 생성자 함수로서 호출 | 생성자 함수가 생성할 인스턴스 |

<br/>

## 17.2.3 생성자 함수의 인스턴스 생성 과정

생성자 함수의 역할은 **인스턴스 생성**과 **인스턴스 초기화**이다. 생성자 함수가 인스턴스를 생성하는 것은 필수고, 초기화 하는것은 옵션이다.

```jsx
function Circle(radius) {
  // 초기화
  this.radius = radius;
  this.getDiameter() {
    return 2 * this.radius;
	};
}

const circle = new Circle(5);
```

생성자 함수 내부의 코드를 보면 `this` 에 프로퍼티를 추가하고 필요에 따라 전달된 인수를 초기값으로 할당하여 인스턴스를 초기화한다. 하지만 어디에도 반환문은 보이지 않는다.

`new` 연산자와 함께 호출하면 자바스크립트 다음과 같은 과정을 거쳐 암묵적으로 인스턴스를 생성하고 초기화한 후 반환한다.

### 1. 인스턴스 생성과 this 바인딩

암묵적으로 빈 객체가 생성되고, 이 빈 객체가 생성자 함수가 생성한 인스턴스다. 그리고 이 빈 객체(인스턴스)는 암묵적으로 `this` 에 바인딩된다. 이 처리는 런타임 이전에 실행된다.

```jsx
function Circle(radius) {
	// 1. 빈 객체 생성 후 this 바인딩
	console.log(this); // Circle {}

	this.radius = radius;
	this.getDiameter() {
    return 2 * this.radius;
	};
}
```

### 2. 인스턴스 초기화

생성자 함수에 기술되어 있는 코드가 한 줄씩 실행되어 `this` 에 바인딩 되어 있는 인스턴스를 초기화한다.

```jsx
function Circle(radius) {
	// 1. 빈 객체 생성 후 this 바인딩
	console.log(this); // Circle {}

	// 2 . this에 바인딩 되어 있는 인스턴스 초기화
	this.radius = radius;
	this.getDiameter() {
    return 2 * this.radius;
	};
}
```

### 3. 인스턴스 반환

생성자 함수 내부의 모든 처리가 끝나면 완성된 인스턴스가 바인딩된 `this` 가 암묵적으로 반환된다.

```jsx
function Circle(radius) {
	// 1. 빈 객체 생성 후 this 바인딩
	console.log(this); // Circle {}

	// 2 . this에 바인딩 되어 있는 인스턴스 초기화
	this.radius = radius;
	this.getDiameter() {
    return 2 * this.radius;
	};

	// 3. 완성된 인스턴스가 바인딩된 this의 암묵적 반환
}

const circle = new Circle(2); // Circle {radius: 1, getDiameter: f}
```

만약 생성자 함수 내부에 **객체** 를 반환한다면 `this` 가 반환되지 못하고 `return` 문에 명시한 객체가 반환된다. 

```jsx
function Circle(radius) {
	console.log(this); // Circle {}

	this.radius = radius;
	this.getDiameter() {
    return 2 * this.radius;
	};

	// 명시적으로 객체를 반환하면 this가 아나리 명시한 객체를 반환한다.
	return {};
}

const circle = new Circle(2); // {}
```

하지만 원시값을 반환하면 이 반환은 무시되고 `this` 가 반환된다.

```jsx
function Circle(radius) {
	console.log(this); // Circle {}

	this.radius = radius;
	this.getDiameter() {
    return 2 * this.radius;
	};

	// 명시적으로 객체를 반환하면 this가 아나리 명시한 객체를 반환한다.
	return 1;
}

const circle = new Circle(2); // Circle {radius: 1, getDiameter: f}
```

따라서 생성자 함수 내부에서는 반드시 `return` 문을 생략하는 것이 좋다.

<br/>

## 17.2.4 내부 메서드 [[Call]] 과 [[Constructor]]

함수 선언문 혹은 함수 표현식으로 정의한 함수는 일반 함수로 호출은 물론, 생성자 함수로서 호출할 수 있다.

함수는 객체이므로 일반 객체와 동일하게 동작할 수 있다. 함수 객체는 일반 객체가 가지고 있는 내부 슬롯과 내부 메서드를 모두 가지고 있기 때문이다.

또 함수는 객체이지만 일반 객체와는 다르다. **일반 객체는 호출할 수 없고, 함수는 호출할 수 있다.** 따라서 함수는 함수 객체만을 위한 [[Call]], [[Construct]] 같은 내부 메서드를 추가로 가지고 있다.

함수가 일반 함수로 호출이 되면 내부 메서드 [[Call]]이 호출되고, `new` 연산자와 함께 생성자 함수로 호출되면 내부메서드 [[Construct]]가 호출된다.

내부 메서드 [[Call]]을 갖는 함수 객체를 callable이라 하고, [[Construct]]를 갖는 함수 객체를 constructor, 갖지 않는다면 non-constructor 라고 부른다.

- callable은 호출할 수 있는 객체, 즉 함수를 말한다.
- constructor는 생성자 함수로 호출할 수 있는 함수를 말한다.
- 즉 모든 함수 객체는 반드시 callable이지만 constructor는 아닐 수 도 있다.

## 17.2.5 constructor와 non-constructor의 구분

자바스크립트 엔진은 함수 정의 방식에 따라 함수를 constructor와 non-constructor로 구분한다.

- constructor: 함수 선언문, 함수 표현식, 클래스
- non-constructor: 메서드(ES6 메서드 축약 표현), 화살표 함수

이때 주의할 것은 ECMAScript 사양에서 메서드로 인정하는 범위가 일반적인 의미의 메서드보다 좁다는 것이다.

```jsx
const baz = {
  x: function () {} // 일반 함수로 정의됨, 메서드로 인정하지 않음, constructor
}
console.log(new baz.x()); // x {}

const obj = {
  x() {} // ES6 메서드 축약표현, 메서드로 인정, non-constructor
};

console.log(new obj.x()); // Uncaught TypeError: obj.x is not a constructor
```

<br/>

## 17.2.6 new 연산자

일반 함수와 생성자 함수 사이에 특별한 형식적 차이는 없다.

단지 `new` 연산자와 함께 호출하면 생성자 함수로 동작하여 [[Call]]이 아닌 [[Construct]]가 호출된다. 따라서 `new` 연산자와 함께 호출하는 함수는 non-consturctor가 아닌 constructor이어야 한다.

```jsx
// 일반 함수
function add(x, y) {
  return x + y;
}

// 일반 함수를 생성자 함수로 호출
let inst = new add();

// 생성자 함수로 동작하여 암묵적으로 생성된 빈 객체 {}가 반환된다.
console.log(inst); // {}

// 객체를 반환하는 일반 함수
function createUser(name, role) {
  return {name, role};
}

// new 연산자와 함께 호출해도 return 문에 객체가 명시되어있어 함수가 생성한 객체를 반환
console.log(new createUser('name', 'admin')); // {name: 'Lee', role: 'admin'}

```

반대로 `new` 연산자 없이 생성자 함수를 호출하면 [[Call]]이 호출되어 일반 함수로 호출된다.

```jsx
function Circle(radius) {
	this.radius;
	this.getDiameter = function () {
		return 2 * this.radius;
	}
}

const circle = Circle(5);
console.log(circle); // undefined
console.log(radius); // 5, 일반 함수의 this는 전역 객체 window를 가리킨다.

```

이렇게 일반 함수와 생성자 함수에서 특별한 형식 차이는 없다. 따라서 생성자 함수는 일반적으로 첫 문자를 대문자로 기술하는 파스칼 케이스로 명명하여 일반 함수와 구분한다.

<br/>

## 17.2.7 new.target

생성자 함수가 `new` 연산자 없이 호출되는 것을 방지하기 위해 ES6에서는 `new.target` 을 지원한다.

함수 내부에서 `new.target` 을 사용하면 `new` 연산자와 함께 호출되었는지 확인할 수 있다.

`new` **연산자와 함께 생성자 함수로 호출되면 함수 내부의** `new.target` **은 함수 자신을 가리키고, 일반 함수로 호출되면** `new.target` **은** `undefined` **이다.**

```jsx
function Circle(radius) {
	// new와 함께 호출되었는지 확인한다.
  if (!new.target) {
		// new와 함께 호출되지 않았다면 함수를 new 연산자와 함께 재귀호출한다.
    return new Circle(radius);
	}
	this.radius;
	this.getDiameter = function () {
		return 2 * this.radius;
	}
}
```

`new` 연산자와 함께 생성자 함수로 생성된 객체는 프로토타입에 의해 생성자 함수와 연결된다. 이를 이용해 `new` 연산자와 함께 호출되었는지 확인할 수 있다.

또한 대부분의 빌트인 함수는 `new` 연산자와 함께 호출하지 않아도 `new` 연산자와 함께 호출되었는지를 확인한 후 적절한 값을 반환한다.

```jsx
let f = new Function('x', 'return x ** x');
console.log(f); // {ƒ anonymous(x) {return x ** x}}
f = FFunction('x', 'return x ** x');
console.log(f); // {ƒ anonymous(x) {return x ** x}}
```

하지만 `String` , `Number` , `Boolean` 생성자 함수는 `new` 연산자와 함께 호출하면 객체를 반환하고, `new` 연산자 없이 호출하면 문자열, 숫자, 불리언 값을 반환한다.

```jsx
const str = String(123);
console.log(typeof str); // string
```
