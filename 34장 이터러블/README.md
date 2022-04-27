# 34.1 이터레이션 프로토콜

ES6에서 도입된 이터레이션 프로토콜은 순회 가능한 데이터 컬렉션을 만들기 위해 ECMAScript 사양에 정의하여 미리 약속한 규칙이다.

ES6 이전에는 순회 가능한 데이터들이 통일된 규약 없이 각자 나름의 구조를 가지고 `for` 문, `for...in` 문, `forEach` 메서드 등 다양한 방법으로 순회할 수 있었다.

ES6 에서는 이터레이션 프로토콜을 준수하는 이터러블로 통일하여 `for...of` 문, 스프레드 문법, 배열 디스트럭처링 할당의 대상으로 사용할 수 있도록 일원화 했다.

이터레이션 프로토콜에는 이터러블 프로토콜과 이터레이터 프로토콜이 있다.

<p align='center'>
<img width = '400px' src='https://bl3302files.storage.live.com/y4mnLjwNqUt3MjTmVkaZsgJTXvfHGVOz5XARz9rqdGpXbPH0lIxWMYjjV3FCzt0rkbg2Hfv2PqiayzPCpzX6oxZESC3ocvF4FhSHNSHqVZBQ5EafCgwZGR2Wjg4qVVJSdQLz9OJJFJ3TDgqJKvOeivYDCYzeTLNRIAvEUz4j9W-pcUe25lvFAZPRv3KsOhKSjWJ?width=988&height=274&cropmode=none'>
</p>

<br/>

## 34.1.1 이터러블

`Symbol.iterator` 를 프로퍼티 키로 사용한 메서드를 직접 구현하거나 프로토타입 체인을 통해 상속받은 `Symbol.iterator` 메서드를 호출하면 이터레이터 프로토콜을 준수한 객체를 반환하며 이 객체를 이터러블이라 한다.

이터러블인지 확인하는 함수는 다음과 같이 구현 가능하다.

```jsx
const isIterable = (v) =>
    v !== null && typeof v[Symbol.iterator] === "function";

isIterable([]); // true
isIterable(""); // true
isIterable(new Map()); // true
isIterable(new Set()); // true
isIterable({}); // false
```

예를 들어, 배열은 이터러블이며 이터러블은 `for...of` 문으로 순회 가능하고, 스프레드 문법과 배열 디스트럭처링 할당의 대상으로 사용할 수 있다.

```jsx
const array = [1, 2, 3];

console.log(Symbol.iterator in array); // true

// for...of 가능
for (const item of array) {
    console.log(item); // 1, 2, 3
}

// 스프레드 문법 가능
console.log([...array]); // [1, 2, 3]

// 배열 디스트럭처링 할당 가능
const [a, ...rest] = array;
console.log(a, rest); // 1, [2, 3];
```

이터러블이 아닌 객체는 `for...of` , 스프레드 문법, 배열 디스트럭처링 할당이 불가능하다.

단, TC39 프로세스의 stage 4단계에 제안되어 있는 스프레드 프로퍼티 제안은 일반 객체에 스프레드 문법의 사용을 허용한다.

```jsx
const obj = { a: 1, b: 2 };
console.log({ ...obj }); // { a: 1, b: 2}
```

일반 객체도 이터러블 프로토콜을 준수하도록 구현하면 이터러블이 된다.

## 34.1.2 이터레이터

이터러블의 `Symbol.iterator` 메서드를 호출하면 이터레이터가 반환되고 **이터레이터는 `next` 메서드를 갖는다.**

이터레이터의 `next` 메서드는 이터러블의 각 요소를 순회하기 위한 포인터의 역할을 한다. 즉, `next` 메서드를 호출하면 이터러블을 순차적으로 한 단계씩 순회하며 순회 결과를 나타내는 **iterator result object** 를 반환한다.

```jsx
const array = [1, 2, 3];
const iterator = array[Symbol.iterator]();

// iterator result object는 value와 done 프로퍼티를 갖는다.
console.log(iterator.next()); // { value: 1, done: false }
console.log(iterator.next()); // { value: 2, done: false }
console.log(iterator.next()); // { value: 3, done: false }
console.log(iterator.next()); // { value: undefined, done: true }
```

<br/>

# 34.2 빌트인 이터러블

| 빌트인 이터러블                           | Symbol.iterator 메서드                |
| ----------------------------------------- | ------------------------------------- |
| Array                                     | Array.prototype[Symbol.iterator]      |
| String                                    | String.prototype[Symbol.iterator]     |
| Map                                       | Map.prototype[Symbol.iterator]        |
| Set                                       | Set.prototype[Symbol.iterator]        |
| TypedArray                                | TypedArray.prototype[Symbol.iterator] |
| arguments                                 | arguments.prototype[Symbol.iterator]  |
| DOM 컬렉션                                | NodeList.prototype[Symbol.iterator]   |
| HTMLCollection.prototype[Symbol.iterator] |                                       |

# 34.3 for ... of 문

`for ... of` 문은 이터러블을 순회하면서 이터러블의 요소를 변수에 할당한다.

```jsx
for (변수 선언문 of 이터러블) { ... }
```

`for ... of` 문은 `for ... in` 문의 형식과 매우 유사하다.

`for ... in` 문은 프로토타입 체인 상에 존재하는 모든 프로토타입의 프로퍼티 중에서 [[Enumberable]] 값이 `true` 인 프로퍼티를 순회하며 열거한다. 프로퍼티 키가 심벌인 프로퍼티는 열거하지 않는다.

`for ... of` 문은 내부적으로 이터레이터의 `next` 메서드를 호출하여 iterator result object의 `value` 프로퍼티 값을 `for ... of` 문 변수에 할당한다. `done` 프로퍼티 값이 `true` 이면 순회를 중단한다.

`for ... of` 문의 내부 동작을 `for` 문으로 표현하면 다음과 같다.

```jsx
const iterable = [1, 2, 3];
const iterator = iterable[Symbol.iterator]();

for (;;) {
    const res = iterator.next();
    if (res.done) break;
    const item = res.value;
}
```

<br/>

# 34.4 이터러블과 유사 배열 겍체

유사 배열 객체는 마치 배열처럼 인덱스로 프로퍼티 값에 접근할 수 있고, `length` 프로퍼티를 갖는 객체를 말한다.

```jsx
const arrayLike = {
    0: 1,
    1: 2,
    2: 3,
    length: 3,
};
```

유사 배열 겍체는 `Symbol.iterator` 메서드가 없기 때문에 `for ... of` 문으로 순회할 수 없다.

단, `arguments`, `NodeList`, `HTMLCollection` 은 유사 배열 객체이면서 이터러블이다. 이들은 ES6에서 이터러블이 되었다. 하지만 이터러블이 된 이후에도 `length` 프로퍼티를 가지며 인덱스로 접근할 수 있기에 유사 배열 객체이면서 이터러블인 것이다.

배열도 마찬가지로 ES6에서 이터러블이 되었다.

이터러블이 아닌 유사 배열 객체는 `Array.from` 메서드를 사용하여 배열로 간단히 변환할 수 있다.

```jsx
const arrayLike = {
    0: 1,
    1: 2,
    2: 3,
    length: 3,
};
const arr = Array.from(arrayLike);

for (let item of arr) {
    console.log(item); // 1, 2, 3
}
```

<br/>

# 34.5 이터레이션 프로토콜의 필요성

ES6에서는 순회 가능한 데이터 컬렉션을 이터레이션 프로토콜을 준수하는 이터러블로 통일하여 일원화 했다.

이터러블은 `for ... of` 문, 스프레드 문법, 배열 디스트럭처링 할당과 같은 데이터 소비자에 의해 사용되므로 데이터 공급자의 역할을 한다고 할 수 있다.

만약 다양한 데이터 공급자가 각자의 순회 방식을 갖는다면 데이터 소비자는 다양한 데이터 공급자의 순회 방식을 모두 지원해야 하고 이는 효율적이지 않다.

즉 이터레이션 프로토콜은 하나의 순회 방식을 갖도록 규정하는 **데이터 소비자와 데이터 공급자를 연결하는 이터페이스의 역할을 한다.**

<br/>

# 34.6 사용자 정의 이터러블

## 34.6.1 사용자 정의 이터러블 구현

일반 객체도 이터레이션 프로토콜을 준수하도록 구현하면 사용자 정의 이터러블이 된다.

```jsx
// 피보나치 수열을 구현한 사용자 정의 이터러블
const fibonacci = {
    [Symbol.iterator]() {
        let [pre, cur] = [0, 1];
        const max = 10; // 수열의 최댓값

        return {
            next() {
                [pre, cur] = [cur, pre + cur];
                return { value: cur, done: cur >= max };
            },
        };
    },
};

for (const num of fibonacci) {
    console.log(num); // 1 2 3 5 8
}

// 이터러블은 스프레드 문법의 대상이 될 수 있다.
const arr = [...fibonacci];

// 이터러블은 배열 디스트럭처링 할당의 대상이 될 수 있다.
const [first, second, ...rest] = fibonacci;
```

## 34.6.2 이터러블을 생성하는 함수

앞서 살펴본 코드에서 내부 수열의 최댓 값 `max` 를 전달받아 이터러블을 반환해보자.

```jsx
// 피보나치 수열을 구현한 사용자 정의 이터러블
const fibonacciFunc = function (max = 10) {
    let [pre, cur] = [0, 1];

    return {
        [Symbol.iterator]() {
            return {
                next() {
                    [pre, cur] = [cur, pre + cur];
                    return { value: cur, done: cur >= max };
                },
            };
        },
    };
};

for (const num of fibonacciFunc(10)) {
    console.log(num); // 1 2 3 5 8
}
```

## 34.6.3 이터러블이면서 이터레이터인 객체를 생성하는 함수

앞서 살펴본 `fibonacciFunc` 함수는 이터러블을 반환한다. 만약 이터레이터를 생성하려면 이터러블의 `Symbol.iterator` 메서드를 호출해야 한다.

이터러블이면서 이터레이터인 객체를 생성하면 `Symbol.iterator` 메서드를 호출하지 않아도 된다.

```jsx
// 이터레이터와 next 메서드를 소유한다.
{
	[Symbol.iterator]() { return this; },
	next() {
		return { value: any, done: boolean };
	}
}
```

앞서 살펴본 `fibonacciFunc` 함수를 이터러블이면서 이터레이터인 객체를 생성하여 반환하는 함수로 변경해보자.

```jsx
// 피보나치 수열을 구현한 사용자 정의 이터러블
const fibonacciFunc = function (max = 10) {
    let [pre, cur] = [0, 1];

    return {
        [Symbol.iterator]() {
            return this;
        },
        next() {
            [pre, cur] = [cur, pre + cur];
            return { value: cur, done: cur >= max };
        },
    };
};

let iter = fibonacciFunc(10);

for (const num of iter) {
    console.log(num); // 1 2 3 5 8
}

iter = fibonacciFunc(10); // 초기화
console.log(iter.next()); // { value: 1, done: false}
```

## 34.6.4 무한 이터러블과 지연 평가

무한 이터러블을 생성하는 함수를 정의하여 무한 수열을 간단히 구현할 수 있다.

```jsx
// 무한 이터러블을 생성하는 함수
const fibonacciFunc = function () {
    let [pre, cur] = [0, 1];

    return {
        [Symbol.iterator]() {
            return this;
        },
        next() {
            [pre, cur] = [cur, pre + cur];
            // 무한을 구현해야 하므로 done 프로퍼티를 생략한다.
            return { value: cur };
        },
    };
};

// fibonacciFunc 함수는 무한 이터러블을 생성한다.
for (const num of fibonacciFunc()) {
    if (num > 10000) break;
    console.log(num); // 1 2 3 5 8...4181 6765
}

// 배열 디스트럭처링 할당을 통해 무한 이터러블에서 3개의 요소만 취득한다.
const [f1, f2, f3] = fibonacciFunc();
console.log(f1, f2, f3); // 1 2 3
```

배열이나 문자열 등은 모든 데이터를 메모리에 미리 확보한 다음 데이터를 공급한다.

하지만 위 예제의 이터러블은 **지연 평가**를 통해 데이터를 생성한다. 지연 평가는 데이터가 필요한 시점 이전까지는 미리 데이터를 생성하지 않다가 데이터가 필요한 시점이 되면 그때야 비로소 데이터를 생성하는 기법이다.

즉, 평가 결과가 필요할 때까지 평가를 늦추는 기법이 지연 평가다.

`next` 메서드가 호출되기 이전까지는 데이터를 생성하지 않는다.

이처럼 지연 평가를 사용하면 빠른 실행속도를 기대할 수 있고 불필요한 메모리를 소비하지 않으며 무한도 표현할 수 있다는 장점이 있다.
