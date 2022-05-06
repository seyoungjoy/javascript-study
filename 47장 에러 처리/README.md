# 47장 에러 처리
## 47.1 에러 처리의 필요성
- 에러는 언제나 발생할 수 있고 발생한 에러에 대해 대처하지 않고 방치하면 프로그램은 강제 종료된다.
```jsx
console.log('[Start]');

foo();

console.log('[End]');
```
<p align="center"><img src="./img/47-1.jpg" width="60%"></p>

- try...catch 문을 사용해 발생한 에러에 적절하게 대응하면 프로그램이 강제 종료되지 않고 계속해서 코드를 실행시킬 수 있다.

```jsx
console.log('[Start]');

try{
    foo();
} catch(error){
    console.log('[에러 발생], error');
}
//발생한 에러에 적절한 대응을 하며 프로그램이 강제 종료되지 않는다.
console.log('[End]');
/*
    [Start]
    [에러 발생], error
    [End]
*/
```

- 직접적으로 에러를 발생하지는 않는 예외적인 상황이 발생할 수도 있다. 예외적인 상황에 적절하게 대응하지 않으면 에러로 이어질 가능성이 크다.
```jsx
//DOM에 button 요소가 존재하지 않으면 querySelector 메서드는 에러를 발생시키지 않고 null을 반환
const $button = document.querySelector('button');
console.log($button); //null

$button.classList.add('disabled');
//Uncaught TypeError
```
- 위 예제의 querySelector 메서드는 인수로 전달한 문자열이 CSS 선택자 문법에 맞지 않는 경우 에러를 발생시킨다.
```jsx
const $elem = document.querySelector('#1');
//Uncaught DOMException
```
- 하지만 querySelector 메서드는 인수로 전달한 CSS 선택자 문자열로 DOM에서 요소 노드를 찾을 수 없는 경우 에러를 발생시키지 않고 null을 반환한다.<br>
이때 if 문으로 querySelector 메서드의 반환값을 확인하거나 단축 평가 또는 옵셔널 체이닝 연산자 ?. 를 사용하지 않으면 다음 처리에서 에러로 이어질 가능성이 크다.
```jsx
//DOM에 button 요소가 존재하는 경우 querySelector 메서드는 에러를 발생시키지 않고 null을 반환한다.
const $button = document.querySelector('button');
$button?.classList.add('disabled');
console.log($button); //null
```
- 이처럼 에러나 예외적인 상황에 대응하지 않으면 프로그램은 강제 종료될 것이다.
- 에러나 예외적인 상황은 너무나 다양하기 때문에 아무런 조치 없이 프로그램이 강제 종료된다면 원인을 파악하여 대응하기 어렵다.
- 따라서 우리가 작성한 코드에서는 언제나 에러나 예외적인 상황이 발생할 수 있다는 것을 전제하고 이에 대응하는 코드를 작성하는 것이 중요하다.

## 47.2 try...catch...finally문
- 에러 처리를 구현하는 방법은 크게 2가지가 있는데
- querySelector나 Array#find 메서드처럼 예외적인 상황이 발생하면 반환하는 값(-1 or null)을 if 문이나 단축 평가 또는 옵셔널 체이닝 연산자를 통해 확인해서 처리하는 방법
- 에러 처리 코드를 미리 등록해 두고 에러가 발생하면 에러 처리 코드로 점프하도록 하는 방법(try...catch...finally)
- finally문은 생략 가능
```jsx
try{
    //실행할 코드(에러가 발생할 가능성이 있는 코드)
} catch(err){
    //try 코드 블록에서 에러가 발생하면 이 코드 블록의 코드가 실행된다.
    //err에는 try 코드 블록에서 발생한 Error 객체가 전달된다.
} finally{
    //에러 발생과 상관없이 반드시 한 번 실행된다.
}
```

- try...catch...finally 문을 실행하면 try 코드 블록이 실행되고
- 이때 try 코드 블록에 포함된 문 중에서 에러가 발생하면 발생한 에러는 catch 문의 err 변수에 전달되고 catch 코드 블록이 실행된다. catch 문의 err변수는 try 코드 블록에 포함된 문 중에서 에러가 발생하면 생성되고 catch 코드 블록에서만 유효하다. 
- finally 코드 블록은 에러 발생과 상관없이 반드시 한 번 실행된다.
- try...catch...finally문으로 에러를 처리하면 프로그램이 강제 종료되지 않는다.
```jsx
console.log('[Start]');

try{
    foo();
} catch(err){
    console.error(err);
} finally{
    console.log('finally');
}

console.log('[End]');
```
<p align="center"><img src="./img/47-2.jpg" width="60%"></p>

## 47.3 Error 객체
- Error 생성자 함수는 에러 객체를 생성한다. Error 생성자 함수에는 에러를 상세히 설명하는 에러 메시지를 인수로 전달할 수 있다.
```jsx
const error = new Error('invalid');
```
- Error 생성자 함수가 생성한 에러 객체는 message 프로퍼티와 stack 프로퍼티를 갖는다.<br>
message 프로퍼티의 값은 Error 생성자 함수에 인수로 전달한 에러 메시지이고, stack 프로퍼티의 값은 에러를 발생시킨 콜스택의 호출 정보를 나타내는 문자열이며 디버깅 목적으로 사용한다.
- 자바스크립트는 Error 생성자 함수를 포함해 7가지의 에러 객체를 생성할 수 있는 Error 생성자 함수를 제공한다.

|생성자 함수|인스턴스|
|:---:|:---:|
|Error|일반적 에러 객체|
|SyntaxError|자바스크립트 문법에 맞지 않는 문을 해석할 때 발생하는 에러 객체|
|ReferenceError|참조할 수 없는 식별자를 참조했을 때 발생하는 에러 객체|
|TypeError|피연산자 또는 인수의 데이터 타입이 유효하지 않을 때 발생하는 에러 객체|
|RangeError|숫자값의 허용 범위를 벗어났을 때 발생하는 에러 객체|
|URIError|encodeURI 또는 decodeURI 함수에 부적절한 인수를 전달했을 때 발생하는 에러 객체|
|EvalError|eval 함수에서 발생하는 에러 객체|

```jsx
1 @ 1 //Uncaught SyntaxError: Invalid or unexpected token

foo(); //Uncaught ReferenceError: foo is not defined

null.foo; //Uncaught TypeError: Cannot read properties of null (reading 'foo')

new Array(-1); //Uncaught RangeError: Invalid array length

decodeURIComponent('%'); //Uncaught URIError: URI malformed
```

## 47.4 throw 문
- Error 생성자 함수로 에러 객체를 생성한다고 에러가 발생하는 것은 아니다. 즉, 에러 객체 생성과 에러 발생은 의미가 다르다.

```jsx
try{
    //에러 객체를 생성한다고 에러가 발생하는 것은 아니다.
    new Error('something wrong');
} catch(error){
    console.log(error);
}
``` 
- 에러를 발생시키려면 try 코드 블록에서 throw 문으로 에러 객체를 던져야 한다.
```
throw 표현식;
```
- throw 문의 표현식은 어떤 값이라도 상관없지만 일반적으로 에러 객체를 지정한다. 에러를 던지면 catch 문의 에러 변수가 생성되괴 던져진 에러 객체가 할당된다. 그리고 catch 코드 블록이 실행되기 시작한다.
```jsx
try{
    //에러 객체를 던지면 catch 코드 블록이 실행되기 시작
    throw new Error('something wrong');
} catch(err){
    console.log(err); //Error: something wrong
}
```
```jsx
//외부에서 전달받은 콜백함수를 n 번만큼 반복 호출한다.
const repeat = (n,f) => {
    //매개 변수 f에 전달된 인수가 함수가 아니면 typeError를 발생시킨다.
    if(typeof f !== 'function') throw new TypeError('f must be a function');

    for (var i =0; i<n; i++){
        f(i); //i를 전달하면서 f를 호출
    }
}

try{
    repeat(2,1); //두번째 인수가 함수가 아니므로 TypeError가 발생한다.
} catch(err){
    console.log(err); //TypeError : f must be a function
}
```

## 47.5 에러의 전파
- 콜 스택의 아래 방향(실행 중인 실행 컨텍스트가 푸시되기 직전에 푸시된 실행 컨텍스트 방향)으로 전파된다.
```jsx
const foo = () => {
    throw Error('foo에서 발생한 애러'); //에러를 throw 한다.
};

const bar = () => {
    foo(); //foo 함수 호출
}

const baz = () => {
    bar(); //bar 함수 호출
}

try{
    baz(); //baz 함수를 호출
} catch (err){
    console.log(err);
}
```
<p align="center"><img src="./img/47-3.jpg" width="50%"></p>
- throw 된 에러를 캐치하지 않으면 호출자 방향으로 전파된다. 이때 throw 된 에러를 캐치하여 적절히 대응하면 프로그램을 강제 종료시키지 않고 코드의 실행 흐름을 복구할 수 있다. throw 된 에러를 어디에서도 캐치하지 않으면 프로그램은 강제 종료된다.
