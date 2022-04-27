ES6에서 도입된 스프레드 문법 `...` 은 하나로 뭉쳐 있는 여러 값들의 집합을 펼쳐서 개별적인 값들의 목록으로 만든다.

스프레드 문법을 사용할 수 있는 대상은 `Array`, `String`, `Map`, `Set` 등과 같이 `for ... of` 문으로 순회할 수 있는 이터러블에 한정된다.

```jsx
// [1, 2, 3]을 개별 요소로 분리한다.
console.log(...[1, 2, 3]); // 1 2 3

// 문자열은 이터러블이다
console.log(...'Hello'); // H e l l o

// Map과 Set은 이터러블이다.
console.log(...new Map([['a', '1'], ['b', '2']]); // ['a', '1'] ['b', '2']
console.log(...new Set([1, 2, 3]); // 1 2 3

// 이터러블이 아닌 일반 객체는 스프레드 문법의 대상이 될 수 없다.
console.log(...{ a: 1, b: 2 });
```

위 예제에서 `...[1, 2, 3]` 은 배열을 펼쳐서 요소들의 목록 1 2 3으로 만든다. 이때 1 2 3은 값이 아니라 값들의 목록이다. 즉, 스프레드 문법의 결과는 값이 아니다. 이는 스프레드 문법 `...` 이 값을 생성하는 연산자가 아님을 의미한다. 따라서 스프레드 문법의 결과는 변수에 할당할 수 없다.

따라서 스프레드 문법의 결과는 다음과 같이 쉼표로 구분한 값의 목록을 사용하는 문맥에서만 사용할 수 있다.

-   함수 호출문의 인수 목록
-   배열 리터럴의 요소 목록
-   객체 리터럴의 프로퍼티 목록

<br/>

# 35.1 함수 호출문의 인수 목록에서 사용하는 경우

배열을 펼쳐서 개별적인 값들의 목록을 만든 후, 이를 함수의 인수 목록으로 전달해야 하는 경우가 있다.

만약 `Math.max` 메서드에 숫자가 아닌 배열을 인수로 전달하면 최대값을 구할 수 없으므로 `NaN` 을 반환한다.

```jsx
Math.max([1, 2, 3]); // NaN
```

이와 같은 문제를 해결하기 위해 배열을 펼쳐서 전달해야 한다.

스프레드 문법 이전에는 `Function.protoype.apply` 를 사용했다.

```jsx
var arr = [1, 2, 3];
var max = Math.max.apply(null, arr); // 3
```

스프레드 문법을 사용하면 더 간결하고 가독성이 좋다.

```jsx
const arr = [1, 2, 3];
// Math.max(...[1,2,3]) => Math.max(1,2,3)
const max = Math.max(...arr); // 3
```

스프레드 문법은 앞에서 살펴본 Rest 파라미터와 형태가 동일하여 혼동할 수 있으므로 주의할 필요가 있다.

Rest 파라미터는 함수에 전달된 인수들의 목록을 배열로 받기 위해 매개변수 이름 앞에 `...` 을 붙이는 것이다.

따라서 Rest 파라미터와 스프레드 문법은 서로 반대의 개념이다.

```jsx
// Rest 파라미터는 인수들의 목록을 배열로 받음
function foo(...rest) {
    console.log(rest); // [1, 2, 3]
}

// foo(1, 2, 3)
foo(...[1, 2, 3]);
```

<br/>

# 35.2 배열 리터널 내부에서 사용하는 경우

스프레드 문법을 배열 리터럴에서 사용하면 더욱 간결하고 가독성 좋게 표현할 수 있다.

## 35.2.1 concat

2개의 배열을 1개의 배열로 만들고 싶은 경우

```jsx
// ES5
var arr = [1, 2].concat(3, 4);
console.log(arr); // [1, 2, 3, 4]

// ES6
const arr = [...[, 1, 2], ...[3, 4]];
console.log(arr); // [1, 2, 3, 4]
```

## 35.2.2 splice

다른 배열의 요소들을 추가하거나 제거할 경우

```jsx
// ES5
var arr1 = [1, 4];
var arr2 = [2, 3];

// 세 번째 인수 arr2를 해체하여 전달해야 한다.
Array.prototpye.splice.apply(arr1, [1, 0].concat(arr2));
console.log(arr1); // [1, 2, 3, 4]
```

```jsx
// ES6
var arr1 = [1, 4];
var arr2 = [2, 3];

arr1.splice(1, 0, ...arr2);
console.log(arr1); // [1, 2, 3, 4]
```

## 35.2.3 배열 복사

```jsx
// ES5
var origin = [1, 2];
var copy = origin.slice();

// ES6
const origin = [1, 2];
const copy = [...origin];
```

이때 복사는 둘 다 얕은 복사이다.

## 35.2.4 이터러블을 배열로 변환

```jsx
// ES5
function sum() {
	var args = Array.protoype.slice.call(arguments);

	return args.reduce(function (pre, cur) {
		return pre + cur;
	}, 0};
}

console.log(sum(1, 2, 3)); // 6

// ES6
function sum() {
	return [...arguments].reduce((pre, cur) => pre + cur, 0);
}
console.log(sum(1, 2, 3)); // 6
```

위 예제보다 나은 방법은 Rest 파라미터를 사용하는 것이다.

```jsx
const sum = (...args) => args.reduce((pre, cur) => pre + cur, 0);
```

단, 이터러블이 아닌 유사 배열 객체는 스프레드 문법의 대상이 될 수 없다.

이터러블이 아닌 유사 배열 객체를 배열로 변경하려면 `Array.from` 메서드를 사용한다.

<br/>

# 35.3 객체 리터럴 내부에서 사용하는 경우

TC39 프로세스의 stage4 단계에 제안되어 있는 스프레드 프로퍼티를 사용하면 객체 리터럴의 프로퍼티 목록에서도 스프레드 문법을 사용할 수 있다.

```jsx
const obj = { x: 1, y: 2 };
const copy = { ...obj }; // 얕은 복사
console.log(copy); // { x: 1, y: 2}
console.log(obj === copy); // false

// 객체 병합
const merged = { x: 1, y: 2, ...{ a: 3, b: 4 } };
console.log(merged); // { x: 1, y: 2, a: 3, b: 4}
```

스프레드 프로퍼티 이전에는 `Object.assign` 메서드를 사용하여 어러 개의 객체를 병합하거나 특정 프로퍼티를 변경 또는 추가했다.
