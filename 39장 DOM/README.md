# 39장 DOM
- 브라우저의 렌더링 엔진은 HTML 문서를 파싱하여 브라우저가 이해할 수 있는 자료구조인 DOM을 생성.
- DOM(Document Object Model)은 HTML 문서의 계층적 구조와 정보를 표현하며 이를 제어할 수 있는 API, 즉 프로퍼티와 메서드를 제공하는 트리 자료구조다.

## 39.1 노드
### 39.1.1 HTML 요소와 노드 객체
- HTML 요소는 HTML 문서를 구성하는 개별적인 요소를 의미
- HTML 요소는 렌더링 엔진에 의해 파싱되어 DOM을 구성하는 요소 노드 객체로 변환된다. 이때 HTML 요소의 어트리뷰트는 어트리뷰트 노드로, HTML 요소의 텍스트 콘텐츠는 텍스트 노드로 변환된다.
<p align="center"><img src="./img/js-39-1.jpg" width="100%"></p>
- HTML 요소는 중첩관계를 가지며 이때 계층적인 부자 관계가 형성된다.
- HTML 요소 간의 부자 관계를 반영하여 HTML 문서의 구성 요소인 HTML 요소를 객체화한 모든 노드 객체들을 트리 자료로 구성한다.
<br><br>

#### **트리 자료구조(Tree Data Structure)**
<p align="center"><img src="./img/js-39-2.jpg" width="60%"></p>

- 트리 자료구조는 부모 노드와 자식 노드로 구성되어 노드 간의 계층적 구조를 표현하는 비선형 자료구조
- 최상위 노드는 부모 노드가 없으며, **루트 노드**라 한다.
- 자식 노드가 없는 노드를 **리프 노드**라 한다.
- **노드 객체들로 루성된 트리 자료구조를 DOM**이라 한다. 노드 객체의 트리로 구조화되어 있기 때문에 DOM을 DOM 트리라고도 부른다.

### 39.1.2 노드 객체의 타입
```html
<!DOCTYPE html>
<html>
<head>
    <meta charset="UTF-8">
    <link rel="stylesheet" href="style.css">
</head>
<body>
    <ul>
        <li id="apple">Apple</li>
        <li id="banana">banana</li>
        <li id="orange">Orange</li>
    </ul>
<script src="app.js"></script>
</body>
</html>
```
<p align="center"><img src="./img/js-39-3.jpg" width="100%"></p>

#### 문서 노드
- DOM 트리의 최상위에 존재하는 루트 노드.
- document 객체를 가리킨다.
- document 객체는 브라우저가 렌더링한 HTML 문서 전체를 가리키는 객체로서 전역 객체 window의 document 프로퍼티에 바인딩되어 있다. 따라서 문서 노드는 window.document 또는 document로 참조할 수 있다.
- 문서 노드, 즉 document 객체는 DOM 트리의 루트 노드이므로 DOM 트리의 노드들에 접근하기 위한 진입점 역할을 담당한다.즉 요소, 어트리뷰트, 텍스트 노드에 접근하려면 문서 노드를 통해야 한다.

#### 요소 노드
- HTML 요소를 가리키는 객체
- 요소 노드는 HTML 요소 간의 중첩에 의해 부자 관계를 가지며, 이 부자 관계를 통해 정보를 구조화한다. 따라서 요소 노드는 문서의 구조를 표현한다고 할 수 있다.

#### 어트리뷰트 노드
- 어트리뷰트 노드는 HTML 요소의 어트리뷰트를 가리키는 객체.
- 요소 노드는 부모 노드와 연결되어 있지만 어트리뷰트 노드는 요소 노드에만 연결되어 있다.
- 어트리뷰트 노드에 접근하여 어트리뷰트를 참조하거나 변경하려면 먼저 요소 노드에 접근해야 한다.

#### 텍스트 노드
- HTML 요소의 텍스트를 가리키는 객체다.
- 텍스트 노드는 요소 노드의 자식 노드이며, 자식 노드를 가질 수 없는 리프 노드다.
- 즉, 텍스트 노드는 DOM 트리의 최종단이다.
- 따라서 텍스트 노드에 접근하려면 먼저 부모 노드인 요소 노드에 접근해야 한다.

### 39.1.3 노드 객체의 상속 구조
- DOM은 HTML 문서의 계층적 구조와 정보를 표현하며, 이를 제어할 수 있는 API, 즉 프로퍼티와 메서드를 제공하는 트리 자료구조라고 했다.
- DOM을 구성하는 노드 객체는 구조와 정보를 제어할 수 있는 DOM API를 사용할 수 있다.
- 이를 통해 노드 객체는 자신의 부모, 형제, 자식을 탐색할 수 있으며, 자신의 어트리뷰트와 텍스트를 조작할 수도 있다.
- DOM을 구성하는 노드 객체는 ECMAScript 사양에 정의된 표준 빌트인 객체가 아니라 브라우저 환경에서 추가적으로 제공하는 호스트 객체다.
- 하지만 노드 객체도 자바스크립트 객체이므로 프로토타입에 의한 상속 구조를 갖는다. 

<p align="center"><img src="./img/js-39-4.jpg" width="60%"></p>

- 모든 노드 객체는 Object, EventTarget, Node 인터페이스를 상속받는다.
- 문서 노드는 Document, HTMLDocument 인터페이스를 상속받고
- 어트리뷰트 노드는 Attr,
- 텍스트 노드는 CharacterData 인터페이스를 상속받는다.
- 요소 노드는 Element 인터페이스를 상속받고 HTMLElement와 태그의 종류별로 세분화된 인터페이스를 상속받는다.

<p align="center"><img src="./img/js-39-5.jpg" width="60%"></p>

- input 요소 노드 객체는 다양한 특성을 갖는 객체이며, 이러한 특성을 나타내는 기능들을 상속을 통해 제공받는다.

|input 요소 노드 객체의 특성|프로토타입을 제공하는 객체|
|:---:|:---:|
|객체|Object|
|이벤트를 발생시키는 객체|EventTarget|
|트리 자료구조의 노드 객체|Node|
|브라우저가 렌더링할 수 있는 웹 문서의 요소를 표현하는 객체|Element|
|웹 문서의 요소 중에서 HTML 요소를 표현하는 객체|HTMLElement|
|HTML 요소 중에서 input 요소를 표현하는 객체|HTMLInputElement|

- DOM은 HTML 문서의 계층적 구조와 정보를 표현하는 것은 물론 노드 객체의 종류, 즉 노드 타입에 따라 필요한 기능을 프로퍼티와 메서드의 집합인 DOM API로 제공한다. 이 DOM API를 통해 HTML 구조나 내용 또는 스타일 등을 동적으로 조작할 수 있다.

## 39.2 요소 노드 취득
- HTML의 구조나 내용 또는 스타일 등을 동적으로 조작하려면 요소 노드를 취해야 한다.
### 39.2.1 id를 이용한 요소 노드 취득
- `Document.prototype.getElementById`
- 인수로 전달한 id 어트리뷰트 값을 갖는 하나의 요소 노드를 탐색하여 반환.

```html
<body>
    <ul>
        <li id="apple">Apple</li>
        <li id="banana">Banana</li>
        <li id="orange">Orange</li>
    </ul>
<script>
    //id 값이 'banana'인 요소 노드를 탐색하여 반환한다.
    //두 번째 li 요소가 파싱되어 생성된 요소 노드가 반환된다.
    const $elem = document.getElementById('banana');

    //취득한 요소 노드의 style.color 프로퍼티 값을 변경한다.
    $elem.style.color = 'red';

    //** 인수로 전달된 id 값을 갖는 HTML 요소가 존재하지 않는 경우 null을 반환
    const $elem2 = document.getElementById('grape');
    console.log($elem2); //null

</script>
</body>
```

- HTML 요소에 id 어트리뷰트를 부여하면 id 값과 동일한 이름의 전역 변수가 암묵적으로 선언되고 해당 노드 객체가 할당되는 부수 효과가 있다.

```html
<body>
    <div id="foo"></div>
<script>

    console.log(foo === document.getElementById('foo'));
    //true

</script>
</body>

```

### 39.2.2 태그 이름을 이용한 요소 노드 취득
- `Document.prototype.getElementsByTagName`
- `Element.prototype.getElementsByTagName`
- 인수로 전달한 태그 이름을 갖는 모든 요소들을 탐색하여 반환
- Elements가 복수형인 것에서 알 수 있듯이 getElementsByTagName 메서드는 여러 개의 요소 노드 객체를 갖는 DOM 컬렉션 객체인 HTMLCollection 객체를 반환한다.
```html
<body>
    <ul>
        <li id="apple">Apple</li>
        <li id="banana">Banana</li>
        <li id="orange">Orange</li>
    </ul>

    <script>
        //태그 이름이 li인 요소 노드를 모두 탐색하여 반환한다.
        //탐색된 요소 노드들은 HTMLCollection 객체에 담겨 반환된다.
        //HTMLCollection 객체는 유사 배열 객체이면서 이터러블이다.
        const $elems = document.getElementsByTagName('li');

        //취득한 모든 요소 노드의 style.color 프로퍼티 값을 변경한다.
        //HTMLCollection 객체를 배열로 변환하여 순회하며 color 프로퍼티 값을 변경한다.
        [...$elems].forEach(elem => elem.style.color = 'red');

    </script>
</body>
```

### 39.2.3 class를 이용한 요소 노드 취득
- `Document.prototype.getElementsByClassName`
- `Element.prototype.getElementsByClassName`
- 메서드는 인수로 전달한 class 어트리뷰트 값을 갖는 모든 요소 노드들을 탐색하여 반환.
- 인수로 전달할 class 값은 공백으로 구분하여 여러 개의 class를 지정할 수 있다.
- 여러 개의 요소 노드 객체를 갖는 DOM 컬렉션 객체인 HTMLCollection 객체를 반환한다.

```html
<body>
    <ul>
        <li class="fruit apple">Apple</li>
        <li class="fruit banana">Banana</li>
        <li class="fruit orange">Orange</li>
    </ul>

    <script>
        const $elems = document.getElementsByClassName('fruit');
        // console.log($elems)
        
        [...$elems].forEach(elem => elem.style.color = 'red');
    </script>
</body>
```

### 39.2.4 CSS 선택자를 이용한 요소 노드 취득
```css
/* 전체 선택자 : 모든 요소를 선택 */
*{...}

p{...}

#foo{...}

.foo{...}

/* 어트리뷰트 선택자 : input 요소 중에 type 어트리뷰트 값이 'text'인 요소를 모두 선택 */
input[type=text]{...}

/* 후손 선택자 */
div p{...}

/* 자식 선택자 */
div > p{...}

/* 인접 형제 선택자 */
p + ul{...}

/* 일반 형제 선택자 */
p ~ ul {...}

```

- `Document.prototype.querySelector`/`Element.prototype.querySelector`
- 인수로 전달한 CSS 선택자를 만족시키는 하나의 요소 노드를 탐색하여 반환
```html
<body>
    <ul>
        <li class="fruit apple">Apple</li>
        <li class="fruit banana">Banana</li>
        <li class="fruit orange">Orange</li>
    </ul>

    <script>
        const $elem = document.querySelector('.banana');

        $elem.style.color = 'red';
    </script>
</body>
```

- `Document.prototype.querySelectorAll`/`Element.prototype.querySelectorAll`
- 인수로 전달한 CSS 선택자를 만족시키는 모든 요소 노드를 탐색하여 반환.
- 여러 개의 요소 노드 객체를 갖는 DOM 컬렉션 객체인 NodeList 객체를 반환한다.
- NodeList 객체는 유사 배열 객체이면서 이터러블이다.

