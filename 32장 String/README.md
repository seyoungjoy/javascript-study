# 32장 String
## 32.1 String 생성자 함수
- 표준 빌트인 객체인 String 객체는 생성자 함수 객체.
- 따라서 new 연산자와 함께 호출하여 String 인스턴스를 생성할 수 있다.
<br>
- String 래퍼 객체는 배열과 마찬가지로 length 프로퍼티와 인덱스를 나타내는 숫자 형식의 문자열을 프로퍼티 키로, 각 문자를 프로퍼티 값으로 갖는 유사 배열 객체이면서 이터러블이다.

```jsx
const strObj = new String('Lee');

console.log(strObj)
/*
0: "L"
1: "e"
2: "e"
length: 3
[[Prototype]]: String
[[PrimitiveValue]]: "Lee"
*/
```
- 문자열은 원시 값이므로 변경할 수 없다.
```jsx
strObj[0] = 'S';
console.log(strObj); //Lee
```


## 32.3 length 프로퍼티
- length 프로퍼티는 문자열의 문자 개수를 반환한다.
```jsx
'Hello'.length; //5
```

## 32.3 String 메서드
- Array의 메서드는 원본배열을 직접 변경하는 메서드, 원본 배열을 직접 변경하지 않고 새로운 배열을 생성하여 반환하는 메서드가 있었다.
- 반면 String의 메서드들은 원시값 변경이 불가능하기 때문에 언제나 새로운 문자열을 반환한다.
- String 래퍼 객체는 읽기 전용 객체로 제공된다.
```jsx
const strObj = new String('Lee');
console.log(Object.getOwnPropertyDescriptors(strObj));
/*
{
    0: {value: 'L', writable: false, enumerable: true, configurable: false}
    1: {value: 'e', writable: false, enumerable: true, configurable: false}
    2: {value: 'e', writable: false, enumerable: true, configurable: false}
    length: {value: 3, writable: false, enumerable: false, configurable: false}
    [[Prototype]]: Object
}
*/
```

### 32.2.1 String.prototype.indexOf
- 메서드를 호출한 문자열에서 인수로 전달받은 문자열을 검색하여 첫 번째 인덱스를 반환한다.
- 검색 실패시 `-1`을 반환.
```jsx
const str = 'Hello World';

str.indexOf('l'); //2

str.indexOf('k'); //-1
```


### 32.2.2 String.prototype.search
- 대상 문자열에서 인수로 전달받은 정규 표현식과 매치하는 문자열을 검색하여 일치하는 문자열의 인덱스를 반환.
```jsx
const str = 'Hello World';

str.search(/o/); //4
str.search(/x/); //-1
```

### 32.2.3 String.prototype.includes
- ES6
- 인수로 전달받은 문자열이 포함되어 있는지 확인하여 그 결과를 true or false로 반환.
```jsx
const str = 'Hello world';

str.includes('Hello'); //true
str.includes('x') //false
```

### 32.2.4 String.prototype.startsWith
- ES6
- 인수로 전달받은 문자열로 시작하는지 확인하여 그 결과를 true/false로 반환.
```jsx
const str = 'Hello world';

str.startsWith('Hello'); //true
str.startsWith('x') //false
```


### 32.2.5 String.prototype.EndsWith
- 인수로 전달받은 문자열로 끝나는지 확인하여 그 결과를 true/false로 반환
```jsx
const str = 'Hello world';

str.endsWith('world'); //true
str.endsWith('x') //false
```
l
### 32.2.6 String.prototype.charAt
- 대상 문자열에서 인수로 전달받은 인덱스에 위치한 문자를 검색하여 반환.
```jsx

const str = 'Hello';

for(let i =0; i<str.length; i++){
    console.log(str.charAt(i))
}
//H e l l o
```

### 32.2.7 String.prototype.substring
- 대상문자열에서 첫 번째 인수로 전달받은 인덱스에 위치하는 문자부터 두 번째 인수로 전달받은 인덱스에 위치하는 문자의 바로 이전 문자까지의 부분 문자열을 반환.
- 두번째 인수를 생략하면 첫번째 인수의 인덱스부터 마지막까지 반환
```jsx

const str = 'Hello World';

str.substring(1,4); //ell

//두번째 인수를 생략하면 첫번째 인수의 인덱스부터 마지막까지 반환
str.substring(1); //ello World
```

### 32.2.8 String.prototype.slice
- substring 메서드와 동일하게 동작하나 음수인 인수를 전달할 수 있다.
- 음수인 인수를 전달하면 대상 문자열의 가장 뒤에서부터 시작하여 문자열을 잘라내어 반환한다.
```jsx
const str = 'Hello World';

str.substring(0,5); //hello
str.slice(0,5); //hello

//인수가 0보다 작으면 -으로 취급.
str.substring(-5); //hello world

//뒤에서 5자리를 잘라서 반환한다.
str.slice(-5); //world
```

### 32.2.9 String.prototype.toUpperCase
- 대상 문자열을 모두 대문자로 변경한 문자열을 반환
```jsx
const str = 'Hello World';
str.toUpperCase(); // HELLO WORLD
```

### 32.2.10 String.prototype.toLowerCase
- 대상 문자열을 모두 소문자로 변경한 문자열을 반환
```jsx
const str = 'Hello World';
str.toLowerCase(); // hello world
```

### 32.2.11 String.prototype.trim
- 대상 문자열 앞 뒤에 공백 문자가 있을 경우 이를 제거.
```jsx
const str = '    foo  ';

str.trim(); //foo
```

### 32.2.12 String.prototype.repeat
- ES6
- 대상문자열을 인수로 전달받은 정수만큼 반복해 새로운 문자열을 반환.
```jsx
const str = 'abc';

str.repeat(2); //abcabc
```

### 32.2.13 String.prototype.replace
- 첫번째 인수로 전달받은 **문자열 or 정규표현식**을 검색하여
- 두번째 인수로 전달한 문자열로 치환해 반환.
```jsx

const str = 'Hello World';

str.replace('world', 'Lee'); //Hello Lee

str.replace(/hello/gi, 'Lee'); //Lee Lee
```

### 32.2.14 String.prototype.split
- 첫번째 인수로 전달한 문자열 또는 정규표현식을 검색하여 문자열을 구분한 후
- 분리된 각 문자열로 이루어진 배열을 반환
- 인수로 빈 문자열을 전달하면 각 문자를 모두 분리하고
- 인수를 생략하면 대상 문자열 전체를 단일 요소로 하는 배열을 반환.
```jsx

const str = 'How are you doing?';

console.log(str.split(' ')); //['How', 'are', 'you', 'doing?']

console.log(str.split(/\s/)); //['How', 'are', 'you', 'doing?']

console.log(str.split('')); 
//['H', 'o', 'w', ' ', 'a', 'r', 'e', ' ', 'y', 'o', 'u', ' ', 'd', 'o', 'i', 'n', 'g', '?']

console.log(str.split()); //['How are you doing?']
```