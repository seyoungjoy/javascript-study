# 39.6 DOM 조작

DOM 조작은 새로운 노드를 생성하여 DOM에 추가하거나 기존 노드를 삭제 또는 교체하는 것을 말한다.

DOM 조작은 리플로우와 리페인트를 발생시켜 성능에 영향을 주므로 최적화를 위해 주의해서 다루어야 한다.

## 39.6.1 innerHTML

`Element.prototype.innerHTML` 프로퍼티는 요소 노드의 HTML 마크업을 취득하거나 변경한다. 

앞서 살펴본 `textContent` 프로퍼티는 텍스트만 반환하지만 `innerHTML` 프로퍼티를 참조하면 요소 노드의 컨텐츠 영역에 포함된 모든 HTML 마크업을 문자열로 반환한다.

요소 노드의 `innerHTML` 프로퍼티에 문자열을 할당하면 요소 노드의 모든 자식 노드가 제거되고 할당한 문자열에 포함되어 있는 HTML 마크업이 파싱되어 DOM에 반영된다.

[https://codepen.io/4anghyeon/pen/mdqQyXQ](https://codepen.io/4anghyeon/pen/mdqQyXQ)

이때 사용자로부터 입력받은 데이터를 그대로 `innerHTML` 프로퍼티에 할당하는 것은 **크로스 사이트 스크립팅 공격**에 취약하므로 위험하다.

HTML5는 `innerHTML` 프로퍼티로 삽입된 `script` 요소 내의 자바스크립트 코드를 실행하지 않는다. 하지만 `script` 요소 없이도 크로스 사이트 스크립팅 공격은 가능하다.

예제) [https://codepen.io/4anghyeon/pen/jOYdJoj](https://codepen.io/4anghyeon/pen/jOYdJoj)

`innerHTML` 프로퍼티의 또 다른 단점은 HTML 마크업 문자열을 할당하는 경우 요소 노드의 모든 자식 노드를 제거하고 DOM을 변경한다는 것이다.

```html
<!DOCTYPE html>
<html>
  <body>
    <ul id="fruits">
      <li class="apple">Apple</li>
    </ul>
  </body>
  <script>
    const $fruits = document.getElementById('fruits');

    // 노드 추가
    $fruits.innerHTML += '<li class="banana">Banana</li>';
  </script>
</html>
```

예를 들어 위의 예제는 `fruits` 노드에 Banana를 추가하기만 하면 되는데 `innerHTML` 프로퍼티는 모든 자식 노드를 삭제하고 새롭게 그린다. 이는 효율적이지 않다.

또 다른 단점으로 새로운 요소를 삽입할 때 삽입될 위치를 지정할 수 없다는 단점도 있다. 

따라서 `innerHTML` 프로퍼티는 복잡하지 않은 요소를 추가할 때만 유용하다.

## 39.6.2 insertAdjacentHTML 메서드

`Element.prototype.insertAdjacentHTML(position, DOMString)` 메서드는 기존 요소를 제거하지 않으면서 위치를 지정해 새로운 요소를 삽입한다.

첫 번째 인수로 전달할 수 있는 위치를 뜻하는 문자열은 ‘beforebegin’, ‘aft erbegin’, beforeend’, ‘afterend’ 4가지다.

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/c5f62d1f-30b5-49e8-8d04-08bbeddd1732/Untitled.png)

[https://codepen.io/4anghyeon/pen/XWVOQpp](https://codepen.io/4anghyeon/pen/XWVOQpp)

`insertAdjacentHTML` 메서드는 기존 요소에 영향을 주지 않고 새롭게 삽입될 요소만 추가하므로 효율적이고 빠르다.

하지만 크로스 사이트 스크립팅 공격에 취약하다는 점은 동일하다.

## 39.6.3 노드 생성과 추가

DOM은 노드를 직접 생성/삽입/삭제/치환하는 메서드도 제공한다.

### 요소 노드 생성

`Document.prototype.createElement(tagName)` 메서드는 요소 노드를 생성하여 반환한다. `tagName` 에는 태그 이름을 나타내는 문자열을 인수로 전달한다.

```jsx
const $li = document.createElement('li');
```

`createElement` 메서드는 요소 노드를 생성할 뿐 DOM에 추가하지는 않는다. 따라서 이후에 생성된 요소 노드를 DOM에 추가하는 처리가 별도로 필요하다.

또한 `createElement` 메서드로 생성한 요소 노드는 아무런 자식 노드도 가지고 있지 않다. 따라서 요소 노드의 자식인 텍스트 노드도 없는 상태다.

### 텍스트 노드 생성

`Document.prototype.createTextNode(text)` 메서드는 텍스트 노드를 생성하여 반환한다.

```jsx
const textNode = document.createTextNode('Banana');
```

`createTextNode` 메서드로 생성한 노드 또한 마찬가지로 홀로 존재하는 상태다.

### 텍스트 노드를 요소 노드의 자식 노드로 추가

`Node.prototype.appendChild(childNode)` 메서드는 인수로 전달한 노드를 `appendChild` 메서드를 호출한 노드의 마지막 자식 노드로 추가한다.

```jsx
$li.appendChild(textNode);
```

위 예제처럼 요소 노드에 자식 노드가 하나도 없는 경우에는 텍스트 노드를 생성하여 자식 노드로 추가하는 것보다 `textContent` 프로퍼티를 사용하는 편이 더욱 간편하다.

```jsx
$li.textContent = 'Banana';
```

### 요소 노드를 DOM에 추가

`Node.prototype.appendChild` 메서드를 사용하여 `#fruits` 요소 노드의 마지막 자식 요소로 추가한다.

```jsx
$fruits.appendChild($li);
```

[https://codepen.io/4anghyeon/pen/XWVOQvZ](https://codepen.io/4anghyeon/pen/XWVOQvZ)

이 과정에서 비로소 새로 생성한 요소 노드가 DOM에 추가된다. 요소 노드를 생성하여 DOM에 한 번 추가하므로 DOM은 한 번만 변경된다.  이때 리플로우와 리페인트가 실행된다.

## 39.6.4 복수의 노드 생성과 추가

```html
<!DOCTYPE html>
<html>
  <body>
    <ul id="fruits"></ul>
  </body>
  <script>
    const $fruits = document.getElementById('fruits');

    ['Apple', 'Banana', 'Orange'].forEach(text => {
      // 1. 요소 노드 생성
      const $li = document.createElement('li');

      // 2. 텍스트 노드 생성
      const textNode = document.createTextNode(text);

      // 3. 텍스트 노드를 $li 요소 노드의 자식 노드로 추가
      $li.appendChild(textNode);

      // 4. $li 요소 노드를 #fruits 요소 노드의 마지막 자식 노드로 추가
      $fruits.appendChild($li);
    });
  </script>
</html>
```

위 예제는 DOM에 3번 추가하므로 DOM이 3번 변경된다. DOM을 변경하는 것은 높은 비용이 들어 가급적 횟수를 줄이는 편이 성능에 유리하다.

이처럼 DOM을 여러 번 변경하는 문제를 회피하기 위해서는 컨테이너 요소를 미리 생성한 다음 컨테이너 요소에 자식들을 추가하고 컨테이너 요소를 `#fruits` 에 자식으로 추가하면 DOM은 한 번만 변경된다.

```html
<!DOCTYPE html>
<html>
  <body>
    <ul id="fruits"></ul>
  </body>
  <script>
    const $fruits = document.getElementById('fruits');

    // 컨테이너 요소 노드 생성
    const $container = document.createElement('div');

    ['Apple', 'Banana', 'Orange'].forEach(text => {
      // 1. 요소 노드 생성
      const $li = document.createElement('li');

      // 2. 텍스트 노드 생성
      const textNode = document.createTextNode(text);

      // 3. 텍스트 노드를 $li 요소 노드의 자식 노드로 추가
      $li.appendChild(textNode);

      // 4. $li 요소 노드를 컨테이너 요소의 마지막 자식 노드로 추가
      $container.appendChild($li);
    });

    // 5. 컨테이너 요소 노드를 #fruits 요소 노드의 마지막 자식 노드로 추가
    $fruits.appendChild($container);
  </script>
</html>
```

위 예제는 DOM을 한 번만 변경하여 성능면에서 유리하긴 하지만 불필요한 컨테이너 요소(div)가 DOM에 추가되는 부작용이 있고, 이는 바람직하지 않다.

이러한 문제는 `DocumentFragment` 노드를 통해 해결할 수 있다. `DocumentFragment` 노드는 노드 객체의 일종으로, 부모 노드가 없어 기존 DOM과 별도로 존재하는 특징이 있다.

`DocumentFragment` 노드는 위 예제의 컨테이너 요소와 같이 서브 DOM을 구성하여 기존 DOM에 추가하기 위한 용도로 사용한다.

`DocumentFragment`는 기존 DOM과 별도로 존재하므로 `DocumentFragment` 노드에 자식을 추가하여도 기존 DOM에는 어떠한 변경도 발생하지 않는다. 또한 `DocumentFragment` 노드를 DOM에 추가하면 자신은 제거되고 자신의 자식 노드만 DOM에 추가된다. 

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/6da2ad8a-cd38-465d-97af-e12c8df1cb14/Untitled.png)

`Document.prototype.createDocumentFragment` 메서드는 비어 있는 `DocumentFragment` 노드를 생성하여 반환한다. 위 예제에서 컨테이너를 `DocumentFragment` 로 바꿔주기만 하면 된다.

```html
<!DOCTYPE html>
<html>
  <body>
    <ul id="fruits"></ul>
  </body>
  <script>
    const $fruits = document.getElementById('fruits');

    // DocumentFragment 노드 생성
    const $fragment = document.createDocumentFragment();

    ['Apple', 'Banana', 'Orange'].forEach(text => {
      // 1. 요소 노드 생성
      const $li = document.createElement('li');

      // 2. 텍스트 노드 생성
      const textNode = document.createTextNode(text);

      // 3. 텍스트 노드를 $li 요소 노드의 자식 노드로 추가
      $li.appendChild(textNode);

      // 4. $li 요소 노드를 DocumentFragment 노드의 마지막 자식 노드로 추가
      $fragment.appendChild($li);
    });

    // 5. DocumentFragment 노드를 #fruits 요소 노드의 마지막 자식 노드로 추가
    $fruits.appendChild($fragment);
  </script>
</html>
```

위 예제는 DOM 변경이 발생하는 것은 한 번뿐이며 리플로우와 리페인트도 한 번만 실행된다. 따라서 여러 개의 요소 노드를 DOM에 추가하는 경우 `DocumentFragment` 노드를 사용하는 것이 더 효율적이다.

## 39.6.5 노드 삽입

### 마지막 노드로 추가

`Node.prototype.appendChid` 메서드는 인수로 전달받은 노드를 자신을 호출한 노드의 마지막 자식 노드로  DOM에 추가한다. 언제나 마지막 자식 노드로 추가한다.

### 지정한 위치에 노드 삽입

`Node.prototype.insertBefore(newNode, childNode)` 메서드는 첫 번째 인수로 전달받은 노드를 두 번째 인수로 전달받은 노드 앞에 삽입한다. 두 번째 인수로 전달받은 노드는 반드시 `insertBefore` 메서드를 호출한 노드의 자식 노드이어야 한다. 그렇지 않으면 `DOMException` 에러가 발생한다.

[https://codepen.io/4anghyeon/pen/jOYdoZa](https://codepen.io/4anghyeon/pen/jOYdoZa)

두 번째 인수로 전달받은 노드가 `null` 이면 `appendChid` 메서드처럼 동작한다.

## 39.6.6 노드 이동

DOM에 이미 존재하는 노드를  `appendChid` 또는 `insertBefore` 메서드를 사용하여 DOM에 다시 추가하면 현재 위치에서 노드를 제거하고 새로운 위치에 노드를 추가한다.

[https://codepen.io/4anghyeon/pen/PoEVvBb](https://codepen.io/4anghyeon/pen/PoEVvBb)

## 39.6.7 노드 복사

`Node.prototype.cloneNode([deep: true | false])` 메서드는 노드의 사본을 생성하여 반환한다.

매개변수 `deep`에 `true`를 인수로 전달하면 노드를 깊은 복사 하여 모든 자손 노드가 포함된 사본을 생성하고, `false`를 전달하면 얕은 복사를하여 노드 자신만의 사본을 생성한다. 얕은 복사로 생성된 노드는 텍스트 노드도 없다.

[https://codepen.io/4anghyeon/pen/bGazyzo](https://codepen.io/4anghyeon/pen/bGazyzo)

## 39.6.8 노드 교체

`Node.prototype.replaceChild(newChild, oldChild)` 메서드는 자신을 호출한 노드의 자식 노드를 다른 노드로 교체한다. 첫 번째 매개변수 `newChild`에는 교체할 새로운 노드를 인수로 전달하고, 두 번째 `oldChild` 에는 이미 존재하는 교체될 노드를 인수로 전달한다. `oldChild`는 `replaceChild` 메서드를 호출한 노드의 자식 노드이어야 한다.

[https://codepen.io/4anghyeon/pen/jOYdoRz](https://codepen.io/4anghyeon/pen/jOYdoRz)

## 39.6.9 노드 삭제

`Node.prototype.removeChild(child)` 메서드는 `child` 매개변수에 인수로 전달한 노드를 DOM에서 삭제한다. 전달한 인수는 `removeChild` 메서드를 호출한 노드의 자식 노드이어야 한다.

# 39.7 어트리뷰트

## 39.7.1 어트리뷰트 노드와 attributes 프로퍼티

HML 요소는 여러 개의 어트리뷰트를 가질 수 있다.

글로벌 어트리뷰트(id, class, style, title ... 등)와 이벤트 핸들러 어트리뷰트(onclick, onchange ... 등)는 모든 요소에서 공통적으로 사용할 수 있지만 특정 HTML 요소에 한정적으로 사용가능한 어트리뷰트도 있다.

예를 들어 type, value, checked 어트리뷰트는 input 요소에만 사용할 수 있다.

모든 어트리뷰트 노드의 참조는 유사 배열 객체이자 이터러블인 `NamedNodeMap` 객체에 담겨서 요소 노드의 `attributes` 프로퍼티에 저장된다.

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/79be723e-cd92-48ff-aa1f-78883f57ed06/Untitled.png)

`attributes` 프로퍼티는 읽기 전용 접근자 프로퍼티이며, `NameNodeMap` 객체를 반환한다.

```html
<!DOCTYPE html>
<html>
<body>
  <input id="user" type="text" value="ungmo2">
  <script>
    // 요소 노드의 attribute 프로퍼티는 요소 노드의 모든 어트리뷰트 노드의 참조가 담긴 NamedNodeMap 객체를 반환한다.
    const { attributes } = document.getElementById('user');
    console.log(attributes);
    // NamedNodeMap {0: id, 1: type, 2: value, id: id, type: type, value: value, length: 3}

    // 어트리뷰트 값 취득
    console.log(attributes.id.value); // user
    console.log(attributes.type.value); // text
    console.log(attributes.value.value); // ungmo2
  </script>
</body>
</html>
```

## 39.7.2 HTML 어트리뷰트 조작

앞에서 살펴본 바와 같이 `attributes` 프로퍼티는 읽기 전용 접근자 프로퍼티이므로 값을 변경할 수는 없다. 또한 `attributes.id.value` 와 같이 `attributes` 프로퍼티를 통해야만 하므로 불편하다.

`Element.prototype.getAttribute` , `Element.prototype.setAttribute` 메서드를 사용하면 직접 값을 취득하거나 변경할 수 있다.

```html
<!DOCTYPE html>
<html>
<body>
  <input id="user" type="text" value="ungmo2">
  <script>
    const $input = document.getElementById('user');

    // value 어트리뷰트 값을 취득
    const inputValue = $input.getAttribute('value');
    console.log(inputValue); // ungmo2

    // value 어트리뷰트 값을 변경
    $input.setAttribute('value', 'foo');
    console.log($input.getAttribute('value')); // foo
  </script>
</body>
</html>
```

특정 어트리뷰트가 존재하는지 확인하려면 `Element.prototype.hasAttribute(attributeName)`, 어트리뷰트를 삭제하려면 `Element.prototype.removeAttribute(attributeName)` 메서드를 사용한다.

## 39.7.3 HTML 어트리뷰트 VS DOM 프로퍼티

요소 노드 객체에는 HTML 어트리뷰트에 대응하는 프로퍼티(DOM 프로퍼티)가 존재한다. 이 DOM 프로퍼티들은 HTML 어트리뷰트 값을 초기값으로 가지고 있다.

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/1b6ee86c-f11f-4157-9328-4f16e1b6216c/Untitled.png)

DOM 프로퍼티는 getter와 setter 모두 존재하므로 DOM 프로퍼티는 참조와 변경이 가능하다. 이처럼 HTML 어트리뷰트는 DOM에서 중복 관리되는 것처럼 보인다. 하지만 그렇지 않다.

**HTML 어트리뷰트의 역할은 HTML 요소의 초기 상태를 지정하는 것이다. 이는 변하지 않는다. 하지만 첫 렌더링 이후 사용자가 `input` 요소에 무언가를 입력하기 시작하면 상황이 달라진다.**

**요소 노드는 상태를 가지고 있다.** 예를 들어 `input` 요소는 필드에 입력한 값을 상태로 가지고, `checkbox` 요소는 체크 여부를 상태로 가지고 있다.

사용자가 `input` 요소에 foo 라는 값을 입력한 경우 최신 상태(foo)를 관리해야 하는 것은 물론 초기 상태도 관리해야 한다. 초기 상태 값을 관리하지 않으면 처음 표시하거나 새로고침할 때 초기 상태를 표시할 수 없다.

**이처럼 요소는 2개의 상태(초기 상태, 최신 상태)를 관리해야 한다. 초기 상태는 어트리뷰트 노드가, 최신 상태는 DOM 프로퍼티가 관리한다.**

### 어트리뷰트 노드

어트리뷰트 노드가 관리하는 초기 상태 값을 취득하거나 변경하려면 `getAttribute/setAttribute` 메서드를 사용한다. HTML 요소에 지정한 어트리뷰트 값은 사용자 입력에 의해 변하지 않으므로 언제나 동일하다.

### DOM 프로퍼티

**DOM 프로퍼티는 사용자 입력에 의한 상태 변화에 반응하여 언제나 최신 상태를 유지한다.**

예를 들어 `input` 요소의 사용자 입력에 의한 상태 변화는 `value` 프로퍼티가 관리하고, `checkbox`는 `checked` 프로퍼티가 관리한다. 하지만 `id` 프로퍼티는 사용자의 입력과 아무런 관계가 없다. 따라서 입력에 의한 상태변화와 관계 없는 `id` 어트리뷰트와 프로퍼티는 동일한 값을 유지한다.

즉, 사용자 입력에 의한 상태 변화와 관계있는 DOM 프로퍼티만 최신 상태 값을 관리한다.

### HTML 어트리뷰트와 DOM 프로퍼티의 대응 관계

대부분의 HTML 어트리뷰트는 동일한 이름의 DOM 프로퍼티와 1:1로 대응한다. 단, 다음과 같이 언제나 일치하는 것은 아니다.

- id 어트리뷰트와 id 프로퍼티는 1:1 대응하며, 동일한 값으로 연동한다.
- input 요소의 value 어트리뷰트는 value 프로퍼티와 1:! 대응한다. 하지만 어트리뷰트는 초기 상태를, 프로퍼티는 최신 상태를 갖는다.
- class 어트리뷰트는 className, classList 프로퍼티와 대응한다.
- for 어트리뷰트는 htmlFor 프로퍼티와 1:1 대응한다.
- td 요소의 colspan 어트리뷰트는 대응하는 프로퍼티가 존재하지 않는다.
- textContent 프로퍼티는 대응하는 어트리뷰트가 존재하지 않는다.
- 어트리뷰트 이름은 대소문자를 구별하지 않지만 프로퍼티 키는 언제나 카멜 케이스를 따른다.

### DOM 프로퍼티 값의 타입

`getAttribute` 메서드로 취득한 어트리뷰트 값은 언제나 문자열이다. 하지만 DOM 프로퍼티로 취득한 값은 문자열이 아닐 수도 있다. 예를 들어, `checked` 어트리뷰트 값은 문자열이지만 값은 불리언 타입이다.