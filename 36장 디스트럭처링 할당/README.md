# 36장 디스트럭처링 할당(구조 분해 할당)
- destructure : ~구조를 해체하다.
- 구조화된 배열과 같은 이터러블 or 객체를 destructuring(비구조화)하여 1개 이상의 변수에 개별적으로 할당하는 것
- 배열과 같은 이터러블 또는 객체 리터럴에 필요한 값만 추출하여 변수에 할당 할 때 유용

## 36.1 배열 디스트럭처링 할당
- ES5에서 구조화된 배열을 디스트럭처링하여 1개 이상의 변수에 할당하는 방법은 아래와 같다.
```jsx
//ES5
var arr = [1,2,3];

var one = arr[0];
var two = arr[1];
var three = arr[2];

console.log(one, two, three);
```
- ES6의 배열 디스트럭처링 할당은 배열의 각 요소를 배열로부터 추출하여 1개이 상의 변수에 할당한다.
- 이때 배열 디스트럭처링 할당의 대상은 이터러블이어야 하며, 할당 기준은 배열의 인덱스다. 즉 순서대로 할당된다.
```jsx
const arr = [1,2,3];

const [one, two, three] = arr;

console.log(one, two, three); //1 2 3
```
- 배열 디스트럭처링 할당을 위한 변수에 기본값을 설정할 수 있다.
```jsx
// 기본값
const [a,b,c = 3] = [1,2];
console.log(a,b,c); //1 2 3

// 기본값보다 할당된 값이 우선한다.
const[e,f=10,g=3] = [1,2];
console.log(e,f,g) //1 2 3
```
- 배열과 같은 이터러블에서 필요한 요소만 추출하여 변수에 할당하고 싶을 때 유용하다.
```jsx
function parseURL(url = ''){
    const parsedURL = url.match(/^(\w+):\/\/([^/]+)\/(.*)$/);
    console.log(parsedURL);
    if(!parsedURL) return{};

    const [, protocol, host, path] = parsedURL;
    return {protocol, host, path};
}

const parsedURL = parseURL('https://section.blog.naver.com/BlogHome');
console.log(parsedURL)
//{protocol: 'https', host: 'section.blog.naver.com', path: 'BlogHome'}
```

## 36.2 객체 디스트럭처링 할당
- ES5에서 객체의 각 프로퍼티를 객체로부터 디스트럭처링하여 변수에 할당하기 위해서는 프로퍼티 키를 사용해야 한다.
```jsx
//ES5
var user = {firstName : 'Ungmo', lastName:'Lee'};

var firstName = user.firstName;
var lastName = user.lastName;

console.log(firstName, lastName); //Ungmo Lee
```
- 객체 디스트럭처링 할당의 대상은 객체이어야 하며, 할당 기준은 프로퍼티 키다.
- 즉 순서는 의미가 없으며 선언된 변수 이름과 프로퍼티 키가 일치하면 할당된다.
```jsx

const user = {firstName:'Ungmo', lastName:'Lee'};

const {lastName, firstName} = user;

console.log(firstName,lastName); //Ungmo Lee
```
- 할당 연산자 왼쪽에 프로퍼티 값을 할당받을 변수를 선언하고 변수는 객체 리터럴 형태로 선언한다.
```jsx
const {lastName, firstName} = {firstName:'Ungmo', lastName:'Lee'};
```
- 프로퍼티 축약 표현을 통해 선언할 수 있다.
```jsx
const {lastName, firstName} = user;

const {lastName:lastName, firstName : firstName} = user;
```
- 객체 디스트럭처링 할당을 위한 변수에 기본값을 설정할 수 있다.
```jsx

const {firstName = 'Ungmo', lastName} = {lastNAme:'Lee'}
console.log(firstName, lastName);

const {firstName:fn='Ungmo', lastName:In} = {lastName:'Lee'};
console.log(fn, In);
```

