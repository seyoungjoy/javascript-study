# 46장 제너레이터와 async/await

## 46.1 제너레이터란?

**제너레이터**는 코드 블록의 실행을 일시 중지했다가 필요한 시점에 재개할 수 있는 특수한 함수다.

<br/>

**제너레이터와 일반 함수의 차이점**

1. 제너레이터 함수는 함수 호출자에게 함수 실행의 제어권을 양도할 수 있다.
2. 제너레이터 함수는 함수 호출자와 함수의 상태를 주고받을 수 있다.
3. 제너레이터 함수를 호출하면 제너레이터 객체를 반환한다.

<br/>

## 46.2 제너레이터 함수의 정의

- function\* 키워드로 선헌
- 하나 이상의 yield 표현식을 포함
- 화살표 함수로 정의할 수 없다.
- new 연산자와 함께 생성자 함수로 호출할 수 없다.

```jsx
// 제너레이터 함수 선언문
function* genDecFunc() {
  yield 1;
}

// 제너레이터 함수 표현식
const genExpFunc = function* () {
  yield 1;
};

// 제너레이터 메서드
const obj = {
  * genObjMethod() {
    yield 1;
  }
};

// 제너레이터 클래스 메서드
class MyClass {
  * genClsMethod() {
    yield 1;
  }
}

// 화살표 함수로 정의할 수 없음
const genArrowFunc = * () => {
  yield 1;
}; // SyntaxError: Unexpected token '*'

// new 연산자와 함께 생성자 함수로 호출 불가
function* genFunc() {
  yield 1;
}

new genFunc(); // TypeError: genFunc is not a constructor
```

<br/>

## 46.3 제너레이터 객체

제너레이터 함수를 호출하면 일반 함수처럼 함수 코드 블록을 실행하는 것이 아니라 제너레이터 객체를 생성해 반환한다. 제너레이터 함수가 반환한 제너레이터 객체는 이터러블이면서 동시에 이터레이터이다.

즉, 제너레이터 객체는 Symbol.iterator 메서드를 상속받는 이터러블이면서 value, done 프로퍼티를 갖는 이터레이터 리절트 객체를 반환하는 next 메서드를 소유하는 이터레이터다.

```jsx
// 제너레이터 함수
function* genFunnc() {
  yield 1;
  yield 2;
  yield 3;
}

// 제너레이터 함수를 호출하면 제너레이터 객체를 반환한다.
const generator = genFunc();

// 제너레이터 객체는 이터러블이면서 동시에 이터레이터다.
// 이터러블은 Symbol.iterator 메서드를 직접 구현하거나 프로토타입 체인을 통해 상속받은 객체다.
console.log(Symbol.iterator in generator); // true
// 이터레이터는 next 메서드를 갖는다.
console.log('next' in generator); // true
```

제너레이터 객체는 **next** 메서드를 갖는 이터레이터이지만 이터레이터에는 없는 **return**, **throw** 메서드를 갖는다.

- **next 메서드**를 호출하면 제너레이터 함수의 yield 표현식까지 코드 블록을 실행하고 yield된 값을 value 프로퍼티 값으로, false를 done 프로퍼티 값으로 갖는 이터레이터 리절트 객체를 반환한다.
- **return 메서드**를 호출하면 인수로 전달받은 값을 value 프로퍼티 값으로, true를 done 프로퍼티 값으로 갖는 이터레이터 리절트 객체를 반환한다.
- **throw 메서드**를 호출하면 인수로 전달받은 에러를 발생시키고 undefined를 value 프로퍼티 값으로, true를 done 프로퍼티 값으로 갖는 이터레이터 리절트 객체를 반환한다.

```jsx
function* genFunc() {
  try {
    yield 1;
    yield 2;
    yield 3;
  } catch (e) {
    console.error(e);
  }
}

const generator = genFunc();

//next 메서드 호출시
console.log(generator.next()); // {value: 1, done: false}

//return 메서드 호출시
console.log(generator.return('End!')); // {value: "End!", done: true}
```

```jsx
function* genFunc() {
  try {
    yield 1;
    yield 2;
    yield 3;
  } catch (e) {
    console.error(e);
  }
}

const generator = genFunc();

//next 메서드 호출시
console.log(generator.next()); // {value: 1, done: false}

//throw 메서드 호출시
console.log(generator.throw('Error!')); // {value: undefined, done: true}
```

<br/>

## 46.4 제너레이터의 일시 중지와 재개

- 제너레이터는 yield 키워드와 next 메서드를 통해 실행을 일시 중지했다가 필요한 시점에 다시 재개할 수 있다.
- 제너레이터는 함수 호출자에게 제어권을 양도하여 필요한 시점에 함수 실행을 재개할 수 있다.
- yield 키워드는 제너레이터 함수의 실행을 일시 중지시키거나 yield 키워드 뒤에 오는 표현식의 평가 결과를 제너레이터 함수 호출자에게 반환한다.

1. next 메서드를 호출
2. yield 표현식까지 실행되고 일시 중지됨
3. 함수의 제어권이 호출자로 양도됨
4. next 메서드는 value, done 프로퍼티를 갖는 이터레이터 리절트 객체를 반환함
   - value : yield 표현식에서 yield된 값이 할당
   - done : 제너레이터 함수가 끝까지 실행되었는지를 나타내는 불리언 값이 할당

```jsx
function* genFunc() {
  yield 1;
  yield 2;
  yield 3;
}

const generator = genFunc();

console.log(generator.next()); // {value: 1, done: false}
console.log(generator.next()); // {value: 2, done: false}
console.log(generator.next()); // {value: 3, done: false}
console.log(generator.next()); // {value: undefined, done: true}
```

- 제너레이터 객체의 next 메서드에는 인수를 전달할 수 있다.

```jsx
function* genFunc() {
  const x = yield 1;
  const y = yield x + 10;
  return x + y;
}

const generator = genFunc(0);
let res = generator.next();
console.log(res); // {value: 1, done: false}
res = generator.next(10); // x에 10 할당
console.log(res); // {value: 20, done: false}
res = generator.next(20); // y에 20 할당
console.log(res); // {value: 30, done: true}
```

```jsx
function* genFunc() {
  const x = yield 1;
  const y = yield x + 10;
  return x + y;
}

const generator = genFunc(0);
let res = generator.next();
console.log(res); // {value: 1, done: false}
res = generator.next(15); // x에 15 할당
console.log(res); // {value: 25, done: false}
res = generator.next(200); // y에 200 할당
console.log(res); // {value: 215, done: true}
res = generator.next(200);
console.log(res); // {value: undefined, done: true}
```

⇒ 이러한 제너레이터의 특성을 활용하면 비동기 처리를 동기 처리처럼 구현할 수 있다.

<br/>

## 46.6 async/await

제너레이터보다 간단하고 가독성 좋게 비동기 처리를 동기 처리처럼 동작하도록 구현할 수 있는 async/await가 도입되었다.

async/await는 프로미스를 기반으로 동작한다. 프로미스의 후속 처리 메서드 없이 마치 동기 처리처럼 프로미스가 처리 결과를 반환하도록 구현할 수 있다.

<br/>

### 46.6.1 async 함수

async 함수는 async 키워드를 사용해 정의하며 언제나 프로미스를 반환한다.

```jsx
// async 함수 선언문
async function foo(n) { return n; }
foo(1).then(v => console.log(v)); // 1

// async 함수 표현식
const bar = async function (n) { return n; };
bar(2).then(v => console.log(v)); // 2

// async 화살표 함수
const baz = async n => n;
baz(3).then(v => console.log(v)); // 3

// async 메서드
const obj = {
  async foo(n) { return n; }
};
obj.foo(4).then(v => console.log(v)); // 4

// async 클래스 메서드
class MyClass {
  async bar(n) { return n; }
}
const myClass = new MyClass();
myClass.bar(5).then(v => console.log(v)); // 5

// 클래스의 constructor 메서드는 async 메서드가 될 수 없다.
// 클래스의 constructor 메서드는 인스턴스를 반환해야 하지만 async 함수는 언제나 프로미스를 반환하기 때문
class MyClass {
  async constructor() { }
  // SyntaxError: Class constructor may not be an async method
}

const myClass = new MyClass();
```

<br/>

### 46.6.2 await 키워드

- await 키워드는 프로미스가 settled상태가 될 때까지 대기하다가 settled 상태가 되면 프로미스가 resolve한 처리 결과를 반환한다.
- await 키워드는 반드시 프로미스 앞에서 사용해야 한다.

```jsx
const fetch = require('node-fetch');

const getGithubUserName = async (id) => {
  const res = await fetch(`https://api.github.com/users/${id}`); // 1
  const { name } = await res.json();
  console.log(name);
};

getGithubUserName('ungmo2');
```

```jsx
async function foo() {
  const a = await new Promise((resolve) => setTimeout(() => resolve(1), 3000));
  const b = await new Promise((resolve) => setTimeout(() => resolve(2), 2000));
  const c = await new Promise((resolve) => setTimeout(() => resolve(3), 1000));

  console.log([a, b, c]); // [1, 2, 3]
}

foo(); // 약 6초 소요된다.

async function foo() {
  const res = await Promise.all([
    new Promise((resolve) => setTimeout(() => resolve(1), 3000)),
    new Promise((resolve) => setTimeout(() => resolve(2), 2000)),
    new Promise((resolve) => setTimeout(() => resolve(3), 1000)),
  ]);

  console.log(res); // [1, 2, 3]
}

foo(); // 약 3초 소요된다. 순차적 처리 X
```

아래 bar 함수는 앞선 비동기 처리의 결과를 가지고 다음 비동기 처리를 수행해야 하므로 순서가 보장되어야 한다. 따라서 모든 프로미스에 await 키워드를 써서 순차적으로 처리할 수밖에 없다.

```jsx
async function bar(n) {
  const a = await new Promise((resolve) => setTimeout(() => resolve(n), 3000));
  // 두 번째 비동기 처리를 수행하려면 첫 번째 비동기 처리 결과가 필요하다.
  const b = await new Promise((resolve) =>
    setTimeout(() => resolve(a + 1), 2000),
  );
  // 세 번째 비동기 처리를 수행하려면 두 번째 비동기 처리 결과가 필요하다.
  const c = await new Promise((resolve) =>
    setTimeout(() => resolve(b + 1), 1000),
  );

  console.log([a, b, c]); // [1, 2, 3]
}

bar(1); // 약 6초 소요된다.
```

<br/>

### 46.6.3 에러 처리

- async/await 에서 에러 처리는 try...catch 문을 사용할 수 있다.
- 프로미스를 반환하는 비동기 함수는 명시적으로 호출할 수 있기 때문에 호출자가 명확하다.
