# **12.1 함수란?**

### 함수는 자바스크립트에서 가장 중요한 핵심 개념

후에 나올 다른 핵심 개념인 스코프, 실행 컨텍스트, 클로저, 생성자 함수, 메서드, this, 프로토타입, 모듈화 등이 모두 함수와 깊은 관련이 있다.

- **함수는 일련의 과정을 문으로 구현하고 코드 블록으로 감싸서 하나의 실행 단위로 정의한 것이다.**
- 함수는 입력을 받아서 출력을 내보낸다. 이때 입력으로 전달 받는 변수를 **매개 변수(parameter)**, 입력을 **인수(argument)**, 출력을 **반환값(return value)** 이라 한다.
- 함수는 값이며, 특정 함수를 구별하기 위해 식별자인 함수 이름을 사용할 수 있다.
- 함수는 정의를 통해 생성하고 호출을 통해 실행한다.

```jsx
// 함수 선언문을 이용한 함수 정의
function add(x, y) {
	return x + y;
}
// 함수 호출
console.log(add(2, 3)); // 반환값 5
```

<br/><br/>

# 12.2 함수를 사용하는 이유

함수는 필요할 때 여러 번 호출할 수 있다. 실행 시점을 개발자가 결정할 수 있고, 몇 번이든 재사용이 가능하다.

따라서 함수는 **코드의 재사용** 이라는 측면에서 매우 유용하다.

### 함수가 없다면?

만약 함수가 없다면 같은 로직을 요하는 결과가 여러번 나올때마다 똑같은 코드를 계속 작성해 줘야한다.

```jsx
// q1. 1 + 2 =?
x = 1;
y = 2;
result = x + y;

// q2. 2 + 3 =?
x = 2;
y = 3;
result = x + y; 

// q3. 4 + 5 =?
x = 4;
y = 5;
result = 4 + 5;

...

// 함수를 이용할 경우 한번만 작성 후 인수를 넣어 호출만 해주면 된다.
function add(x, y) {
	return x + y;
}
// q1. 1 + 2 =?
result = add(1, 2);
// q2. 2 + 3 =?
result = add(2, 3);
// q3. 4 + 5 =?
result = add(4, 5);
```

- 함수가 없다면 로직이 수정될 때마다 로직을 사용한 코드를 일일히 다 찾아서 바꿔줘야 한다.
- 함수가 없다면 사람은 실수하기 마련이므로 서로 로직이 달라질 수 있다.
- 함수를 사용함으로써 코드의 중복을 억제하고 재사용성을 높여 **유지 보수의 편의성**을 높이고 실수를 줄여 **코드의 신뢰성**을 높이는 효과가 있다.
- 함수에는 이름(식별자)을 붙일 수 있다. 함수의 역할을 잘 표현하는 이름을 짓는다면 내부 코드를 이해하지 않고도 함수의 역할을 파악할 수 있게 돕는다. 이는 **코드의 가독성**을 향상시킨다.


<br/><br/>
# 12.3 함수 리터럴

함수는 객체 타입의 값이다. 앞서 [5장 표현식과 문](https://www.notion.so/05-c3f96cab1d114cd1abaa84d049042feb) 에서 **리터럴은 사람이 이해할 수 있는 문자 또는 약속된 기호를 사용해 값을 생성하는 표기법을 말한다.** 이라고 했다.

따라서 함수는 변수에 할당할 수 있다.

```jsx
var f = function add(x, y) {
	return x + y;
};
```

함수 리터럴의 구성 요소는 다음과 같다.

함수 리터럴은 평가되어 값을 생성하며 이 값은 객체다. 즉, **함수는 객체다.**

함수는 객체지만 일반 객체와는 다르게 **호출할 수 있다.** 그리고 함수 객체만의 고유한 프로퍼티를 갖는다.

<br/><br/>
# 12.4 함수 정의

함수를 정의하는 방법에는 4가지가 있다.

## 12.4.1 함수 선언문

```jsx
// 함수 선언문
function add(x, y) {
	return x + y;
}
// 함수 참조
console.dir(add);

// 함수 호출
console.log(add(2, 5));
```

함수 선언문은 함수 리터럴과 형태가 동일하다. 단, 함수 리터럴은 함수 이름을 생략할 수 있으나 **함수 선언문은 함수 이름을 생략할 수 없다.**

**함수 선언문은 표현식이 아닌 문이다.** 

만약 함수선언문이 표현식이라면 크롬 개발자도구 콘솔에서 함수 선언문을 실행했을 때 `undefined` 가 아니라 생성된 함수가 출력되어야 한다.

<p align='center'><img src='https://bl3302files.storage.live.com/y4mYOgGt58hPkqfa0MWJcefgvUJa-fX44VgiCTA2sV3u6g4MF6qwNUTmza06gR4fJRwL_b3stb_66v6ckVeTD2s5iY6nPFlWVO8QbkjmuZR-csBmsvJ122sNQPz0GPoS55Z3ZVtPNxyE7D8jpXkWApezoAkvywVkI38ikJ99FBeAHe9VEhBtDyzKPUCVd9rtITC?width=318&height=114&cropmode=none' width='250px'></p>

완료값 undefined 출력

[5.6절](https://www.notion.so/05-c3f96cab1d114cd1abaa84d049042feb)에서 표현식이 아닌 문은 변수에 할당할 수 없다고 배웠다. 그러나 다음 코드는 함수 선언문이 변수에 할당되는 것처럼 보인다.

```jsx
var add = function add(x, y) {
	return x + y;
};

console.log(add(2, 5)); // 7
```

이렇게 동작하는 이유는 자바스크립트 엔진이 코드의 문맥에 따라 **함수 리터럴을 함수 선언문으로 해석하는 경우와 함수 선언문을 함수 리터럴 표현식으로 해석하는 경우**가 있기 때문이다.

예를 들어, `{ }` 은 블록문일 수도 있고 객체 리터럴일 수도 있다. `{ }` 처럼 중의적인 코드는 문맥에 따라 해석이 달라진다.

자바스크립트 엔진은 `{ }` 이 단독으로 존재하면 블록문으로 해석하고 값으로 평가되야 할 문맥에서 피연산자등으로 사용되면 객체 리터럴로 해석한다.

```jsx
function foo() { console.log('foo'); }
foo(); // foo

(function bar() { console.log('bar'); });
bar(); // ReferencError
```

위 예제에서 단독으로 사용된 `foo` 는 함수 선언문으로 해석되고, 그룹 연산자 `( )` 내에 있는 함수 선언문은 함수 리터럴 표현식으로 해석된다. 그룹 연산자의 피연산자는 값으로 평가될 수 있는 표현식이어야 하기 때문이다.

둘 다 함수 객체를 생성한다는 점에서는 동일하지만 호출에서 차이가 있다.

`foo` 는 호출이 가능하지만 `bar` 는 가능하지 않다.

12.3절에서 **함수 이름은 함수 몸체 내에서만 참조할 수 있는 식별자다.** 라고 했다. 이는 함수 몸체 외부에서는 함수 이름으로 함수를 참조할 수 없으므로 외부에서는 호출할 수 없다는 뜻이다.
<p align='center'><img src='https://bl3302files.storage.live.com/y4mfoWAIakK9gPz6Fp_H0ZJ2BOxDL_-wg7nAI_NIsHTdOXimpymNtw1o9dk5R2UVOfzqn7cFLUZSXUZyBNoGqq3y2ozv5uVjIHTXlbTxHZqNhcNGEP0BKeqEQOOd4_AXBrAJCcYyR95A_U0Z2njKqypcluW3duARC3f8PB2aPfXr8ZHwpKh9PiBSYS2l21NmjDy?width=758&height=592&cropmode=none' width='250px'></p>



하지만 `foo` 도 함수 내부에서만 유효한 식별자이므로 `foo` 도 호출할 수 없어야한다. `foo` 라는 이름으로 호출하려면 `foo` 는 함수 이름이 아니라 함수 객체를 가리키는 식별자여야 한다. 하지만 위 예제에서는 `foo` 를 선언한적도, 할당한적도 없다.

결론부터 말하자면 `foo` 는 자바스크립트 엔진이 **암묵적으로** 생성한 식별자이다.

**자바스크립트 엔진은 생성된 함수를 호출하기 위해 함수 이름과 동일한 이름의 식별자를 암묵적으로 생성하고, 거기에 함수 객체를 할당한다.**


<p align='center'><img src='https://bl3302files.storage.live.com/y4mKJW-2i_37E5O0rJO2Byt_GmKkyLN6wyq7Q85cbDDYKHDTe5PsUTtXAWgKU0wIlzjFpH4CSwHDCVTubBd7qIr0biMytxuGlgZmu2hZhidXiDBEahqFY8eMemnyZZWs6byqMIcDph97BTAHD1NA7VZLACvf9GNT_wpJOkEje9SfXo1z2Nc4scctg8aJo8Ps2vy?width=758&height=592&cropmode=none' width='250px'></p>

**함수는 함수 이름으로 호출하는 것이 아니라 함수 객체를 가리키는 식별자로 호출한다.**

즉, 함수 선언문으로 생성한 함수를 호출하는 것은 함수 이름 add가 아니라 자바스크립트 엔진이 암묵적으로 생성한 식별자 add인 것이다.
<br/><br/>
## 12.4.2 함수 표현식

자바스크립트의 함수는 객체이면서 값처럼 변수에 할당할 수 있고, 프로퍼티 값이 될 수도 있으며, 배열의 요소가 될 수도 있다. 이러한 값의 성질을 갖는 객체를 **일급 객체** 라고 한다.

함수는 일급 객체이므로 함수 리터럴로 생성한 함수 객체를 변수에 할당할 수 있다. 이러한 함수 정의 방식을 함수 표현식이라 한다.

```jsx
// 익명 함수 표현식
var add = function(x, y) {
	return x + y;
};
console.log(add(2, 5)); // 7

// 기명 함수 표현식
var add = function foo(x, y) {
	return x + y;
};

console.log(foo(2, 5)); // ?
```

**함수는 함수 이름이 아니라 식별자를 통해 호출한다.**

<br/>

## 12.4.3 함수 생성 시점과 함수 호이스팅

```jsx
console.dir(add); // f add(x, y)
console.dir(sub); // undefined

console.log(add(2, 5)); // 7
console.log(sub(2, 5)); // TypeError: sub is not a function

// 함수 선언문
function add(x, y) {
	return x + y;
}

// 함수 표현식
var sub = function (x, y) {
	return x - y;
};
```

위 예제와 같이 함수 선언문으로 정의한 함수는 함수 선언문 이전에 호출할 수 있다. 그러나 함수 표현식은 이전에 호출할  수 없다. 이는 두 함수의 생성 시점이 다르기 때문이다.

모든 선언문은 런타임 이전에 자바스크립트 엔진에 의해 먼저 실행된다. 함수 선언문은 런타임 이전에 함수 객체가 먼저 생성된다.

그에 반해 함수 표현식은 식별자만 먼저 선언되고 할당은 런타임 이후에 되므로 참조할 수 없다.

이처럼 **함수 선언문이 코드의 선두로 끌어 올려진 것처럼 동작하는 자바스크립트 고유의 특징을 함수 호이스팅** 이라 한다.

함수 호이스팅과 변수 호이스팅은 미묘한 차이가 있다.

- 변수 호이스팅: `var` 키워드를 사용한 변수 선언문은 `undefined` 로 초기화된다.
- 함수 호이스팅: 함수 선언문을 통해 암묵적으로 생성된 식별자는 함수 객체로 초기화 된다.

따라서 **함수 표현식으로 함수를 정의하면 함수 호이스팅이 아니라 변수 호이스팅이 발생한다.**

> 함수 호이스팅은 호출하기전에 선언해야 한다는 당연한 규칙을 무시한다. 이 같은 문제 때문에 JSON을 창안한 더글라스 크락포드는 함수 표현식을 사용할 것을 권장한다.
> 

<br/>

## 12.4.4 Function 생성자 함수

자바스크립트가 기본 제공하는 빌트인 함수다.

`Function` 생성자 함수에 매개변수 목록과 함수 몸체를 문자열로 전달하면서 `new` 연산자와 함께 호출하면 함수 객체를 생성하여 반환한다. (`new` 연산자 없이 호출해도 결과는 동일하다.)

```jsx
var add = new Function('x', 'y', 'return x + y');
```

`Function` 생성자 함수로 함수를 생성하는 방식은 일반적이지 않으며 바람직하지도 않다.

`Function` 생성자 함수로 생성한 함수는 클로저를 생성하지 않는 등. 함수 선언문이나 표현식으로 생성한 함수와 다르게 동작한다.

<br/>

## 12.4.5 화살표 함수

ES6에서 도입된 화살표 함수는 `function` 키워드 대신 화살표 `=>` 를 사용해 좀 더 간략한 방법으로 선언할 수 있다.

화살표 함수는 항상 익명 함수로 정의한다.

```jsx
const add = (x, y) => x + y;
```

화살표 함수는 표현만 간략한 것이 아니라 내부 동작 또한 간략화되어 있다.

- `this` 바인딩이 다르다.
- `prototype` 프로퍼티가 없다.
- `arguments` 객체를 생성하지 않는다.

<br/><br/>

# 12.5 함수 호출

- 함수는 함수를 가리키는 식별자와 한 쌍의 소괄호인 함수 호출 연산자로 호출한다.
- 함수를 호출하면 현재의 실행 흐름을 중단하고 호출된 함수로 실행 흐름을 옮긴다.

<br/>

## 12.5.1 매개변수와 인수

함수를 실행하기 위해 필요한 값을 함수 외부에서 내부로 전달할 필요가 있는 경우, 매개변수를 통해 인수를 전달한다.

- 인수는 값으로 평가될 수 있는 표현식이어야 한다.
- 인수는 함수를 호출할 때 지정하며, 개수와 타입에 제한이 없다.

매개변수는 함수를 정의할 때 선언하고, 함수 내부에서 변수와 동일하게 취급된다. 

- 함수가 호출되면 함수 몸체 내에서 암묵적으로 매개변수가 생성되고 일반 변수처럼 `undefined` 로 초기화된 이후 인수가 할당된다.

매개변수는 함수 몸체 내부에서만 참조할 수 있고 외부에서는 참조할 수 없다.

```jsx
function add(x, y) {
	console.log(x, y); // 2 5
	return x + y;
}
add(2, 5);

console.log(x, y); // ReferenceError
```

함수는 매개변수의 개수와 인수의 개수가 일치하는지 체크하지 않는다.

- 인수가 부족하거나 더 많아도 에러가 발생하지 않는다.
- 인수가 부족해서 할당되지 않은 매개변수 값은 `undefined` 이다.
- 인수가 더 많을 경우 초과된 인수는 무시된다.
    - 모든 인수는 `arguments` 객체의 프로퍼티로 보관되기 때문에 초과된 인수도 참조할 수는 있다.

<br/>

## 12.5.2 인수 확인

```jsx
function addNumber(x, y) {
	return x + y;
}
console.log(addNumber(2)); // NaN
console.log(addNumber('a', 'b')); // 'ab'
```

위 함수는 숫자를 더한다는 함수의 역할을 정확히 수행하지 못한다. 이유는 다음과 같다.

- 자바스크립트는 매개변수와 인수의 개수가 일치하는지 확인하지 않는다.
- 자바스크립트는 매개변수의 타입을 사전에 지정할 수 없다.

따라서 함수를 정의할 때 적절한 인수가 전달되었는지 확인할 필요가 있다.

1. 타입 확인
    
    ```jsx
    function add(x, y) {
    	if (typeof x !== 'number' || typeof y !== 'number') {
    		throw new TypeError('인수는 모두 숫자 값이어야 합니다.');
    	}
    	return x + y;
    }
    ```
    
2. 단축 평가를 이용한 기본값 할당
    
    ```jsx
    function add(a, b, c) {
    	a = a || 0;
    	b = b || 0;
    	c = c || 0;
    	return a + b + c;
    }
    ```
    
3. ES6에서 도입된 매개변수 기본값 할당
    
    ```jsx
    function add(a = 0, b = 0, c = 0) {
    	return a + b + c;
    }
    ```
    

+ `arguments` 객체를 통한 인수의 개수도 체크할 수 있다.

<br/>

## 12.5.3 매개변수의 최대 개수

ECMAScript 사양에서는 매개변수의 최대 개수에 대해 명시적으로 제한하고 있지 않다. 함수의 매개변수는 코드를 이해하는 데 방해되는 요소이므로 **이상적인 매개변수 개수는 0개이며 적을수록 좋다. 이상적인 함수는 한 가지 일만 해야 하며 가급적 작게 만들어야 한다.**

따라서 매개변수는 최대 3개 이상을 넘지 않는 것을 권장한다. 만약 그 이상의 매개변수가 필요하다면  하나의 매개변수를 선언하고 객체를 인수로 전달하는 것이 유리하다.

객체를 인수로 전달하면 매개변수의 순서를 신경쓰지 않아도 되고, 가독성이 좋아지며 실수도 줄어드는 효과가 있다.

하지만 주의할 점은 함수 외부에서 내부로 전달한 객체가 내부에서 바뀔 경우 외부의 객체가 변경되는 부수 효과가 발생한다.

<br/>

## 12.5.4 반환문

- 반환문은 두 가지 역할을 한다.
    1. 함수의 실행문을 중단하고 함수 몸체를 빠져나간다. 반환문 이후에 문은 실행되지 않는다.
    2. `return` 키워드 뒤에 오는 표현식을 평가해 반환한다. 지정하지 않으면 `undefined` 가 반환된다.
        
        ```jsx
        function foo() {
        	return;
        }
        console.log(foo()); // undefined
        ```
        
- `return` 키워드와 반환값 사이에 줄바꿈이 있으면 세미콜론 자동 삽입 기능에 의하여 의도치 않은 결과가 발생할 수 있다.
    
    ```jsx
    function multiply(x, y) {
    	return
    	x + y;
    }
    
    // 👇
    
    function multiply(x, y) {
    	return;
    	x + y; // 실행되지 않음
    }
    ```
    
- 반환문은 함수 몸체 내부에서만 사용할 수 있다.

</br></br>

# 12.6 참조에 의한 전달과 외부 상태의 변경

원시 값은 값에 의한 전달, 객체는 참조에 의한 전달 방식으로 동작한다. 매개변수도 그대로 따른다.

```jsx
function changeVal(primitive, obj) {
	primitive += 100;
	obj.name = 'Kim';
}

// 외부 상태
var num = 100;
var person = { name: 'Lee' }

console.log(num); // 100
console.log(person); // { name: 'Lee' }

// 원시 값은 값 자체가 복사되어 전달되고, 객체는 참조 값이 복사되어 전달된다.
changeVal(num, person);

// 원시값은 보존
console.log(num); // 100

// 참조값은 훼손
console.log(person); // { name: 'Kim' }
```

**원시 값은 직접 변경할 수 없기 때문에 재할당을 통해 할당된 새로운 원시 값으로 교체했고, 객체는 변경 가능한 값이므로 직접 변경할 수 있기 때문에 재할당 없이 직접 할당된 객체를 변경했다.**

1. 외부에서 `num` 과 `person` 이 메모리에 할당된다.

<p align='center'><img src='https://bl3302files.storage.live.com/y4mI0YOU_3aM1FWH7FaXpA4-94O14I6VM7D8oHjskNiYDTBlZm6HlT1KqUnsHY8Kw85nwRotPG8a_36KHihDhsKmTit2ECR8-uc5hksLpWOt05unvU4Yb4uNvC87q-4716-zLs84ZzaH5DzfzEN9aUyj5SlBze7x5CnsLwsd2ERYzeSk2ZuOwLT0bZHAmFeD9L0?width=1340&height=468&cropmode=none' width='450px'></p>

2. 함수가 호출되면 매개변수의 선언 및 초기화가 일어난다.
<p align='center'><img src='https://bl3302files.storage.live.com/y4mjYQcwHhtdoINwrHiW1jUr5LNCv7daiX85MbmFUfLrXGNj5RqC5TlCAW4BN3gVWyrSxyLXWangxfNcjHs9AbV0-6l86KmavP5H6QbYCZAqNl6_-hQJ8Nydnw92BccrwwjLDwrhGoWLdDPyh-uTmYzx2kHmmA5hblY-lai0GAmwzs9Fk77VOlf5tp0RoiRnG1g?width=668&height=468&cropmode=none' width='250px'></p>


1. `changeVal` 함수 로직에 의하여 값이 할당된다.
<p align='center'><img src='https://bl3302files.storage.live.com/y4mGgeaBp6JQLz95-NNK5azMMBRY0XR3T9_tvVM2d6Y64_ZiVwmfcrbwJzNB7qYSEe-nnp0rJIt-OI3f57yTS4ga8vjzTimoO7CSRCB6ElXHNpdLy9db4jbbjlWs8IfpNGoYngnrorzyebwf0Z7wv1WyAOgaN-RyvzHoprliKFckRaMe1vLCYtYKDWzbzk7vfnU?width=1304&height=468&cropmode=none' width='450px'></p>

4. 함수 종료후 다시 외부에서 값을 참조한다.

<p align='center'><img src='https://bl3302files.storage.live.com/y4mF_RraMM17OpBUdI4LMcjbS-YI9Nzj2vCiYk2Ev8x6OtdDSQn4kcf9uovcHTm1OXs_U7WaOKQ3WD1KtYVJN1lAZVAkS_zm3GbwwOsF7smE7InLbnsz1xcNR8wUcv4rdoCZ580sUPsvY7uwB18eQL8t1G3c3lwbVt52BnPJIDbahYBKbG61VIUZJOotLh-Td4Z?width=1366&height=468&cropmode=none' width='450px'></p>

이처럼 함수가 외부 상태를 변경하면 상태 변화를 추적하기 어려워진다. 이는 코드의 복잡성을 증가시키고 가독성을 해친다.

이러한 문제의 해결 방법 중 하나는 객체를 불변 객체로 만들어 사용하는 것이다. 객체의 복사본을 새로 생성하는 비용은 들지만 객체를 원시 값처럼 변경 불가능한 값으로 동작하게 만드는 것이다. 이는 깊은 복사(deep copy)를 통해 새로운 객체를 생성하고 재할당을 통해 교체한다.

외부 상태를 변경하지 않고 외부 상태에 의존하지도 않는 함수를 순수 함수라 한다.

<br/><br/>

# 12.7 다양한 함수의 형태

## 12.7.1 즉시 실행 함수

함수 정의와 동시에 즉시 호출되는 함수를 즉시 실행 함수라 한다.

- 단 한번만 호출되며 다시 호출할 수 없다.

즉시 실행 함수는 익명 함수를 사용하는 것이 일반적이다. 기명 함수로도 사용할 수 있지만 다시 호출할 수 없기에 의미가 없다.

```jsx
(function () {
	var a = 3;
	var b = 5;
	return a + b;
}());
```

즉시 실행 함수는 반드시 그룹 연산자 `( )` 로 감싸야 한다. 그렇지 않으면 다음과 같은 에러가 발생한다.

```jsx
function () {
	// ...
}(); // SyntaxError: Function statements require a function name
```

이는 함수 선언문은 함수 이름을 생략할 수 없는데 이름을 생략해서 나타난 에러이다.

```jsx
function foo() {}(); // SyntaxError: Unexpected token ')'
```

함수 이름을 생략할 수 없다 해서 함수 이름을 넣어 줬다. 그런데도 오류가 난다. 왜 그럴까?

이는 [ASI](https://262.ecma-international.org/#sec-automatic-semicolon-insertion)에 의해 `{}` 뒤에 세미콜론이 자동으로 삽입되기 때문이다. 따라서 `()` 는 그룹연산자로 취급되고 그룹연산자는 안에 피연산자가 없으면 에러가 난다.  *(만약 세미콜론이 없다면 자바스크립트 엔진은 이를 호출 연산자로 취급 한다.)*

그룹 연산자의 피연산자는 값으로 평가되므로 함수를 그룹 연산자로 감싸면 함수 리터럴로 평가되어 함수 객체가 된다.

```jsx
(function (){}) // ƒ (){}
```

즉, 그룹 연산자로 묶은 이유는 함수 리터럴을 평가해 함수 객체를 생성하기 위해서다. 따라서 함수 객체를 생성할 수 있다면 굳이 그룹 연산자로 묶지 않아도 된다.

다음과 같은 방법이 있다.

```jsx
(function () {
	// ...
}());

(function () {
	// ...
})();

!function () {
	// ...
}();

+function () {
	// ...
}();
```

즉시 실행 함수도 일반 함수처럼 값을 반환하고, 인수를 전달할 수 있다.

```jsx
var res = (function () {
	return 8;
}());
console.log(res); // 8

res = (function (a, b) {
	return a + b;
}(3, 5));

console.log(res); // 8
```

즉시 실행 함수를 사용하면 변수나 함수 이름의 충돌을 방지할 수 있다. (14.3 장에서 계속..)

<br/>

## 12.7.2 재귀 함수

재귀 함수는 자기 자신을 호출하는 함수를 말한다. 

- 재귀 함수는 반복되는 처리를 위해 사용된다.
- 재귀 함수는 자신을 무한 재귀 호출한다.
    - 따라서 재귀 함수 내에는 호출을 멈출 수 있는 **탈출 조건**을 반드시 만들어야 한다.

```jsx
var factorial = function (n) {
	if (n <= 1) return 1; // 탈출 조건
	return n * factorial(n - 1);
};

console.log(factorial(5)); // 120
```
<br/>

## 12.7.3 중첩 함수

함수 내부에 정의된 함수를 중첩 함수 또는 내부 함수라 한다.

- 중첩 함수를 포함하는 함수는 외부 함수라 부른다.
- 중첩 함수는 외부 함수 내부에서만 호출할 수 있다.
- 일반적으로 중첩 함수는 외부 함수를 돕는 헬퍼 함수의 역할을 한다.

```jsx
// 외부 함수
function outer() {
	var x = 1;
	
	// 중첩 함수
	function inner() {
		var y = 2;
		console.log(x + y); // 외부 함수 변수 참조 가능
	}
	inner();
}
outer();
```

ES6 이전에는 함수는 코드의 최상위 또는 다른 함수 내부에서만 정의할 수 있었으나 ES6 부터는 `if` 문이나 `for` 문 등의 코드 블록 내에서도 정의할 수 있다.

<br/>

## 12.7.4 콜백 함수

**함수의 매개변수를 통해 다른 함수의 내부로 전달되는 함수를 콜백 함수라고 한다.**

- 함수의 외부에서 콜백 함수를 전달받은 함수를 고차 함수라고 한다.
- 고차 함수는 콜백 함수를 자신의 일부분으로 합성한다.
- 고차 함수는 매개변수로 전달 받은 콜백 함수의 호출 시점을 결정해서 호출한다.
- 콜백 함수는 고차 함수에 의해 호출된다.

```jsx
function repeat(n, f) {
	for (var i = 0; i < n; i++) {
		f(i); // i를 인수로 전달하며 콜백 함수 f 호출
	}
}

var logAll = function (i) {
	console.log(i);
};

var logOdds = function (i) {
	if (i % 2) console.log(i);
};

// 전부 콘솔을 찍는 함수를 콜백 함수로 전달
repeat(5, logAll);

// 홀수만 콘솔을 찍는 함수를 콜백 함수로 전달
repeat(5, logOdds);
```

콜백 함수는 함수형 프로그래밍 패러다임뿐만 아니라 비동기 처리, 배열 고차함수 등에서 사용되므로 잘 알아두자.

<br/>

## 12.7.5 순수 함수와 비순수 함수

외부 상태에 의존하지도 않고 변경하지도 않는 부수 효과가 없는 함수를 순수 함수라 한다.

외부 상태에 의존하거나 외부 상태를 변경하는 부수 효과가 있는 함수를 비순수 함수라 한다.

<br/><br/><br/>

------------
# 세미콜론과 관하여...

함수 선언문과 함수 표현식에 관하여 공부 중 세미콜론을 넣고 안넣고의 차이가 헷갈려서 다시 알아보았다.

단순히 생각해보니 간단했다. 먼저 왜 세미콜론이 필요한지에 대해 생각해보자.  세미콜론은 문의 종료를 알리기위해 사용한다. 문은 최소의 실행 단위이고, 프로그램은 이러한 문들의 집합으로 구성된다.

따라서 세미콜론을 삽입하는 것은 문의 종료를 정확히 알려주고, 문이 의도한 바를 정확히 이행할 수 있게 해주므로 중요하다.

그럼 왜 함수 선언문도 **문**인데 왜 세미콜론을 넣어주지 않는것일까?

```jsx
function why() {
    console.log("no semicolon")
}
```

자바스크립트에서 코드 블록은 0개 이상의 문들로 이루어진 하나의 실행 단위로 그 자체로 종결성을 갖는다. 세미콜론은 문의 **종료** 를 알리기 위해서 사용하는  것인데, 코드 블록은 그 자체로 하나의 실행 단위, 종결 한다는 의미를 가지니 세미콜론을 넣어 줄 이유가 없다.
따라서 한글로 번역된 함수 선언**문**은 잘못된 표현이다. 함수 선언문은 없다. 함수 선언이라고 불러야 맞는 표현 같다. [**모질라 재단의 MDN 문서**](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Statements/function)에도 함수 선언이라는 말로 번역되어있다.

함수 표현식에는 세미콜론을 삽입해줘야 한다.

```jsx
var why = function() { console.log("yes semicolon"); };
```

피연산자로 평가된 함수 `function() { console.log("yes semicolon"); }` 가 자체 종결성을 갖더라도, 위의 코드는 변수 `why` 에 값을 할당하는 할당**문** 이기 때문에 세미콜론으로 문을 구별해줘야 하기 때문이다.

> 참조: https://www.codecademy.com/forum_questions/507f6dd09266b70200000d7e
>
