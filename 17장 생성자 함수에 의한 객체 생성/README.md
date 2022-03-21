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

# 17.2 생성자 함수

생성자 함수(constructor)란 `new` 연산자와 함께 호출하여 객체(인스턴스)를 생성하는 함수를 말한다. 생성자 함수에 의해 생성된 객체를 인스턴스(instance)라 한다.

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