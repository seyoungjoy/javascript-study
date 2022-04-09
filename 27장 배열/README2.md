## 27.9 배열 고차 함수

**고차 함수** : 함수를 인수로 전달받거나 함수를 반환하는 함수

- 외부 산태의 변경이나 가변 데이터를 피하고 불변성을 지향하는 함수형 프로그래밍에 기반을 두고 있다.

<br/>

**함수형 프로그래밍**

- 조건문과 반복문을 제거
- 변수의 사용을 억제
- 순수 함수를 통해 부수 효과를 최대한 억제

<br/>

### 27.9.1 Array.prototype.sort

- `sort` 메서드는 배열의 요소를 오름차순 정렬한다.
- 원본 배열을 직접 변경한다.
- 내림차순으로 요소를 정렬하려면 `sort` 메서드를 사용하여 오름차순으로 정렬한 후 `reverse` 메서드를 사용하여 요소의 순서를 뒤집는다.

<br/>

```jsx
const fruits = ['Banana', 'Orange', 'Apple'];

// 오름차순(ascending) 정렬
fruits.sort();

// sort 메서드는 원본 배열을 직접 변경
console.log(fruits); // ['Apple', 'Banana', 'Orange']

// 내림차순(descending) 정렬
fruits.reverse();

// reverse 메서드도 원본 배열을 직접 변경
console.log(fruits); // ['Orange', 'Banana', 'Apple']
```

<br/>

- `sort` 메서드의 기본 정렬 순서는 유니코드 코드 포인트의 순서를 따른다.
  - 배열의 요소가 숫자 타입이라 할지라도 배열의 요소를 일시적으로 문자열로 변환한 후 유니코드 코드 포인트의 순서를 기준으로 정렬한다.

```jsx
// ‘1’ → U+0031
// ‘2’ → U+0032
// ‘10’ → U+0031U+0030
// 따라서 문자열 ‘2’의 유니코드 코드 포인트보다 ‘10’의 유니코드 코드 포인트보다 앞에 있기 때문에 sort 메서드로 정렬하면 [10, 2] 가 된다.

['2', '1'].sort(); // -> ["1", "2"]
[2, 1].sort(); // -> [1, 2]

['2', '10'].sort(); // -> ["10", "2"]
[2, 10].sort(); // -> [10, 2]
```

<br/>

- 숫자 요소를 정렬할 때는 `sort` 메서드에 정렬 순서를 정의하는 비교 함수를 인수로 전달해야 한다.

```jsx
const points = [40, 100, 1, 5, 2, 25, 10];

// 숫자 배열의 오름차순 정렬. 비교 함수의 반환값이 0보다 작으면 a를 우선하여 정렬한다.
points.sort((a, b) => a - b);
console.log(points); // [1, 2, 5, 10, 25, 40, 100]

// 숫자 배열의 내림차순 정렬. 비교 함수의 반환값이 0보다 작으면 b를 우선하여 정렬한다.
points.sort((a, b) => b - a);
console.log(points); // [100, 40, 25, 10, 5, 2, 1]
```

<br/>

### 27.9.2 Array.prototype.forEach

- `forEach` 메서드는 for 문을 대체할 수 있는 고차함수다.
- `forEach` 메서드는 반복문을 추상화한 고차 함수로서 내부에서 반복문을 통해 자신을 호출한 배열을 순회하면서 수행해야 할 처리를 콜백 함수로 전달받아 반복 호출한다.
- 원본 배열을 변경하지 않는다.

<br/>

```jsx
// for 문으로 배열 순회
const numbers = [1, 2, 3];
let pows = [];

for (let i = 0; i < numbers.length; i++) {
  pows.push(numbers[i] ** 2);
}
console.log(pows); // [1, 4, 9]

// forEach 메서드는 numbers 배열의 모든 요소를 순회하면서 콜백 함수를 반복 호출
const numbers = [1, 2, 3];
let pows = [];

numbers.forEach((item) => pows.push(item ** 2));
console.log(pows); // [1, 4, 9]
```

<br/>

- `forEach` 메서드는 콜백 함수를 호출할 때 3개의 인수를 순차적으로 전달한다.
  - **item** : `forEach` 메서드를 호출한 배열의 요소값
  - **index** : 인덱스
  - **arr** : `forEach` 메서드를 호출한 배열 자체 `this`

```jsx
[1, 2, 3].forEach((item, index, arr) => {
  console.log(
    `요소값: ${item}, 인덱스: ${index}, this: ${JSON.stringify(arr)}`,
  );
});
/*
요소값: 1, 인덱스: 0, this: [1,2,3]
요소값: 2, 인덱스: 1, this: [1,2,3]
요소값: 3, 인덱스: 2, this: [1,2,3]
*/
```

<br/>

### 27.9.3 Array.prototype.map

- `map` 메서드는 자신을 호출한 배열의 모든 요소를 순회하면서 인수로 전달받은 콜백 함수를 반복 호출한다.
- 콜백 함수의 반환값들로 구성된 새로운 배열을 반환한다.
- 원본 배열은 변경되지 않는다.
- `map` 메서드를 호출한 배열과 `map` 메서드가 생성하여 반환한 배열은 1:1 매핑한다.

```jsx
const numbers = [1, 4, 9];

const roots = numbers.map((item) => Math.sqrt(item));

// map 메서드는 새로운 배열을 반환한다.
console.log(roots); // [1, 2, 3]

// map 메서드는 원본 배열을 변경하지 않는다.
console.log(numbers); // [1, 4, 9]
```

<br/>

### 27.9.4 Array.prototype.filter

- `filter` 메서드는 자신을 호출한 배열의 모든 요소를 순회하면서 인수로 전달받은 콜백 함수를 반복 호출한다.
- 콜백 함수의 반환값이 true인 요소로만 구성된 새로운 배열을 반환한다.
- 원본 배열은 변경되지 않는다.

```jsx
const numbers = [1, 2, 3, 4, 5];

// numbers 배열에서 홀수인 요소만을 필터링한다(1은 true로 평가된다).
const odds = numbers.filter((item) => item % 2);
console.log(odds); // [1, 3, 5]
```

| forEach                          | map                                                               | filter                                                                            |
| -------------------------------- | ----------------------------------------------------------------- | --------------------------------------------------------------------------------- |
| undefined 을 반환                | 콜백 함수의 반환값들로 구성된 새로운 배열을 반환                  | 콜백 함수의 반환값이 true인 요소만 추출한 새로운 배열을 반환                      |
| 반복문을 대체하기 위한 고차 함수 | 요소값을 다른 값으로 매핑한 새로운 배열을 생성하기 위한 고차 함수 | 필터링 조건을 만족하는 특정 요소만 추출하여 새로운 배열을 생성하기 위한 고차 함수 |

```jsx
// filter 메서드는 콜백 함수를 호출하면서 3개(요소값, 인덱스, this)의 인수를 전달한다.
[1, 2, 3].filter((item, index, arr) => {
  console.log(
    `요소값: ${item}, 인덱스: ${index}, this: ${JSON.stringify(arr)}`,
  );
  return item % 2;
});
/*
요소값: 1, 인덱스: 0, this: [1,2,3]
요소값: 2, 인덱스: 1, this: [1,2,3]
요소값: 3, 인덱스: 2, this: [1,2,3]
*/
```

<br/>

- `filter` 메서드는 자신을 호출한 배열에서 특정 요소를 제거하기 위해 사용할 수도 있다.

```jsx
class Users {
  constructor() {
    this.users = [
      { id: 1, name: 'Lee' },
      { id: 2, name: 'Kim' },
    ];
  }

  // 요소 추출
  findById(id) {
    // id가 일치하는 사용자만 반환한다.
    return this.users.filter((user) => user.id === id);
  }

  // 요소 제거
  remove(id) {
    // id가 일치하지 않는 사용자를 제거한다.
    this.users = this.users.filter((user) => user.id !== id);
  }
}

const users = new Users();

let user = users.findById(1);
console.log(user); // [{ id: 1, name: 'Lee' }]

// id가 1인 사용자를 제거한다.
users.remove(1);

user = users.findById(1);
console.log(user); // []
```

<br/>

- `filter` 메서드를 사용해 특정 요소를 제거할 경우 특정 요소가 중복되어 있다면 중복된 요소가 모두 제거된다.
- 특정 요소를 하나만 제거하려면 `indexOf` 메서드를 통해 특정 요소의 인덱스를 취득한 다음 `splice` 메서드를 사용한다.

<br/>

### 27.9.5 Array.prototype.reduce

- `reduce` 메서드는 자신을 호출한 배열의 모든 요소를 순회하며 인수로 전달받은 콜백 함수를 반복 호출한다.
- 콜백 함수의 반환값을 다음 순회 시에 콜백 함수의 첫 번째 인수로 전달하면서 콜백 함수를 호출하여 **하나의** **결과값을 만들어 반환**한다.
- 원본 배열은 변경되지 않는다.
- `reduce` 메서드의 콜백 함수는 4개의 인수를 전달한다.
  - **accumulator** : callback 함수의 반환값을 누적
  - **currentValue** : 배열의 현재 요소
  - **index** : 배열의 현재 요소의 인덱스
  - **array** : 호출한 배열 (this)

```jsx
// 1부터 4까지 누적을 구한다.
// reduce 메서드의 콜백 함수는 4개의 인수를 전달받아 배열의 length만큼 총 4회 호출된다.
const sum = [1, 2, 3, 4].reduce(
  (accumulator, currentValue, index, array) => accumulator + currentValue,
  0,
);

console.log(sum); // 10
```

<p align="center"><img src="./img/27_3.png" width="50%"></p>

<br/>

- `reduce` 메서드는 자신을 호출한 배열의 모든 요소를 순회하며 하나의 결과값을 구해야 하는 경우에 사용한다.

<br/>

1. **평균 구하기**

```jsx
const values = [1, 2, 3, 4, 5, 6];

const average = values.reduce((acc, cur, i, { length }) => {
  // 마지막 순회가 아니면 누적값을 반환하고 마지막 순회면 누적값으로 평균을 구해 반환
  return i === length - 1 ? (acc + cur) / length : acc + cur;
}, 0);

console.log(average); // 3.5
```

<br/>

2. **최대값 구하기**

```jsx
const values = [1, 2, 3, 4, 5];

const max = values.reduce((acc, cur) => (acc > cur ? acc : cur), 0);

console.log(max); // 5
```

```jsx
// Math.max 메서드를 사용하는 방법이 더 직관적이다.
const values = [1, 2, 3, 4, 5];

const max = Math.max(...values);

console.log(max); // 5
```

<br/>

3. **요소의 중복 횟수 구하기**

```jsx
const fruits = ['banana', 'apple', 'orange', 'apple'];

const count = fruits.reduce((acc, cur) => {
  acc[cur] = (acc[cur] || 0) + 1;
  return acc;
}, {});

console.log(count); // {banana: 1, apple: 2, orange: 1}
```

<br/>

4. **중첩 배열 평탄화**

```jsx
const values = [1, [2, 3], 4, [5, 6]];

const flatten = values.reduce((arr, cur) => arr.concat(cur), []);
// [1] => [1, 2, 3] => [1, 2, 3, 4] => [1, 2, 3, 4, 5, 6]

console.log(flatten); // [1, 2, 3, 4 ,5, 6]
```

```jsx
// ES10에서 도입된 Array.prototype.flat 메서드를 사용하는 방법이 더 직관적이다.
[1, [2, 3, 4, 5]].flat(); // [1, 2, 3, 4, 5]

// 인수 2는 중첩 배열을 평탄화하기 위한 깊이 값이다.
[1, [2, 3, [4, 5]]].flat(2); // [1, 2, 3, 4, 5]
```

<br/>

5. **중복 요소 제거**

```jsx
const values = [1, 2, 1, 3, 5, 4, 5, 3, 4, 4];

const result = values.reduce((acc, cur, i, arr) => {
  // 순회 중인 요소의 인덱스가 자신의 인덱스라면 처음 순회하는 요소다.
  // 이 요소만 초기값으로 전달받은 배열에 담아 반환한다.
  // 순회 중인 요소의 인덱스가 자신의 인덱스가 아니라면 중복된 요소다.
  if (arr.indexOf(cur) === i) acc.push(cur);
  return acc;
}, []);

console.log(result); // [1, 2, 3, 5, 4]
```

```jsx
// filter 메서드를 사용하는 방법이 더 직관적이다.
const values = [1, 2, 1, 3, 5, 4, 5, 3, 4, 4];

const result = values.filter((v, i, arr) => arr.indexOf(v) === i);

console.log(result); // [1, 2, 3, 5, 4]
```

```jsx
// 중복되지 않는 유일한 값들의 집합인 Set을 사용할 수도 있다.
const values = [1, 2, 1, 3, 5, 4, 5, 3, 4, 4];

const result = [...new Set(values)];

console.log(result); // [1, 2, 3, 5, 4]
```

<br/>

- `reduce` 메서드를 호출할 때는 두 번째 인수로 초기값을 전달하는 것이 안전하다.

```jsx
// 초기값 생략할 경우 TypeError
const sum = [].reduce((acc, cur) => acc + cur);
// TypeError: Reduce of empty array with no initial value

const sum = [].reduce((acc, cur) => acc + cur, 0);
console.log(sum); // 0
```

<br/>

### 27.9.6 Array.prototype.some

- `some` 메서드는 자신을 호출한 배열의 요소를 순회하면서 인수로 전달된 콜백 함수를 호출한다.
- 반환값이 단 한번이라도 참이면 true, 모두 거짓이면 false를 반환한다.

```jsx
// 배열의 요소 중에 10보다 큰 요소가 1개 이상 존재하는지 확인
[5, 10, 15].some((item) => item > 10); // -> true

// 배열의 요소 중에 0보다 작은 요소가 1개 이상 존재하는지 확인
[5, 10, 15].some((item) => item < 0); // -> false

// 배열의 요소 중에 'banana'가 1개 이상 존재하는지 확인
['apple', 'banana', 'mango'].some((item) => item === 'banana'); // -> true

// some 메서드를 호출한 배열이 빈 배열인 경우 언제나 false를 반환한다.
[].some((item) => item > 3); // -> false
```

<br/>

### 27.9.7 Array.prototype.every

- `every` 메서드는 자신을 호출한 배열의 요소를 순회하면서 인수로 전달된 콜백 함수를 호출한다.
- 반환값이 모두 참이면 true, 단 한 번이라도 거짓이면 false를 반환한다.

```jsx
// 배열의 모든 요소가 3보다 큰지 확인
[5, 10, 15].every((item) => item > 3); // -> true

// 배열의 모든 요소가 10보다 큰지 확인
[5, 10, 15].every((item) => item > 10); // -> false

// every 메서드를 호출한 배열이 빈 배열인 경우 언제나 true를 반환한다.
[].every((item) => item > 3); // -> true
```

<br/>

### 27.9.8 Array.prototype.find

- `find` 메서드는 자신을 호출한 배열의 요소를 순회하면서 인수로 전달된 콜백 함수를 호출하여 반환값이 true인 첫 번째 요소를 반환한다.

```jsx
const users = [
  { id: 1, name: 'Lee' },
  { id: 2, name: 'Kim' },
  { id: 3, name: 'Choi' },
  { id: 4, name: 'Park' },
];

users.find((user) => user.id === 2);
// {id: 2, name: 'Kim'}
```

<br/>

- `filter` 메서드의 반환값은 언제나 배열이지만 `find` 메서드는 콜백 함수의 반환값이 true인 첫 번째 요소를 반환하므로 `find` 메서드의 결과값은 배열이 아닌 해당 요소값이다.

```jsx
// filter는 배열을 반환한다.
[1, 2, 2, 3].filter((item) => item === 2); // -> [2, 2]

// find는 요소를 반환한다.
[1, 2, 2, 3].find((item) => item === 2); // -> 2
```

<br/>

### 27.9.9 Array.prototype.findIndex

- `findIndex` 메서드는 자신을 호출한 배열의 요소를 순회하면서 인수로 전달된 콜백 함수를 호출하여 반환값이 true인 첫 번째 요소의 인덱스를 반환한다.
- 반환값이 true인 요소가 존재하지 않는다면 -1을 반환한다.

```jsx
const users = [
  { id: 1, name: 'Lee' },
  { id: 2, name: 'Kim' },
  { id: 2, name: 'Choi' },
  { id: 3, name: 'Park' },
];

// id가 2인 요소의 인덱스를 구한다.
users.findIndex((user) => user.id === 2); // -> 1

// name이 'Park'인 요소의 인덱스를 구한다.
users.findIndex((user) => user.name === 'Park'); // -> 3

// 위와 같이 프로퍼티 키와 프로퍼티 값으로 요소의 인덱스를 구하는 경우
// 다음과 같이 콜백 함수를 추상화할 수 있다.
function predicate(key, value) {
  // key와 value를 기억하는 클로저를 반환
  return (item) => item[key] === value;
}

// id가 2인 요소의 인덱스를 구한다.
users.findIndex(predicate('id', 2)); // -> 1

// name이 'Park'인 요소의 인덱스를 구한다.
users.findIndex(predicate('name', 'Park')); // -> 3
```

<br/>

### 27.9.10 Array.prototype.flatMap

- `flatMap` 메서드는 `map` 메서드를 통해 생성된 새로운 배열을 평탄화한다.
- 즉, `map` 메서드와 `flat` 메서드를 순차적으로 실행하는 효과가 있다.
  - `flat` 메서드 : 모든 하위 배열 요소를 지정한 깊이까지 재귀적으로 이어붙인 새로운 배열을 생성
- `flatMap` 메서드는 평탄화 깊이를 지정할 수는 없고 1단계만 평탄화한다. 중첩 배열의 평탄화 깊이를 지정해야 하면 `map` 메서드와 `flat` 메서드를 각각 호출한다.

```jsx
const arr = ['hello', 'world'];

// map과 flat을 순차적으로 실행
arr.map((x) => x.split('')).flat();
// -> ['h', 'e', 'l', 'l', 'o', 'w', 'o', 'r', 'l', 'd']

// flatMap은 map을 통해 생성된 새로운 배열을 평탄화한다.
arr.flatMap((x) => x.split(''));
// -> ['h', 'e', 'l', 'l', 'o', 'w', 'o', 'r', 'l', 'd']
```
