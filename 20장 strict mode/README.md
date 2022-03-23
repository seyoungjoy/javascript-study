# 20장. strict mode

## 20.1 strict mode란?

**strict mode**는 자바스크립트 언어의 문법을 좀 더 엄격히 적용하여 오류를 발생시킬 가능성이 높거나 자바스크립트 엔진의 최적화 작업에 문제를 일으킬 수 있는 코드에 대해 명시적인 에러를 발생시키는 것이다.

잠재적인 오류를 발생시키기 어려운 개발 환경을 만들고 그 환경에서 개발하기 위해 ES5부터 추가되었다.

린트 도구를 사용해도 strict mode와 유사한 효과를 얻을 수 있다.

<br/>

**린트 도구**  
정적 분석 기능을 통해 소스코드를 실행하기 전에 소스코드를 스캔하여 문법적 오류만이 아니라 잠재적 오류까지 찾아내고 오류의 원인을 리포팅해주는 유용한 도구 ex) ESLint, Prettier, 자동화 등

<br/>
## 20.2 strict mode의 적용

적용 방법 : 전역의 선두 또는 함수 몸체의 선두에 `‘use strict’;` 를 추가한다. 전역의 선두에 추가하면 스크립트 전체에 strict mode가 적용된다.

```jsx
'use strict';

function foo() {
  x = 10; // ReferenceError: x is not defined
}
foo();
```

<br/>
## 20.3 전역에 strict mode를 적용하는 것은 피하자.

전역에 적용한 strict mode는 스크립트 단위로 적용된다. 스크립트 단위로 적용된 strict mode는 다른 스크립트에 영향을 주지 않고 해당 스크립트에 한정되어 적용된다.

- strict mode 스크립트와 non-strict mode 스크립트를 혼용하면 오류 발생시킬 수 있다.
- 외부 서드파티 라이브러리 사용 경우 라이브러리가 non-strict mode인 경우도 있음.
  ⇒ 전역에 strict mode를 적용하는 것은 바람직하지 않다.
  ⇒ 즉시 실행 함수로 스크립트 전체를 감싸서 스코프를 구분한다.
  ⇒ 즉시 실행 함수의 선두에 strict mode를 적용한다.

<br/>
## 20.4 함수 단위로 strict mode를 적용하는 것도 피하자.

- 어떤 함수는 strict mode를 적용하고 어떤 함수는 적용하지 않는 것은 바람직하지 않다.
- 모든 함수에 일일이 strict mode를 적용하는 것은 번거롭다.
- strict mode가 적용된 함수가 참조할 함수 외부의 컨텍스트에 strict mode를 적용하지 않으면 문제가 발생할 수 있다.

⇒ strict mode는 즉시 실행 함수로 감싼 스크립트 단위로 적용하는 것이 바람직하다.

<br/>
## 20.5 strict mode가 발생시키는 에러

### 20.5.1 암묵적 전역

선언하지 않은 변수를 참조하면 ReferenceError가 발생

```jsx
(function () {
  'use strict';

  x = 1;
  console.log(x); // ReferenceError: x is not defined
})();
```

<br/>
### 20.5.2 변수, 함수, 매개변수의 삭제

delete 연산자로 변수, 함수, 매개변수를 삭제하면 syntaxError가 발생

```jsx
(function () {
  'use strict';

  var x = 1;
  delete x;
  // SyntaxError: Delete of an unqualified identifier in strict mode.

  function foo(a) {
    delete a;
    // SyntaxError: Delete of an unqualified identifier in strict mode.
  }
  delete foo;
  // SyntaxError: Delete of an unqualified identifier in strict mode.
}());
```

<br/>
### 20.5.3 매개변수 이름의 중복

중복된 매개변수 이름을 사용하면 syntaxError가 발생한다.

```jsx
(function () {
  'use strict';

  //SyntaxError: Duplicate parameter name not allowed in this context
  function foo(x, x) {
    return x + x;
  }
  console.log(foo(1, 2));
})();
```

<br/>
### 20.5.4 with 문의 사용

with문을 사용하면 syntaxError가 발생한다.

<br/>

**with문**

동일한 객체의 프로퍼티를 반복해서 사용할 때 객체 이름을 생략할 수 있어서 코드가 간단해지지만 성능과 가독성은 나빠진다.

```jsx
(function () {
  'use strict';

  // SyntaxError: Strict mode code may not include a with statement
  with ({ x: 1 }) {
    console.log(x);
  }
})();
```

<br/>
## 20.6 strict mode 적용에 의한 변화

### 20.6.1 일반 함수의 this

strict mode에서 함수를 일반 함수로서 호출하면 this에 undefined가 바인딩된다. ⇒ 생성자 함수가 아닌 일반 함수 내부에서는 this를 사용할 필요가 없기 때문
<br/>

```jsx
(function () {
  'use strict';

  function foo() {
    console.log(this); // undefined
  }
  foo();

  function Foo() {
    console.log(this); // Foo
  }
  new Foo();
})();
```

<br/>

### 20.6.2 arguments 객체

strict mode에서는 매개변수에 전달된 인수를 재할당하여 변경해도 arguments 객체에 반영되지 않는다.
<br/>

```jsx
(function (a) {
  'use strict';
  // 매개변수에 전달된 인수를 재할당하여 변경
  a = 2;

  // 변경된 인수가 arguments 객체에 반영되지 않는다.
  console.log(arguments); // { 0: 1, length: 1 }
})(1);
```
