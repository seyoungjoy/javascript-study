# 27장 배열
## 27.1 배열이란?
- 여러 개의 값을 순차적으로 나열한 자료 구조
### 요소(Element)
- 배열이 가지고 있는 값
- 원시값, 객체, 함수, 배열 등 자바스크립트에서 값으로 인정하는 모든 것
### 인덱스(Index)
- 배열에서 자신의 위치를 나타내는 0 이상의 정수
### length 프로퍼티
- 요소의 개수, 배열의 길이를 나타낸다.
```javascript
const arr = ['apple', 'banana', 'orange'];

arr[0] // -> 'apple'
arr[1] // -> 'banana'
arr[2] // -> 'orange'

console.log(arr.length); //3
```
### 배열은 객체 타입이다.
- 배열 리터럴, Array 생성자 함수, Array of, Array from 메서드로 생성할 수 있다.
- 배열의 생성자 함수는 Array
- 배열의 프로토타입 객체는 `Array.prototype`
```javascript
console.log(Array.prototype);

at: ƒ at()
concat: ƒ concat()
constructor: ƒ Array()
copyWithin: ƒ copyWithin()
entries: ƒ entries()
every: ƒ every()
fill: ƒ fill()
filter: ƒ filter()
find: ƒ find()
findIndex: ƒ findIndex()
findLast: ƒ findLast()
findLastIndex: ƒ findLastIndex()
flat: ƒ flat()
flatMap: ƒ flatMap()
forEach: ƒ forEach()
includes: ƒ includes()
indexOf: ƒ indexOf()
join: ƒ join()
keys: ƒ keys()
lastIndexOf: ƒ lastIndexOf()
length: 0
map: ƒ map()
pop: ƒ pop()
push: ƒ push()
reduce: ƒ reduce()
reduceRight: ƒ reduceRight()
reverse: ƒ reverse()
shift: ƒ shift()
slice: ƒ slice()
some: ƒ some()
sort: ƒ sort()
splice: ƒ splice()
toLocaleString: ƒ toLocaleString()
toString: ƒ toString()
unshift: ƒ unshift()
values: ƒ values()
Symbol(Symbol.iterator): ƒ values()
Symbol(Symbol.unscopables): {copyWithin: true, entries: true, fill: true, find: true, findIndex: true, …}
[[Prototype]]: Object
```
- 배열은 객체지만 일반 객체와는 구별되는 독특한 특징이 있다.   

배열은 값의 순서(index)와 length 프로퍼티를 갖기 때문에 순차적으로 요소에 접근하거나 원하는 요소에 접근이 가능하다.

|구분|객체|배열|
|:---:|:---:|:---:|
|구조|프로퍼티 키와 프로퍼티 값|인덱스와 요소|
|값의 참조|프로퍼티 키|인덱스|
|값의 순서|X|O|
|length 프로퍼티|X|O|

## 27.2 자바스크립트 배열은 배열이 아니다.
- 자료구조(Data Structure)의 배열
    - 밀집 배열 : 하나의 데이터 타입으로 통일되어 있고 서로 연속적으로 인접.

<p align="center"><img src="./img/jsdeep-27-2.jpg" width="70%"></p>

- 자바스크립트에서의 배열
    - 일반적인 배열의 동작을 흉내 낸 특수한 개체
    - 요소에는 여러가지 데이터 타입이 들어갈 수 있다.(메모리 공간이 동일한 크기를 갖지 않아도 된다.)
    - 연속적으로 이어져 있지 않아도 된다.

```javascript
const arr = [
    'string',
    10,
    true,
    null,
    undefined,
    NaN,
    Infinity,
    [ ],
    {},
    function(){}
]
```
```javascript
console.log(Object.getOwnPropertyDescriptors([1,2,3]));
/*
{
    0: {value: 1, writable: true, enumerable: true, configurable: true}
    1: {value: 2, writable: true, enumerable: true, configurable: true}
    2: {value: 3, writable: true, enumerable: true, configurable: true}
    length: {value: 3, writable: true, enumerable: false, configurable: false}
    [[Prototype]]: Object
}
*/
//인덱스를 나타내는 문자열을 프로퍼티 키로 가지며
//length 프로퍼티를 갖는다.
//배열의 요소는 사실 프로퍼티 값이다.
```
## 27.3 length 프로퍼티와 희소 배열
### length 프로퍼티
- length 프로퍼티의 값은 0과 2<sup>32</sup> - 1(4,294,967,296 - 1) 미만의 양의 정수
- length 프로퍼티의 값은 배열에 요소를 추가,삭제시 자동 갱신된다.
```javascript
[].length // -> 0
[1,2,3].length // -> 3
```
## 27.4 배열 생성
### 27.4.1 배열 리터럴
- 가장 일반적이고 간편한 배열 생성 방식
- 배열 리터럴은 객체 리터럴과 달리 프로퍼티 키가 없고 값만 존재한다.
```javascript
const arr = [1,2,3];
console.log(arr.length); //3

//요소가 0개일 때는 length 프로퍼티 값은 0
const arr = [];
console.log(arr.length); //0

//요소를 생략하면 희소 배열 생성
const arr = [1, ,3];
console.log(arr.length);//3
console.log(arr); //[1, empty, 3]
console.log(arr[1]); undefined
```
### 27.4.2 Array 생성자 함수
```javascript
const arr = new Array(10);

console.log(arr);
console.log(arr.length);
```

## 27.8 배열 메서드
### 27.8.1 `Array.isArray`
- 전달된 인수가 배열이면 true, 배열이 아니면 false

### 27.8.2 `Array.prototype.indexOf`
- 원본 배열에서 인수로 전달된 요소를 검색하여 인덱스를 반환
    - 인수로 전달된 요소가 여러 개면 첫 번째로 검색된 요소의 인덱스를 반환
    - 인수로 전달된 요소가 없으면 `-1`을 반환
- 배열에 특정 요소가 존재하는지 확인활 때 유용
- (ES7) Array.prototype.includes 메서드가 가독성이 더 좋다.
```javascript
const arr = [1,2,2,3];
console.log(arr.indexOf(2)); 1

console.log(arr.indexOf(2,2)); 2

//배열에 특정 요소가 존재하는지 확인할 때 유용하다.
const foods = ['apple', 'banana', 'orange'];

if(foods.indexOf('orange')===-1){
    console.log('오렌지 X')
} else{
    console.log('오렌지 O')
}

//Array.prototype.includes
const fooods = ['apple', 'banana', 'orange'];

if(!foods.includes('orange')){
    console.log('오렌지 X')
} else{
    console.log('오렌지 O')
}
```

### 27.8.3 `Array.prototype.push`
- 인수로 전달받은 모든 값을 원본 배열의 마지막 요소로 추가
- 변경된 length 프로퍼티 값 반환
- 원본 배열 직접 변경
```jsx
const arr = [1,2];

let result = arr.push(3,4);
console.log(result); //4

console.log(arr); //[1,2,3,4]
```
- push 메서드를 지양하는 이유
    1. 성능 면에서 좋지 않다. 추가할 요소가 하나라면 length 프로퍼티로 직접 추가하는게 더 빠르다.
    2. 배열을 직접 변경하는 부수 효과가 있기 때문에 스프레드 문법을 사용하는 편이 좋다.
```jsx
//length 프로퍼티로 배열 추가
const arr = [1,2];

arr[arr.length] = 3;

console.log(arr);//[1,2,3]

//스프레드 문법으로 추가
const arr = [1,2];

const newArr = [...arrr, 3];
console.log(newArr); //[1,2,3]
```

### 27.8.4 `Array.prototype.pop`
- 마지막 요소를 제거하고 제거한 요소를 반환
- 원본 배열이 빈 배열이면 undefined 반환
- 원본 배열 직접 변경
```jsx
const arr = [1,2];

let result = arr.pop();
console.log(result); //2
console.log(arr); //[1]
```

### 27.8.5 `Array.prototype.unshift`
- 인수로 전달받은 모든 값을 원본 배열의 선두에 요소로 추가
- 변경된 length 프로퍼티 값 반환
- 원본 배열 직접 변경
- 부수 효과가 있기 때문에 스프레드 문법을 추천
```jsx
const arr = [1,2];

let result = arr.unshift(0);
console.log(result); //3
console.log(arr); //[0,1,2]

//스프레드 문법으로 선두에 요소 추가
const arr = [1,2];

const newArr = [0, ...arr];
console.log(newArr); //[0,1,2]
```

### 27.8.6 `Array.prototype.shift`
- 원본 배열에서 첫 번째 요소를 제거하고 제거한 요소를 반환
- 원본 배열이 빈 배열이면 undefined 반환
- 원본 배열 직접 변경
```jsx
const arr = [1,2];

const result = arr.shift();
console.log(result); //1
console.log(arr); //[2]
```

### 27.8.7 `Array.prototype.concat`
- 인수로 전달된 값들을 원본 배열의 마지막 요소로 추가한 새로운 배열을 반환.
- 원본 배열 변경X, 새로운 배열 반환
```jsx
const arr1 = [1,2];
const arr2 = [3,4];

let result = arr1.concat(arr2); 
console.log(result); //[1,2,3,4]

//스프레드 문법
let result2 = [...arr1, ...arr2]
console.log(result2);//[1,2,3,4]
```

### 27.8.8 `Array.prototype.splice`
- 원본 배열의 중간에 요소를 추가하거나 중간에 있는 요소를 제거할 때 사용.
- 원본 배열 직접 변경
```jsx
//제거 후 새로운 요소 삽입
const arr = [1,2,3,4];
const result = arr.splice(1,2,20,30)

console.log(arr); //[1,20,30,4]


//새로운 요소 삽입
const arr = [1,2,3,4];

const result = arr.splice(1,0,100);
console.log(arr); //[1,100,2,3,4]

//요소 제거
const arr = [1,2,3,4];

const result = arr.splice(1,2);
console.log(arr); //[1,4]

```

### 27.8.9 `Array.prototype.slice`
- 인수로 전달된 범위의 요소들을 복사하여 배열로 반환.
- 원본 배열 변경X
```jsx
const arr= [1,2,3];

console.log(arr.slice(0,1)); //[1]
console.log(arr.slice(1,2)); //[2]
```

### 27.8.10 `Array.prototype.join`
- 원본 배열의 모든 요소를 문자열로 변환 후, 인수로 전달받은 문자열(구분자)로 연결한 문자열을 반환.
- 구분자는 생략가능
- 기본 구분자는 콤마(,)
```jsx
const arr= [1,2,3];

//기본 구분자 콤마
console.log(arr.join()); //1,2,3

//빈 문자열로 연결
console.log(arr.join(''));//123

//:로 연결
console.log(arr.join(':'));//1:2:3
```

### 27.8.11 `Array.prototype.reverse`
- 원본 배열의 순서를 반대로 뒤집는다
- 원본 배열 변경
```jsx
const arr= [1,2,3];

const result = arr.reverse();
console.log(arr);//[3,2,1]
console.log(result);//[3,2,1]
```

### 27.8.12 `Array.prototype.fill`
- ES6
- 인수로 전달받은 값을 배열의 처음부터 끝까지 요소로 채운다.
- 원본 배열 변경
```jsx
const arr=[1,2,3];

arr.fill(0);

console.log(arr);//[0,0,0];
```


### 27.8.13 `Array.prototype.includes`
- ES7
- 배열 내에 특정 요소가 포함되어 있으면 true, 없으면 false를 반환
```jsx
const arr=[1,2,3];

console.log(arr.includes(3));//true
console.log(arr.includes(4));//false
```

### 27.8.14 `Array.prototype.flat`
- ES10
- 인수로 전달한 깊이만큼 재귀적으로 배열을 평탄화한다.
```jsx
const result = [1,[2,3,4],5].flat();

console.log(result);//[1,2,3,4,5]
```