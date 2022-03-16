## 10.1 객체란?
---
- 원시 값을 제외한 나머지 값(함수, 배열, 정규 표현식 등)은 모두 객체
- 원시 타입의 값은 변경 불가능한 값이지만,<br>
  객체 타입의 값, 즉 객체는 변경 가능한 값이다.
- 객체는 프로퍼티와 메서드로 구성된 집합체  

![Untitled](https://user-images.githubusercontent.com/85421850/158594129-98144483-dec6-464e-9e13-5ac08695c353.png)

#### 프로퍼티

- 키와 값으로 구성
- 객체의 상태를 나타내는 값(data)

#### 메서드

- 프로퍼티 값이 함수일 경우 메서드라고 부른다.
- 자바스크립트 함수는 일급 객체이므로 값으로 취급될 수 있다.
- 프로퍼티(상태 데이터)를 참조하고 조작할 수 있는 동작(behavior)
<br><br>

## 10.2 객체 리터럴에 의한 객체 생성
---
- 자바스크립트는 프로토타입 기반 객체지향 언어로서 다양한 객체 생성 방법을 지원
    - 객체 리터럴 : 객체를 생성하기 위한 표기법
    - Object 생성자 함수
    - 생성자 함수
    - Object.create 메서드
    - 클래스(ES6)

```javascript
//- 객체 리터럴 생성 방법
//중괄호({...}) 내에 0개 이상의 프로퍼티를 정의

var person = {
    name:'seyoung',
    sayHello: function(){
        console.log(`hello my name is ${this.name}!`);
    }
};

console.log(typeof person); //object
console.log(person.name); //seyoung
person.sayHello(); //hello my name is seyoung!
```

**주의할 점 !**
- 객체 리터럴의 중괄호 뒤에는 세미콜론을 붙인다.
- 객체 리터럴의 중괄호는 코드 블록을 의미하는 것이 아니라 값으로 평가되는 표현식이기 때문
<br><br>

## 10.3 프로퍼티
---
- 객체는 프로퍼티의 집합이며, 프로퍼티는 키와 값으로 구성된다.
- **프로퍼티 키** : 빈 문자열을 포함하는 모든 문자열 또는 심벌값
- **프로퍼티 값** : 자바스크립트에서 사용할 수 있는 모든 값

```jsx
var person = {
    //키 : 값
	name:'Lee',
	age:20
}
```

#### 프로퍼티 키
- 프로퍼티 값에 접근할 수 있는 이름으로서 **식별자 역할**
- 심벌값보다는 일반적으로 문자열을 사용.
- 문자열이기 때문에 (”...”)로 묶어야 한다.
- 식별자 네이밍 규칙을 준수하는 경우 따옴표 생략 가능
- 식별자 네이밍 규칙을 따르지 않는 이름에는 반드시 따옴표 사용
```jsx
var person = {
    //식별자 네이밍 규칙 준수시 따옴표 생략 가능
    firstName:'Ung-mo',
    //식별자 네이밍 규칙 미준수시 따옴표 생략 불가능
    'last-name':'Lee'
}
console.log(person) //{firstName: 'Ung-mo', last-name: 'Lee'}
```
<br>

- **프로퍼티를 키를 동적**으로 생성하기
- 키로 사용할 표현식을 대괄호[...]로 묶어야 한다.

```jsx
var obj = {};
var key = 'hello';

obj[key] = 'world';
console.log(obj); //{hello: 'world'}
```
<br>

- 문자열, 심벌 값 외의 값을 사용하면 암묵적 타입변환을 통해 문자열이 된다.
- 숫자 리터럴 사용시 문자열로 변환된다.
```jsx
var foo={
    0:1,
    1:2,
    2:3
}
console.log(foo); //{0: 1, 1: 2, 2: 3}
```
<br>

- var, function 과 같은 예약어는 에러가 발생할 여지가 있으므로 키로 권장하지 않는다.
- 이미 존재하는 프로퍼티 키를 중복 선언하면 나중에 선언한 프로퍼티가 덮어쓴다.
- 에러가 나지 않는다는 점을 주의.
```jsx
var foo = {
    name:'Lee',
    name:'Kim'
}
console.log(foo); //{name: 'Kim'}
```

## 10.4 메서드
---
- 프로퍼티 값이 함수일 경우 일반 함수와 구분하기 위해 메서드라고 부른다.
- 12장 함수에서 살펴보자

## 10.5 프로퍼티 접근
---
#### 마침표 표기법 : 마침표 프로퍼티 접근 연산자`(.)`를 사용
- 자바스크립트에서 사용 가능한 유효한 이름
#### 대괄호 표기법 : 대괄호 프로퍼티 접근 연산자`([...])`를 사용
- 자바스크립트에서 사용 가능한 유효한 이름
- 유효하지 않은 이름
- 반드시 따옴표로 감싼 문자열을 대괄호로 묶어줘야 한다.
- 따옴표로 감싸지 않은 이름을 프로퍼티 키로 사용하면 식별자로 해석된다.

```jsx
var person = {
    name:'Lee'
};
//마침표 표기법에 의한 프로퍼티 접근
console.log(person.name); //Lee

//대괄호 표기법에 의한 프로퍼티 접근
console.log(person['name']); //Lee

//대괄호 내부를 따옴표로 감싸지 않으면 식별자로 해석.
console.log(person[name]); //undefined

var person = {
    'last-name' : 'Lee',
    1:10
};

person.'last-name'; //SyntaxError: Unexpected string
person.last-name; //NaN

person[last-name];//ReferenceError: last is not defined
person['last-name']; //Lee

//프로퍼티 키가 숫자로 이뤄진 경우 따옴표를 생략할 수 있다.
person.1; //SyntaxError: Unexpected number
person.'1'; //SyntaxError: Unexpected string
person[1]; //10
person['1']; //10
```

## 10.6 프로퍼티 값 갱신
---
- 이미 존재하는 프로퍼티 값을 할당하면 프로퍼티 값이 갱신된다.

```jsx
var person = {
	name:'Lee'
};

person.name = 'Kim';

console.log(person); //{name:"Kim"}
```

## 10.7 프로퍼티 동적 생성

- 존재하지 않는 프로퍼티에 값을 할당하면 프로퍼티가 동적으로 생성되어 추가되고 프로퍼티 값이 할당된다.

```jsx
var person = {
    name:'Lee'
}

person.age=20;
console.log(person) //{name: 'Lee', age: 20}
```

## 10.8 프로퍼티 삭제

- delete 연산자로 개체의 프로퍼티를 삭제할 수 있다.

```jsx
var person = {
    name:'Lee'
}

person.age=20;

delete person.name;
console.log(person) //{age: 20}
```

## 10.9 ES6에서 추가된 객체 리터럴의 확장 기능
---
### 10.9.1 프로퍼티 축약 표현

- 변수 이름과 프로퍼티 키가 동일한 이름일 때 프로퍼티 키를 생략할 수 있다.
- 프로퍼티 키는 변수 이름으로 자동 생성된다.

```jsx
let x = 1;
let y = 2;

var obj = {
    x : x,
    y : y,
}
const obj = {x, y};

console.log(obj.x);//1
console.log(obj.y);//2
console.log(obj);//{x: 1, y: 2}
```

### 10.9.2 계산된 프로퍼티 이름

- 표현식을 사용해 프로퍼티 키를 동적으로 생성할 수 있다.

```jsx
var prefix = 'prop';
var i =0;

var obj = {};

obj[prefix+'-'+ ++i] = i;
obj[prefix+'-'+ ++i] = i;
obj[prefix+'-'+ ++i] = i;

obj[`${prefix}-${++i}`] = i;
obj[`${prefix}-${++i}`] = i;
obj[`${prefix}-${++i}`] = i;

console.log(obj); //{prop-1: 1, prop-2: 2, prop-3: 3}
```

### 10.9.3 메서드 축약 표현

- 메서드를 정의할 때 function 키워드를 생략한 축약 표현을 사용할 수 있다.

```jsx
var obj = {
    sayHello : function(){
        console.log('hi');
    }
}

const object = {
    sayHello(){
        console.log('hi');
    }
}
```