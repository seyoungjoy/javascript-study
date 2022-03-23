# 18.1 일급 객체

다음 조건을 만족하는 객체를 **일급 객체**라 한다.

1. 무명의 리터럴로 생성할 수 있다. 즉, 런타임에 생성이 가능하다.
2. 변수나 자료구조등에 저장할 수 있다.
3. 함수의 매개변수에 전달할 수 있다.
4. 함수의 반환값으로 사용할 수 있다.

자바스크립트의 함수는 위의 조건을 모두 만족하므로 일급 객체다.

```jsx
// 1. 함수는 무명의 리털로 생성할 수 있다.
// 2. 함수는 변수에 저장할 수 있다.
const increase = function (num) {
	return ++num;
};

const decrease = function (num) {
	return --num;
};

// 2. 함수는 객체에 저장할 수 있다.
const auxs = { increase, decrease };

// 3. 함수의 매개변수에 전달할 수 있다.
// 4. 함수의 반환값으로 사용할 수 있다.
function makeCounter(aux) {
	let num = 0;
	
	return function () {
		num = aux(num);
		return num;
	};
}

// 3. 함수는 매개변수에 함수를 전달할 수 있다. 
const increaser = makeCounter(auxs.increase); // 클로저
console.log(increaser()); // 1
```

함수가 일급 객체라는 것은 함수를 객체와 동일하게 사용할 수 있다는 의미고, 값을 사용할 수 있는 곳이라면 어디서든지 리터럴로 정의할 수 있다는 것이다.

일급 객체로서 함수가 가지는 가장 큰 특징은 함수의 매개변수로 전달할 수 있고, 반환값으로도 사용할 수 있다는 것이다.

<br/><br/>

# 18.2 함수 객체의 프로퍼티

함수는 객체이므로 함수도 프로퍼티를 가질 수 있다.

```jsx
function square(number) {
	return number * number;
}

console.log(Object.getOwnPropertyDescriptors(square));
```
<p align='center'>
<img src='https://bl3302files.storage.live.com/y4mCuiy5dN6lDlK4yOz5YAiPwibxN7yD6cz6zTEyZY50bnD4r5qDHeFVc1UqIOQ1ljaGN-fUEsZfQx3BX0zAY9Ze04-EOZsJcH4S72CfhnBrDbCQ8i0Tv2cs4i-pVhnSm--UiwHsC9agyWBoYmrieDclmYemunynmA9V9VFcz_ZkvS0Np7mmkz4PaCNIXarWwyh?width=1188&height=262&cropmode=none'>
</p>

<br/>

## 18.2.1 arguments 프로퍼티

arguments 프로퍼티 값은 `arguments` 객체다. `arguments` 객체는 함수 호출시 전달된 인수의 정보를 담고있는 iterable한 유사 배열 객체이며, 함수 내부에서 지역 변수처럼 사용된다.

`Function` 객체의 arguments 프로퍼티는 ES3부터 표준에서 폐지되었으므로 `Function.arguments` 와 같은 사용법은 권장되지 않으며 함수 내부에서 지역 변수처럼 사용할 수 있는 `argumetns` 객체를 참조하도록 한다.

자바스크립트는 매개변수와 인수의 개수가 일치하는지 확인하지 않고, 매개변수 만큼 인수를 전달하지 않아도 에러가 발생하지 않는다.

```jsx
function multiply(x, y) {
	console.log(arguments);
    return x * y;
}
console.log(multiply());
console.log(multiply(1));
console.log(multiply(1, 2));
console.log(multiply(1, 2, 3));
```

<p align='center'>
<img src='https://bl3302files.storage.live.com/y4mqLKQCg69N9D1e5K3yigpnDIsHN7kBFtS44FM74JA7Cra1AVX6GUhdqBwXtSVVH7uK6BqgHc_A_IHVtLdh-EONCT96X1UqX0GG1r8nvwRwPY08e_FiQSRADzjNosQUDEefSFr2WNsGKllnPEac74cZZHaJ_ChK2_1kxGu7CcB536-_7-jv8CKO6c0r0AHNrnk?width=938&height=560&cropmode=none'>
</p>

`arguments` 객체는 인수를 프로퍼티 값으로 소유하며, 프로퍼티 키는 인수의 순서를 나타낸다. `callee` 프로퍼티는 함수 자신을 가리키고, `length` 프로퍼티는 인수의 개수를 가리킨다. `Symbol.iterator` 프로퍼티는 객체를 iterable로 만들기 위한 프로퍼티다. 이는 34장에서 자세히 살펴본다.

`arguments` 객체는 함수의 인수의 개수를 확인하고 이에 따라 동작을 달리할 때와 매개변수 개수를 확정할 수 없는 **가변 인자 함수**를 구현할 때 유용하다.

```jsx
function sum() {
  let res = 0;
  for (let i = 0; i < arguments.length; i++) {
		res += arguments[i];
	}
	return res;
}

console.log(sum()); // 0
console.log(sum(1, 2)); // 3
console.log(sum(1, 2, 3)); // 6
```

`arguments` 객체는 배열 형태로 정보를 담고 있지만 실제 배열이 아닌 유사 배열 객체다.

- 유사 배열 객체: `length` 프로퍼티를 가진 객체로 `for` 문으로 순회할 수 있는 iterable한 객체

따라서 객체는 배열이 아니므로 배열 메서드를 사용할 경우 에러가 발생한다. 배열 메서드를 사용하기 위해서는 `Function.prototype.call`, `Function.prototype.apply` 를 사용해 간접 호출해야하는 번거로움이 있다. 이러한 번거로움을 해결하기 위해 ES6에서 Rest 파라미터를 도입했다.

이는 22장, 26장에서 더 살펴본다.

<br/>

## 18.2.2 caller 프로퍼티

`caller` 프로퍼티는 ECMAScript 사용에 포함되지 않은 비표준 프로퍼티고, 이후 표준화 될 예정도 없다.

`caller` 프로퍼티는 함수 자신을 호출한 함수를 가리킨다.

```jsx
function foo(func) {
	func();
}

function bar() {
	console.log(bar.caller);
}

foo(bar); // function foo ...
bar(); // null
```

<br/>

## 18.2.3 length 프로퍼티

`length` 프로퍼티는 함수를 정의할 때 선언한 매개변수의 개수를 가리킨다.

```jsx
function foo() {}
console.log(foo.length); // 0

function bar(x) {}
console.log(bar.length); // 1
```

`arguments` 객체의 `length` 와 함수 객체의 `length` 값은 다를 수 있으므로 주의한다!

<br/>

## 18.2.4 name 프로퍼티

ES6에 정식 표준이 된 `name` 프로퍼티는 함수 이름을 나타낸다. `name` 프로퍼티는 ES5와 ES6에서 다르게 동작한다. 익명 함수 표현식의 경우 ES5 에서는 `name` 프로퍼티가 빈 문자열을 갖고, ES6에서는 함수 객체를 가리키는 식별자를 값으로 갖는다.

```jsx
var func = function () {};
console.log(func.name); // func

var namedFunc = function foo() {};
console.log(namedFunc.name); // foo
```

<br/>

## 18.2.5 \_\_protot\_\_ 접근자 프로퍼티

모든 객체는 [[Prototype]]이라는 내부 슬롯을 갖는다. `__proto__` 프로퍼티는 [[Prototype]] 내부 슬롯이 가리키는 프로토타입 객체에 접근하기 위해 사용하는 접근자 프로퍼티다. [[Prototype]] 내부 슬롯에는 직접 접근할 수 없으며, `__proto__` 접근자 프로퍼티를 통해서만 간접적으로 접근할 수 있다.

<br/>

## 18.2.6 prototype 프로퍼티

`prototype` 프로퍼티는 생성자 함수로 호출할 수 있는 객체, 즉 constructor만이 소유하는 프로퍼티다. non-constructor는 `prototype` 프로퍼티가 없다.

`prototype` 프로퍼티는 함수가 생성자 함수로 호촐될 때 생성자 함수가 생성할 인스턴스의 프로토타입 객체를 가리킨다.