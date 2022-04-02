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
    - 장점 : 인덱스로 요소에 빠르게 접근할 수 있다.
    - 단점 : 요소를 삽입 or 삭제하는 경우 효율적이지 않다.(배열의 요소를 연속적으로 유지하기 위해 요소를 이동시켜야 하기 때문)

<p align="center"><img src="./img/jsdeep-27-2.jpg" width="50%"></p>

- 자바스크립트에서의 배열
    - 일반적인 배열의 동작을 흉내 낸 특수한 개체
    - 요소에는 여러가지 데이터 타입이 들어갈 수 있다.(메모리 공간이 동일한 크기를 갖지 않아도 된다.)
    - 배열의 요소가 연속적으로 이어져 있지 않아도 된다.(희소 배열)
    - 단점 : 해시 테이블로 구현된 객체이므로 인덱스로 요소에 접근하는 경우 일반적인 배열보다 성능적인 면에서 느리다.
    - 장점 : 요소를 삽입 또는 삭제하는 경우에는 일반적인 배열보다 빠른 성능을 기대할 수 있다.

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
> ### 해시테이블(HashTable)이란?
> (Key, Value)로 데이터를 저장하는 자료구조
> <img src="./img/js-deep-27-1.png" width="70%">

## 27.3 length 프로퍼티와 희소 배열
### length 프로퍼티
- length 프로퍼티의 값은 0과 2<sup>32</sup> - 1(4,294,967,296 - 1) 미만의 양의 정수
- length 프로퍼티의 값은 배열에 요소를 추가,삭제시 자동 갱신된다.
```javascript
[].length // -> 0
[1,2,3].length // -> 3
```
### 희소 배열
- 배열의 요소가 연속적으로 위치하지 않고 일부가 비어 있는 배열
- 자바스크립는 문법적으로 이를 허용.
- length와 배열 요소의 개수가 일치하지 않는다.
- 희소배열은 사용하지 않는 것이 좋음.
- **배열을 생성할 경우에는 같은 타입의 요소를 연속적으로 위치시키는 것이 성능적으로 좋다.**

```jsx
const sparse = [ , 2, , 4]

console.log(sparse) //[empty, 2, empty, 4];
console.log(sparse.length) //4
console.log(Object.getOwnPropertyDescriptors(sparse));
/*
    1: {value: 2, writable: true, enumerable: true, configurable: true}
    3: {value: 4, writable: true, enumerable: true, configurable: true}
    length: {value: 4, writable: true, enumerable: false, configurable: false}
*/
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
//인수가 1개일 때
const arr = new Array(10);

console.log(arr); //(10) [empty × 10]
console.log(arr.length); //10

//인수가 2개이상 일때
const arrr = new Array(1,2,3);

console.log(arrr) //[1,2,3]
```

### 27.4.3 Array.of
- ES6
```jsx

const arr = Array.of(1);
console.log(arr); //[1]

const arrr = Array.of(1,2,3);
console.log(arrr); //[1,2,3];
```

### 27.4.4 Array.from
- ES6
- 유사 배열 객체, 이터러블 객체를 인수로 전달받아 배열로 변환하여 반환
```jsx
Array.from({length:2, 0:'a', 1:'b'});//[a,b]

Array.from('Hello'); //['H', 'e', 'l', 'l', 'o']
```

## 27.5 배열 요소의 참조
- 대괄호 표기법으로 참조
```jsx
const arr = [1,2];

console.log(arr[0]); //1
console.log(arr[1]); //2
```

## 27.6 배열 요소의 추가와 갱신
- 배열의 요소를 동적으로 추가할 수 있다. 이때 length 프로퍼티 값은 자동 갱신된다.
```jsx
const arr = [0];

arr[1] = 1;
console.log(arr);//[0,1]
console.log(arr.length); //2
```

- 인덱스는 요소의 위치를 나타내기 때문에 반드시 0 이상의 정수를 사용해야 한다. 정수 이외의 값은 프로퍼티가 생성된다.
```jsx
const arr= [];

//배열 요소의 추가
arr[0] = 1;
arr[1] = 2;

//프로퍼티 추가
arr['foo'] = 3;
arr.bar = 4;
arr[1.1] = 5;
arr[-1] = 6;

console.log(arr); //
/*
0: 1
1: 2
1.1: 5
-1: 6
bar: 4
foo: 3
length: 2
*/
```

## 27.7 배열 요소의 삭제
- delete연산자로 삭제하면 희소 배열이 생성되기 때문에
- `Array.prototype.splice` 메서드 사용을 추천한다.

## 27.8 배열 메서드
- 배열 객체의 프로토타입인 Array.prototype은 프로토타입 메서드를 제공한다.
- 결과물을 반환할 때 두가지 패턴을 가진다.
    - 원본 배열을 직접 변경하는 메서드
    - 원본 배열을 직접 변경하지 않고 새로운 배열을 생성하여 반환하는 메서드
- 원본 배열을 직접 변경하는 메서드는 상태를 직접 변경하는 부수 효과를 가지기 때문에 주의해야 한다.
- **가급적 원본 배열을 직접 변경하지 않는 메서드를 사용하는 편이 좋다.**


### 27.8.1 `Array.isArray`
- 전달된 인수가 배열이면 true, 배열이 아니면 false
```jsx
console.log(Array.isArray([])); //true
console.log(Array.isArray([1,2])); //true
console.log(Array.isArray({})); //false
```

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
- push 메서드 사용을 지양하는 이유
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
- 스프레드 문법으로 대체 가능.
```jsx
const arr1 = [1,2];
const arr2 = [3,4];

let result = arr1.concat(arr2); 
console.log(result); //[1,2,3,4]

//스프레드 문법
let result2 = [...arr1, ...arr2]
console.log(result2);//[1,2,3,4]
```

> 📌 결론적으로 push/unshift, concat 메서드 대신 ES6의 스프레드 문법을 일관성 있게 사용하는 권장한다.


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
//원본 배열 arr의 모든 요소를 문자열로 변환 후 기본 구분자로 연결한 문자열 반환
console.log(arr.join()); //1,2,3

//빈 문자열로 연결
//원본 배열 arr의 모든 요소를 문자열로 변환 후, 빈 문자열로 연결한 문자열을 반환
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