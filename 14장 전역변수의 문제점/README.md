# 14장 전역변수의 문제점

## 14.1 변수의 생명주기

- 변수는 자신이 선언된 위치에서 생성되고 소멸된다.
- **전역 변수**의 생명 주기 = 애플리케이션의 생명 주기
- **지역 변수**의 생명 주기 = 함수의 생명 주기

### 14.1.1 지역 변수의 생명주기

- 함수 내부에서 선언된 지역 변수는 함수가 호출되면 생성되고 함수가 종료하면 소멸된다.
- 함수 내부에 선언한 변수는 함수가 호출된 직후에 함수 몸체의 다른 코드가 실행되기 이전에 JS엔진에 의해 먼저 실행되어 변수가 선언되고 `undefined`로 초기화된다.
- **호이스팅은 스코프를 단위로 동작한다.**

```javascript
function foo() {
  var x = 'local';
  console.log(x); // local
  return x;
}

foo();
console.log(x); // ReferenceError: x is not defined

// 호이스팅
var y = 'global';

function bar() {
  console.log(y); // undefined
  var y = 'local';
  return y;
}

bar(); // -> undefined출력, 'local' 반환
console.log(bar); // -> [Function: bar]
console.log(bar()); // -> undefined, local
console.log(y); // -> global
```

### 14.1.2 전역 변수의 생명주기

- 전역 코드는 명시적인 호출없이 실행된다.
  - 특별한 진입점(entry point)이 없고 코드가 로드되자마자 실행된다.
- 마지막 문이 실행되어 더 이상 실행할 문이 없을 때 종료된다.
- var 키워드로 선언한 전역 변수 = 전역 객체의 프로퍼티 - 전역 변수의 생명 주기 = 전역 객체의 생명 주기 - **전역 객체**: 코드가 실행되기 이전 단계에 JS엔진에 의해 생성되는 특수한 객체 - 클라이언트 사이드 환경(브라우저): window 객체 - 서버 사이드 환경(Node.js): global 객체 - ES11: globalThis
  <br>

---

## 14.2 전역 변수의 문제점

**암묵적 결합**

- 모든 코드가 전역 변수를 참조하고 변경할 수 있는 **암묵적 결합**을 허용한다.
- 변수의 유효범위가 클수록 코드의 가독성이 나빠지고 의도치 않게 상태가 변경될 수 있는 위험성도 높아진다.

**긴 생명 주기**

- 전역 변수의 생명 주기는 길다. 따라서,
  메모리 리소스 오랜 기간 소비한다. 또한,
  전역 변수의 상태를 변경할 수 있는 시간도 길고, 모든 함수가 참조할 수 있기 때문에 상태를 변경할 기회도 많아진다. var 키워드는 변수의 중복 선언 허용되기 때문에 버그를 발생시킬 확률을 높인다.

  ```javascript
  var x = 1;

  // 변수의 중복 선언에 의한 의도치 않은 재할당
  var x = 'Hello';

  console.log(x); // -> Hello
  ```

**스코프 체인 상에서 종점에 존재**

- 변수를 검색할 때 전역 변수가 가장 마지막에 검색된다.
- 전역 변수의 검색 속도가 가장 느리다.

**네임 스페이스 오염**

- 파일이 분리되어 있더라도 하나의 전역 스코프 공유한다.
- 다른 파일 내에서도 동일한 이름의 변수나 함수가 같은 스코프 내에 존재할 경우 예상치 못한 결과 초래한다.

<br>

---

## 14.3 전역 변수 사용 억제 방법

- 전역 변수를 반드시 사용하여야 할 이유를 찾지 못하면 지역 변수를 사용한다.
- 변수의 스코프는 좁을수록 좋다.
- 무분별한 전역 변수 사용은 지양하자.

### 14.3.1 즉시 실행 함수

- 함수의 정의와 동시에 단 한번만 호출된다.
- 모든 코드를 즉시 실행 함수로 감싸면 모든 변수는 즉시 실행 함수의 지역 변수가 된다.

```javascript
(function () {
  var foo = 10; // 즉시 실행 함수의 지역 변수
})();

console.log(foo); // ReferenceError
```

### 14.3.2 네임스페이스 객체

- 전역에 네임 스페이스(Namespace) 역할을 담당할 객체 생성한다. 전역 변수처럼 사용하고 싶은 변수를 프로퍼티로 추가한다.
- 네임스페이스 분리를 통해 식별자 충돌 방지할 수 있다. (하지만 네임 스페이스 객체가 전역 변수에 할당되므로 실용성은 떨어져보인다.)

```javascript
var MYAPP = {}; // 전역 네임 스페이스 객체

MYAPP.name = 'Joo';
console.log(MYAPP.name); // -> Joo

// 네임 스페이스를 계층적으로 구성
MYAPP.person = {
  age: 27;
  address: 'Seoul';
}

console.log(MYAPP.pereson.age); // -> 27
```

### 14.4.3 모듈 패턴

- 클래스를 모방하여 관련이 있는 변수와 함수를 모아 즉시 실행 함수로 감싸 하나의 모듈 생성한다.
- 클로저 기반으로 동작한다.
- 전역 변수의 억제한다.
- **캡슐화(Encapsulation)**: 외부에 공개될 필요가 없는 정보를 외부에 노출시키지 않고 숨기는 것이다.( = 정보은닉 )
- 전역 네임 스페이스의 오염을 막는 기능은 한정적이다.

```javascript
var Counter = (function () {
  // private 변수
  var num = 0;

  // 외부로 공개할 데이터나 메소드를 프로퍼티로 추가한 객체를 반환
  return {
    increase() {
      return ++num;
    },
    decrease() {
      return --num;
    },
  };
})();

// private 변수는 외부로 노출되지 않음
console.log(Counter.num); // undefined

console.log(Counter.increase()); // 1
console.log(Counter.increase()); // 2
console.log(Counter.decrease()); // 1
```

### 14.3.4 ES6 모듈

- 모던 브라우저(Chrome 61, FF 60, SF 10.1, Edge 16 이상)에서 ES6 모듈을 사용 가능하다.
- script 태그에 type="module" 어트리뷰트를 추가하면 로드된 자바스크립트 파일은 모듀로서 동작한다. 모듈 확장자는 mjs 권장한다.

```html
<script type="module" src="lib.mjs"></script>
<script type="module" src="app.mjs"></script>
```

<br>
