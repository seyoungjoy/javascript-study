# 33.1 심벌이란?

ES6 이전에 자바스크립트에는 문자열, 숫자, 불리언, undefined, null, 객체 총 6개의 타입이 있었다.

심벌은 ES6에서 도입된 7번째 데이터 타입으로 변경 불가능한 원시 타입의 값이다.

심벌 값은 다른 값과 중복되지 않는 유일무히안 값으로 충돌 위험이 없는 유일한 프로퍼티 키를 만들기 위해 사용한다.

<br/>

# 33.2 심벌 값의 생성

## 33.2.1 Symbol 함수

다른 원시값들은 리터럴 표기법을 통해 값을 생성할 수 있지만 심벌 값은 `Symbol` 함수를 호출하여 생성해야 한다. 이때 생성된 심벌 값은 외부로 노출되지 않아 확인할 수 없으며, **다른 값과 절대 중복되지 않는 유일무이한 값이다.**

```jsx
const mySymbol = Symbol();
console.log(mySymbol); // Symbol() 외부로 노출되지 않아 확인 불가
```

`Symbol` 함수는 `new` 연산자와 함께 호출하지 않는다.

<p align='center'>
<img width = '400px' src='https://bl3302files.storage.live.com/y4mPAcOaT8lNPVgNfc-TxyWemPgIWvGLKTUP8rA_VVjNZY2t5tzNnoSXDYVrMMQz4gOGI30gYAIw8HQekLzccS0K1T-k8scADwNHC6W_JbrixFCnh40NDweJVmfHAkS3QG0QWpoPhNlcgln2kEH55a-QG3f_uQcLSKYjZVvlpYOSrBH0XOsWzKGahSl0iKf9TJX?width=706&height=130&cropmode=none'>
</p>

`Symbol` 함수에는 선택적으로 문자열을 인수로 전달할 수 있다. 이 문자열은 생성된 심벌 값에 대한 설명으로 디버깅 용도로만 사용되며 값에는 어떠한 영향도 주지 않는다. 즉, 설명이 같더라도 심벌은 유일무이한 식별 값을 생성한다.

심벌 값은 암묵적으로 문자열이나 숫자 타입으로 변환되지 않는다.

```jsx
const mySymbol = Symbol();

console.log(mySymbol + ""); // Cannot convert a Symbol value to a string
console.log(+mySymbol); // Cannot convert a Symbol value to a nunmber
```

단, 불리언 타입으로는 암묵적으로 변환되며 이를 통해 `if` 문 등에서 존재 확인이 가능하다.

```jsx
const mySymbol = Symbol();

if (mySymbol) console.log("exist");
```

<br/>

## 33.2.2 Symbol.for / Symbol.keyFor 메서드

`Symbol.for` 메서드는 인수로 전달받은 문자열을 키로 사용하여 키와 심벌 값의 쌍이 저장되어 있는 전역 심벌 레지스트리에서 해당 키와 일치하는 식별 값을 검색한다.

-   검색에 성공하면 검색된 심벌 값을 반환한다.
-   만약 검색에 실패하면 새로운 심벌값을 생성하여 전달받은 키로 전역 심벌 레지스트리에 저장한 후, 생성된 심벌 값을 반환한다.

```jsx
const s1 = Symbol.for("mySymbol"); // mySymbol이 키로 지정된 심벌 값이 없으므로 생성후 반환
const s2 = Symbol.for("mySymbol"); // mySymbol이 키로 지정된 심벌 값을 찾아 반환

s1 === s2; //  true
```

`Symbol` 함수는 호출될 때마다 유일무이한 심벌 값을 생성한다. 이때 전역 심벌 레지스트리에서 키를 지정할 수 없으므로 전역 심벌 레지스트리에 등록되어 관리되지 않는다. 하지만 `Symbol.for` 메서드를 사용하면 전역에서 중복되지 않는 유일무이한 상수인 심벌 값을 단 하나만 생성하여 공유할 수 있다.

`Symbol.keyFor` 메서드를 사용하면 저장된 심벌 값의 키를 추출할 수 있다.

```jsx
const s1 = Symbol.for("mySymbol");
Symbol.keyFor(s1); // mySymbol

const s2 = Symbol.for("foo");
Symbol.keyFor(s2); // foo
```

# 33.3 심벌과 상수

위, 아래, 왼쪽, 오른쪽을 나타내는 상수를 정의한다고 해보자.

```jsx
const Direction = {
    UP: 1,
    DOWN: 2,
    LEFT: 3,
    RIGHT: 4,
};

const myDirection = Direction.UP;
```

위 예제와 같이 값에는 특별한 의미가 없고 상수 이름 자체에 의미가 있는 경우가 있다. 이때 문제는 상수 값 1, 2, 3, 4는 변경될 수 있으며, 다른 변수 값과 중복될 수도 있다. 이런 경우 중복될 가능성이 없는 심벌 값을 사용할 수 있다.

```jsx
const Direction = {
    UP: Symbol("up"),
    DOWN: Symbol("down"),
    LEFT: Symbol("left"),
    RIGHT: Symbol("right"),
};

const myDirection = Direction.UP;
```

또한 `Object.freeze` 와 같이 `enum` 을 흉내 내어 사용할 수 있다.

```jsx
const Direction = Object.freeze({
    UP: Symbol("up"),
    DOWN: Symbol("down"),
    LEFT: Symbol("left"),
    RIGHT: Symbol("right"),
});

const myDirection = Direction.UP;
```

<br/>

# 33.4 심벌과 프로퍼티 키

객체의 프로퍼티 키는 모든 문자열 또는 심벌 값으로 만들 수 있다. 심벌 값을 프로퍼티 키로 사용하려면 대괄호를 사용해야 한다. 접근할 때도 마찬가지로 대괄호를 사용한다.

```jsx
const obj = {
    [Symbol.for("mySymbol")]: 1,
};

obj[Symbol.for("mySymbol")]; // 1
```

**심벌 값은 유일무이한 값이므로 절대 프로퍼티 키가 충돌하지 않는다.**

<br/>

# 33.5 심벌과 프로퍼티 은닉

심벌 값을 프로퍼티 키로 사용하면 `for ... in` 문이나 `Object.keys` , `Object.getOwnPropertyNames` 메서드로 찾을 수 없다. 이처럼 심벌 값을 프로퍼티 키로 사용하면 외부에 노출할 필요가 없는 프로퍼티를 은닉할 수 있다.

하지만 프로퍼티를 완벽하게 숨길 수 있는 것은 아니다. ES6에서 도입된 `Object.getOwnPropertySymbols` 메서드를 사용하면 심벌 값도 나타난다.

```jsx
const obj = {
    [Symbol.for("mySymbol")]: 1,
};

const symbolkey1 = Object.getOwnPropertySymbols(obj)[0];
console.log(obj[symbolkey1]); // 1
```

<br/>

# 33.6 심벌과 표준 빌트인 객체 확장

일반적으로 표준 빌트인 객체에 사용자 정의 메서드를 직접 추가하여 확장하는 것은 권장하지 않는다.

```jsx
Array.prototype.sum = function () {
    return this.reduce((acc, cur) => acc + cur, 0);
};
```

그 이유는 개발자가 직접 추가한 메서드와 미래에 표준 사양으로 추가될 메서드의 이름이 중복될 수 있기 때문이다.

하지만 중복될 가능성이 없는 심벌 값으로 프로퍼티 키를 생성하여 표준 빌트인 객체를 확장하면 안전하게 확장할 수 있다.

```jsx
Array.prototype[Symbol.for("sum")] = function () {
    return this.reduce((acc, cur) => acc + cur, 0);
};

[1, 2][Symbol.for("sum")]();
```

<br/>

# 33.7 Well-know Symbol

자바스크립트가 기본 제공하는 빌트인 심벌 값이 있다.

```jsx
console.dir(Symbol);
```

<p align='center'>
<img width = '400px' src='https://bl3302files.storage.live.com/y4m6rVb6ncyCp89mzWPgUEeFL_vLCU0DR8k3-7glS_IlyzG493-OAHJmRo-O0W6Ln4J2vkKSNmsarKd8na0WUUPuMBFByXJVnJGgmuewKcRCPeIBhPdTFSohrlK8tOgQeUqFyOQcgK4x9RwpHPu_rchPecmAFn1RWZwF9O2XXCFA_BsIQHIUSdhj9D0qGVcU81z?width=1446&height=784&cropmode=none'>
</p>

자바스크립트가 기본 제공하는 빌트인 심벌 값을 ECMAScript 사양에서는 Well-know Symbol 이라 부른다. Well-known Symbol은 자바스크립트 엔진의 내부 알고리즘에 사용된다.

예를 들어, `Array` , `String` , `Map` , `Set` 와 같이 `for...of` 문으로 순회 가능한 빌트인 이터러벌은 `Symbol.iterator` 를 키로 갖는 메서드를 가진다. `Symbol.iterator` 메서드를 호출하면 이터레이터를 반환하도록 ECMAScript 사양에 규정되어 있다. 빌트인 이터러벌은 이터레이션 프로토콜을 준수한다.

만약 이터러블이 아닌 일반 객체를 이터러블처럼 동작하도록 구현하고 싶다면 이터레이션 프로토콜을 따르면 된다.
