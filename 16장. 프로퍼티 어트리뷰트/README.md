# 16장. 프로퍼티 어트리뷰트

## 16.1 내부 슬롯과 내부 메서드

내부 슬롯과 내부 메서드는 자바스크립트 엔진의 구현 알고리즘을 설명하기 위해 ECMAScript 사양에서 사용하는 의사 프로퍼티와 의사 메서드다.
이중 대괄호 `([[…]])` 로 감싼 이름들이 내부 슬롯과 내부 메서드다.

내부 슬롯과 내부 메서드에 직접적으로 접근하거나 호출할 수는 없지만 간접적으로 접근할 수 있는 수단이 있다.
`[[Prototype]]` 내부 슬롯의 경우, `__proto__` 를 통해 간접적으로 접근할 수 있다.

```jsx
const o = {};

// 내부 슬롯은 자바스크립트 엔진의 내부 로직이므로 직접 접근할 수 없다.
o[[Prototype]]; // -> Uncaught SyntaxError: Unexpected token '['
// 단, 일부 내부 슬롯과 내부 메서드에 한하여 간접적으로 접근할 수 있는 수단을 제공하기는 한다.
o.__proto__; // -> Object.prototype
/*
{constructor: ƒ, __defineGetter__: ƒ, __defineSetter__: ƒ, hasOwnProperty: ƒ, __lookupGetter__: ƒ, …}
*/
```

## 16.2 프로퍼티 어트리뷰트와 프로퍼티 디스크립터 객체

프로퍼티를 생성할 때 프로퍼티의 상태를 나타내는 프로퍼티 어트리뷰트를 기본값으로 자동 정의한다.

**프로퍼티 상태** : 프로퍼티의 값(value), 값의 갱신 가능 여부(writable), 열거 가능 여부(enumerable), 재정의 가능 여부(configurable)
**프로퍼티 어트리뷰트** : 자바스크립트 엔진이 관리하는 내부 상태 값(meta-property)인 내부 슬롯 `[[Value]]` , `[[Writable]]` , `[[Enumerable]]` , `[[Configurable]]`

**Object.getOwnPropertyDescriptor 메서드**
프로퍼티 어트리뷰트에 간접적으로 확인할 수 있는 메서드인데, 첫번째 매개변수에는 객체의 참조를 전달하고, 두번째 매개변수에는 프로퍼티 키를 문자열로 전달한다. 프로퍼티 어트리뷰트 정보를 제공하는 프로퍼티 디스크립터 객체를 반환한다.

```jsx
const person = {
  name: 'Lee',
};

// 프로퍼티 어트리뷰트 정보를 제공하는 프로퍼티 디스크립터 객체를 반환한다.
console.log(Object.getOwnPropertyDescriptor(person, 'name'));
// {value: "Lee", writable: true, enumerable: true, configurable: true}
```

ES8에서 도입된 Object.getOwnPropertyDescriptors 메서드는 모든 프로퍼티의 프로퍼티 어프리뷰트 정보를제공하는 프로퍼티 디스크립터 객체들을 반환한다.

```jsx
const person = {
  name: 'Lee',
};

// 프로퍼티 동적 생성
person.age = 20;

// 모든 프로퍼티의 프로퍼티 어트리뷰트 정보를 제공하는 프로퍼티 디스크립터 객체들을 반환한다.
console.log(Object.getOwnPropertyDescriptors(person));
/*
{
  name: {value: "Lee", writable: true, enumerable: true, configurable: true},
  age: {value: 20, writable: true, enumerable: true, configurable: true}
}
*/
```

## 16.3 데이터 프로퍼티와 접근자 프로퍼티

**데이터 프로퍼티** : 키와 값으로 구성된 일반적인 프로퍼티
**접근자 프로퍼티** : 다른 데이터 프로퍼티의 값을 읽거나 저장할 때 호출되는 접근자 함수로 구성된 프로퍼티

### 16.3.1 데이터 프로퍼티

이 프로퍼티 어트리뷰트는 자바스크립트 엔진이 프로퍼티를 생성할 때 기본값으로 자동 정의된다.

| 프로퍼티 어트리뷰트 | 프로퍼티 디스크립터 객체의 프로퍼티 | 설명 |
| --- | --- | --- |
| [[value]] | value | - 프로퍼티 키를 통해 프로퍼티 값에 접근하면 반환되는 값 - 프로퍼티 키를 통해 값을 변경하면 [[value]]에 값을 재할당. |
| [[Writable]] | writable | - 프로퍼티 값의 변경 가능 여부를 나타냄 - 값은 불리언 타입 - false인 경우 값을 변경할 수 없는 읽기 전용 프로퍼티가 됨. |
| [[Enumerable]] | enumerable | - 프로퍼티의 열거 가능 여부를 나타냄. - 값은 불리언 타입 - false인 경우 for...in문이나 Object.keys 메서드 등으로 열거할 수 없음. |
| [[configurale]] | configurable | - 프로퍼티의 재정의 가능 여부를 나타냄 - 값은 불리언 타입 - false인 경우 해당 프로퍼티의 삭제, 프로퍼티 어트리뷰트 값의 변경이 금지됨. |

```jsx
const person = {
  name: 'Lee',
};

// 프로퍼티 어트리뷰트 정보를 제공하는 프로퍼티 디스크립터 객체를 취득한다.
console.log(Object.getOwnPropertyDescriptor(person, 'name'));
// {value: "Lee", writable: true, enumerable: true, configurable: true}

// 프로퍼티 동적 생성
person.age = 20;

console.log(Object.getOwnPropertyDescriptors(person));
/*
{
  name: {value: "Lee", writable: true, enumerable: true, configurable: true},
  age: {value: 20, writable: true, enumerable: true, configurable: true}
}
*/
```

프로퍼티가 생성될 때 `[[value]]` 의 값은 프로퍼티 값으로 초기화되며 `[[Writable]]` , `[[Enumerable]]` , `[[Configurable]]` 값은 true로 초기화된다. 동적으로 추가해도 마찬가지다.

### 16.3.2 접근자 프로퍼티

접근자 프로퍼티는 자체적으로는 값을 갖지 않고 다른 데이터 프로퍼티의 **값을 읽거나 저장할 때 사용**하는 접근자 함수로 구성된 프로퍼티다.

getter/setter 함수라고도 부른다.

| 프로퍼티 어트리튜브 | 프로퍼티 디스크립터 객체의 프로퍼티 | 설명 |
| --- | --- | --- |
| [[Get]] | get | - 접근자 프로퍼티를 통해 데이터 프로퍼티의 값을 읽을 때 호출되는 접근자 함수 - 접근자 프로퍼티 키로 프로퍼티 값에 접근하면 프로퍼티 어트리뷰트의 값 getter 함수가 호출되고 그 결과가 값으로 반환됨. |
| [[Set]] | set | - 접근자 프로퍼티를 통해 데이터 프로퍼티의 값을 저장할 때 호출되는 접근자 함수 - 접근자 프로퍼티 키로 프로퍼티 값을 저장하면 프로퍼티 어트리뷰트의 값 setter 함수가 호출되고 그 결과가 값으로 저장됨. |
| [[Enumerable]] | enumerable | - 데이터 프로퍼티의 [[Enumerable]]과 같음. |
| [[Configurable]] | configurable | - 데이터 프로퍼티의 [[Configurable]]과 같음. |

```jsx
const person = {
  // 데이터 프로퍼티
  firstName: 'Ungmo',
  lastName: 'Lee',

  // 접근자 프로퍼티
  // getter 함수 (접근자 함수)
  get fullName() {
    return `${this.firstName} ${this.lastName}`;
  },

  // setter 함수 (접근자 함수)
  set fullName(name) {
    [this.firstName, this.lastName] = name.split(' ');
  },
};

console.log(person.firstName + ' ' + person.lastName); // Ungmo Lee

// fullName에 값을 저장하면 setter 함수가 호출
person.fullName = 'Heegun Lee';
console.log(person); // {firstName: "Heegun", lastName: "Lee"}

// fullName에 접근하면 getter 함수가 호출
console.log(person.fullName); // Heegun Lee

// firstName : 데이터 프로퍼티
let descriptor = Object.getOwnPropertyDescriptor(person, 'firstName');
console.log(descriptor);
// {value: "Heegun", writable: true, enumerable: true, configurable: true}

// fullName : 접근자 프로퍼티
descriptor = Object.getOwnPropertyDescriptor(person, 'fullName');
console.log(descriptor);
// {get: ƒ, set: ƒ, enumerable: true, configurable: true}
```

데이터 프로퍼티는 [[Value]], [[Writable]], [[Enumerable]], [[Configurable]] 프로퍼티 어트리뷰트를 갖고, 접근자 프로퍼티는 [[Get]], [[Set]], [[Enumerable]], [[Configurable]] 프로퍼티 어트리뷰트를 갖는다.

## 16.4 프로퍼티 정의

프로퍼티 정의란 새로운 프로퍼티를 추가하면서 프로퍼티 어트리뷰트를 명시적으로 정의하거나, 기존 프로퍼티의 프로퍼티 어트리뷰트를 재정의하는 것을 말한다.

**Object.defineProperty 메서드**

프로퍼티의 어트리뷰트를 정의할 수 있는 메서드. 인수로는 객체의 참조와 데이터 프로퍼티의 키인 문자열, 프로퍼티 디스크립터 객체를 전달한다. 한번에 하나의 프로퍼티만 정의할 수 있다.

```jsx
const person = {};

// 데이터 프로퍼티 정의
Object.defineProperty(person, 'firstName', {
  value: 'Ungmo',
  writable: true,
  enumerable: true,
  configurable: true,
});
```

**Object.defineProperties 메서드**

여러 개의 프로퍼티를 한 번에 정의할 수 있다.

```jsx
const person = {};

Object.defineProperties(person, {
  // 데이터 프로퍼티 정의
  firstName: {
    value: 'Ungmo',
    writable: true,
    enumerable: true,
    configurable: true,
  },
  lastName: {
    value: 'Lee',
    writable: true,
    enumerable: true,
    configurable: true,
  },
  // 접근자 프로퍼티 정의
  fullName: {
    // getter 함수
    get() {
      return `${this.firstName} ${this.lastName}`;
    },
    // setter 함수
    set(name) {
      [this.firstName, this.lastName] = name.split(' ');
    },
    enumerable: true,
    configurable: true,
  },
});

person.fullName = 'Heegun Lee';
console.log(person); // {firstName: "Heegun", lastName: "Lee"}
```

디스크립터 객체의 프로퍼티를 누락시키면 undefind, false가 기본값이다.

```jsx
const person = {};

// 데이터 프로퍼티 정의
Object.defineProperty(person, 'firstName', {
  value: 'Ungmo',
  writable: true,
  enumerable: true,
  configurable: true,
});

// 디스크립터 객체의 프로퍼티를 누락
Object.defineProperty(person, 'lastName', {
  value: 'Lee',
});

let descriptor = Object.getOwnPropertyDescriptor(person, 'firstName');
console.log('firstName', descriptor);
// firstName {value: "Ungmo", writable: true, enumerable: true, configurable: true}

descriptor = Object.getOwnPropertyDescriptor(person, 'lastName');
console.log('lastName', descriptor);
// lastName {value: "Lee", writable: false, enumerable: false, configurable: false}

// lastName 프로퍼티는 [[Enumerable]]의 값이 false이므로 열거되지 않는다.
console.log(Object.keys(person)); // ["firstName"]

// 이때 값을 변경하면 에러는 발생하지 않고 무시된다.
person.lastName = 'Kim';

// 이때 프로퍼티를 삭제하면 에러는 발생하지 않고 무시된다.
delete person.lastName;

// 해당 프로퍼티를 재정의할 경우 typeError 발생
// Object.defineProperty(person, 'lastName', { enumerable: true });
// Uncaught TypeError: Cannot redefine property: lastName

// 접근자 프로퍼티 정의
Object.defineProperty(person, 'fullName', {
  // getter 함수
  get() {
    return `${this.firstName} ${this.lastName}`;
  },
  // setter 함수
  set(name) {
    [this.firstName, this.lastName] = name.split(' ');
  },
  enumerable: true,
  configurable: true,
});

descriptor = Object.getOwnPropertyDescriptor(person, 'fullName');
console.log('fullName', descriptor);
// fullName {get: ƒ, set: ƒ, enumerable: true, configurable: true}

console.log(person); // {firstName: 'Ungmo', lastName: 'Lee'}
person.fullName = 'Heegun Lee';
console.log(person); // {firstName: "Heegun", lastName: "Lee"}
```

```jsx
const person = {
  name: 'Lee',
};

console.log(Object.getOwnPropertyDescriptor(person, 'name'));
// {value: "Lee", writable: true, enumerable: true, configurable: true}

// 프로퍼티 동적 생성
person.age = 20;

console.log(Object.getOwnPropertyDescriptors(person));
/*
{
  name: {value: "Lee", writable: true, enumerable: true, configurable: true},
  age: {value: 20, writable: true, enumerable: true, configurable: true}
}
*/

const person2 = {};

Object.defineProperty(person2, 'name', {
  value: 'Lee',
});

console.log(Object.getOwnPropertyDescriptors(person2));
// {value: "Lee", writable: false, enumerable: false, configurable: false}
```

Object.defineProperty 메서드로 프로퍼티를 정의할 때 프로퍼티 디스크립터 객체의 프로퍼티를 생략할 경우 기본값이 적용된다.

| 프로퍼티 디스크립터 객체의 프로퍼티 | 대응하는 프로퍼티 어트리뷰트 | 생략했을 때의 기본값 |
| ----------------------------------- | ---------------------------- | -------------------- |
| value                               | [[value]]                    | undefined            |
| get                                 | [[get]]                      | undefined            |
| set                                 | [[set]]                      | undefined            |
| writable                            | [[Writable]]                 | false                |
| enumerable                          | [[Enumerable]]               | false                |
| configurable                        | [[Configurable]]             | false                |

## 16.5 객체 변경 방지

객체는 변경 가능한 값이므로 재할당 없이 직접 변경할 수 있다. 프로퍼티를 추가하거나 삭제할 수 있고, 프로퍼티 값을 갱신할 수 있으며, 메서드를 사용하여 프로퍼티 어트리뷰트를 재정의할 수도 있다.

자바스크립트는 객체의 변경을 방지하는 다양한 메서드를 제공한다.

| 구분 | 메서드 | 프로퍼티 추가 | 프로퍼티 삭제 | 프로퍼티 값 읽기 | 프로퍼티 값 쓰기 | 프로퍼티 어트리뷰트 재정의 |
| --- | --- | --- | --- | --- | --- | --- |
| 객체 확장 금지 | Object.preventExtensions | X | O | O | O | O |
| 객체 밀봉 | Object.seal | X | X | O | O | X |
| 객체 동결 | Object.freeze | X | X | O | X | X |

### 16.5.1 객체 확장 금지 메서드 : Object.preventExtensions

Object.preventExtensions 메서드는 프로퍼티 추가가 금지된다. 프로퍼티는 프로퍼티 동적 추가와 Object.defineProperty 메서드로 추가할 수 있다. 이 두 가지 추가 방법이 모두 금지된다.

**Object.isExtensible 메서드**

확장이 가능한 객체인지 여부를 알 수 있다.

```jsx
const person = { name: 'Lee' };

console.log(Object.isExtensible(person)); // true

// person 객체의 확장을 금지하여 프로퍼티 추가를 금지
Object.preventExtensions(person);

console.log(Object.isExtensible(person)); // false

// 프로퍼티 추가가 금지됨.
person.age = 20; // 무시.
console.log(person); // {name: "Lee"}

// 프로퍼티 추가는 금지되지만 삭제는 가능
delete person.name;
console.log(person); // {}

// 프로퍼티 정의에 의한 프로퍼티 추가도 금지됨.
Object.defineProperty(person, 'age', { value: 20 });
// TypeError: Cannot define property age, object is not extensible
```

### 16.5.2 객체 밀봉 : Object.seal

Object.seal 메서드는 객체를 밀봉하여 프로퍼티 **추가**, **삭제**, **프로퍼티 어트리뷰트 재정의**를 금지한다. 즉, 읽기와 쓰기만 가능하다.

**Object.isSealed 메서드**

밀봉된 객체인지 여부를 알 수 있다.

```jsx
const person = { name: 'Lee' };

console.log(Object.isSealed(person)); // false

// person 객체를 밀봉(seal)하여 프로퍼티 추가, 삭제, 재정의를 금지
Object.seal(person);

console.log(Object.isSealed(person)); // true

// 밀봉(seal)된 객체는 configurable이 false다.
console.log(Object.getOwnPropertyDescriptors(person));
/*
{
  name: {value: "Lee", writable: true, enumerable: true, configurable: false},
}
*/

// 프로퍼티 추가가 금지됨.
person.age = 20; // 무시.
console.log(person); // {name: "Lee"}

// 프로퍼티 삭제가 금지됨.
delete person.name; // 무시.
console.log(person); // {name: "Lee"}

// 프로퍼티 값 갱신은 가능
person.name = 'Kim';
console.log(person); // {name: "Kim"}

// 프로퍼티 어트리뷰트 재정의가 금지됨.
Object.defineProperty(person, 'name', { configurable: true });
// TypeError: Cannot redefine property: name
```

### 16.5.3 객체 동결 : Object.freeze

Object.freeze 메서드는 객체를 동결하여 프로퍼티 **추가**, **삭제**, **프로퍼티 어트리뷰트 재정의 금지**, **프로퍼티 값 갱신을** 금지한다. 즉, 읽기만 가능하다.

**Object.isFrozen 메서드**

동결된 객체인지 여부를 알 수 있다.

```jsx
const person = { name: 'Lee' };

console.log(Object.isFrozen(person)); // false

// person 객체를 동결(freeze)하여 프로퍼티 추가, 삭제, 재정의, 쓰기를 금지함.
Object.freeze(person);

console.log(Object.isFrozen(person)); // true

// 동결(freeze)된 객체는 writable과 configurable이 false다.
console.log(Object.getOwnPropertyDescriptors(person));
/*
{
  name: {value: "Lee", writable: false, enumerable: true, configurable: false},
}
*/

// 프로퍼티 추가가 금지됨.
person.age = 20; // 무시.
console.log(person); // {name: "Lee"}

// 프로퍼티 삭제가 금지됨.
delete person.name; // 무시.
console.log(person); // {name: "Lee"}

// 프로퍼티 값 갱신이 금지됨.
person.name = 'Kim'; // 무시.
console.log(person); // {name: "Lee"}

// 프로퍼티 어트리뷰트 재정의가 금지됨.
Object.defineProperty(person, 'name', { configurable: true });
// TypeError: Cannot redefine property: name
```

### 16.5.4 불변 객체

변경 방지 메서드는 얕은 변경 방지로 직속 프로퍼티만 변경이 방지되고 중첩 객체까지는 영향을 주지 못한다.

```jsx
const person = {
  name: 'Lee',
  address: { city: 'Seoul' },
};

// 얕은 객체 동결
Object.freeze(person);

// 직속 프로퍼티만 동결함.
console.log(Object.isFrozen(person)); // true
// 중첩 객체까지 동결하지 못함.
console.log(Object.isFrozen(person.address)); // false

person.address.city = 'Busan';
console.log(person); // {name: "Lee", address: {city: "Busan"}}
```

객체의 중첩 객체까지 동결하여 변경이 불가능한 읽기 전용의 불변 객체를 구현하려면 객체를 값으로 갖는 모든 프로퍼티에 대해 재귀적으로 Object.freeze 메서드를 호출해야 한다.
