# 48장 모듈
## 48.1 모듈의 일반적 의미
- 모듈 : 애플리케이션을 구성하는 개별적 요소로서 재사용 가능한 코드 조각
- 기능을 기준으로 파일 단위로 분리.
- 모듈이 성립하려면 모듈은 자신만의 파일 스코프를 가질 수 있어야 한다.
- 모듈은 캡슐화가 되어 다른 모듈에서 접근할 수 없다.
- export로 선택적 공개가 가능하여 재사용이 가능해진다.
- 모듈 사용자는 모듈이 공개한 자산 중 일부 또는 전체를 선택해 자신의 스코프 내로 불러들여 재사용할 수 있다. 이를 import라 한다.
<p align="center"><img src="./img/48-1.jpg" width="50%"></p>

- 모듈은 애플리케이션과 분리되어 개별적으로 존재하다가, 필요에 따라 다른 모듈에 의해 재사용된다.
- 모듈은 기능별로 분리되어 개별적인 파일로 작성된다.
- 따라서 코드의 단위를 명확히 분리하여 애플리케이션을 구성할 수 있고, 재사용성이 좋아서 개발 효율성과 유지보수성을 높일 수 있다.

## 48.2 자바스크립트와 모듈
- 자바스크립트는 웹페이지의 단순한 보조 기능을 처리하기 위한 목적으로 태어나 부족한 부분이 있었고, 대표적으로 모듈 시스템을 지원하지 않았다.
- script 태그를 사용해 외부의 자바스크립트 파일을 로드할 수 있지만 파일마다 독립적인 스코프를 갖지 않는다.
- 자바스크립트 파일은 여러 개의 파일로 분리해도 하나의 전역을 공유했기 때문에 전역 변수 중복 등 문제가 발생할 수 있다.
- 이때 모듈 시스템을 해결하기 위해 CommonJS와 AMD(Asynchronous Module Definition)이 제안
- 브라우저 환경에서 모듈을 사용하기 위해 CommonJS 와 AMD 라이브러리를 사용해야 했다.- 자바스크립트 런타임 환경인 Node.js는 CommonJS를 택했고 CommonJS와 100% 동일하진 않지만 기본적으로 CommonJS 사양을 따라 모듈 시스템을 지원한다.
- 따라서 Node.js 환경에서는 파일별로 독립적인 파일 스코프를 갖는다.

#### **CommonJS**
- 내보내기
```jsx
const exchangeRate = 0.91;

function roundTwoDecimals(amount) {
  return Math.round(amount * 100) / 100;
}

const canadianToUs = function (canadian) {
  return roundTwoDecimals(canadian * exchangeRate);
};

function usToCanadian(us) {
  return roundTwoDecimals(us / exchangeRate);
}

exports.canadianToUs = canadianToUs; // 내보내기 1
exports.usToCanadian = usToCanadian; // 내보내기 2
```

- 불러오기
```jsx
const currency = require("./currency-functions");

console.log("50 Canadian dollars equals this amount of US dollars:");
console.log(currency.canadianToUs(50));

console.log("30 US dollars equals this amount of Canadian dollars:");
console.log(currency.usToCanadian(30));
```

## 48.3 ES6(ESM,ECMAScript Module)
- ES6에서 모듈 기능을 추가했고 IE를 제외한 대부분의 브라우저에서 ES6 모듈을 사용할 수 있다.
- 사용법 : script 태그에 type="module" 어트리뷰트를 추가, 확장자는 mjs를 사용
```html
<script type="module" src="app.mjs"></script>
```

### 48.1 모듈 스코프
- ESM은 파일 자체의 독자적인 모듈 스코프를 제공한다.
- 모듈 내부에서 선언한 모든 식별자는 해당 모듈 내부에서만 참조할 수 있다.
```html
<body>
    <script type="module" src="foo.mjs"></script>
    <script type="module" src="bar.mjs"></script>
</body>
```

```jsx
//foo.mjs
var x = 'foo';
console.log(x); //foo
//var로 전연변수로 선언했지만 전역 변수로 사용할 수 없다.
console.log(window.x); //undefined
```

```jsx
//bar.mjs
console.log(x);//ReferenceError x is not defined
```

### 48.3.2 export 키워드
- 모듈 내부에서 선언한 식별자를 외부에 공개하여 다른 모듈들이 재사용할 수 있도록 export 키워드를 사용한다.
```jsx
//lib.mjs
//변수 공개
export const pi = Maht.PI;

//함수 공개
export function square(x){
    return x * x;
}

//클래스 공개
export class Person {
    constructor(name){
        this.name = name;
    }
}
```

- export할 대상을 하나의 객체로 구성하여 한 번에 export도 가능.
```jsx
//lib.mjs
const pi = Maht.PI;

function square(x){
    return x * x;
}

class Person {
    constructor(name){
        this.name = name;
    }
}

export{pi, square, Person};
```

### 48.4.4 import 키워드
- 다른 모듈에서 공개한 식별자를 자신의 모듈 스코프 내부로 로드하려면 import 키워드를 사용한다.
- 다른 모듈이 export한 식별자 이름으로 import 해야하며
- ESM 파일 확장자는 생략할 수 없다.

```jsx
//app.mjs
import {pi, square, Person} from './lib.mjs';

console.log(pi);
console.log(square(10));
console.log(new Person('Lee'));
```
- app.mjs 는 애플리케이션의 진입점이기 때문에 반드시 script 태그로 로드해야 하며
- lib.mjs는 import에 의해 로드되는 의존성이 있어 따로 로드하지 않아도 된다.
```html
<body>
    <script type="module" src="app.mjs"></script>
</body>
```

- export한 식별자 이름을 일일이 지정하지 않고 한번에 import 할 수도 있다.
```jsx
//app.mjs
//import되는 식별자는 as 뒤에 지정한 이름의 객체 lib에 프로퍼티로 할당된다.
import * as lib from './lib.mjs';

console.log(lib.pi);
console.log(lib.square(10));
console.log(new lib.Person('Lee'));
```

- 모듈이 export한 식별자 이름을 변경하여 import 할 수도 있다.
```jsx
//app.mjs
import {pi as PI, square as sq, Person as P} from './lib.mjs';

console.log(PI);
console.log(sq(10));
console.log(new P('Lee'));
```
- 모듈에서 하나의 값만 export 한다면 default 키워드를 사용할 수 있다.
```jsx
//lib.mjs
export default x => x * x;
```
