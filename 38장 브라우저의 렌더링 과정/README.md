자바스크립트가 가장 많이 사용되는 분야는 웹 브라우저 환경이다. 따라서 브라우저 환경을 고려할 때 더 효율적인 프로그래밍이 가능하다.

이를 위해 브라우저가 HTML, CSS, 자바스크립트로 작성된 문서를 어떻게 파싱하여 브라우저에 렌더링하는지 살펴보자.

- 파싱
    
    프로그래밍 언어의 문법에 맞게 작성된 텍스트 문서를 읽어 실행하기 위해 텍스트의 문자열을 토큰으로 분해 하고, 토큰에 문법적 의미와 구조를 반영하여 파스 트리를 생성하는 과정을 말한다.
    
- 렌더링
    
    HTML, CSS, 자바스크립트로 작성된 문서를 파싱하여 브라우저에 시각적으로 출력하는 것을 말한다.
    
<p align='center'>
<img width = '700px' src='https://bl3302files.storage.live.com/y4m4nETkD6IHYT9Apxr80RFYBG--hR5nMf8ZPrzt0vXjMyfklJxDbT6perQ4fUcthBOrOFpN5w_A-QHXZhIXRc37ZduuqWThEHWmEQdWt5sIT8u_WIpDwzKo-vWonAkMDwUtQb3LZGWu1cozY_g8d3Vr1yyiVF_QIqwLe-aeFo_HCqTVGEvbCR3fgVyF3apMwRh?width=1230&height=742&cropmode=none'>
</p>


브라우저는 다음과 같은 과정을 거쳐 렌더링을 수행한다.

1. HTML, CSS, 자바스크립트, 이미지, 폰트 파일 등 렌더링에 필요한 리소스를 요청하고 서버로부터 응답을 받는다.
2. 브라우저의 렌더링 엔진이 HTML과 CSS를 파싱하여 DOM과 CSSOM을 생성하고 이들을 결합하여 렌더 트리를 생성한다.
3. 자바스크립트 엔진이 자바스크립트를 파싱하여 AST(Abstract Syntax Tree)를 생성하고 바이트코드로 변환하여 실행한다. 이때 자바스크립트가 DOM API를 통해 DOM이나 CSSOM을 변경할 수 있다. 변경된 DOM과 CSSOM은 다시 렌더 트리로 결합된다.
4. 렌더 트리를 기반으로 HTML 요소의 레이아웃(위치, 크기)을 계산하고 브라우저 화면에 HTML 요소를 페인팅한다.

<br/><br/>

# 38.1 요청과 응답

브라우저의 핵심 기능은 필요한 리소스를 서버에 요청하고 서버로부터 응답 받아 브라우저에 시각적으로 렌더링하는 것이다.

즉, 렌더링에 필요한 리소스는 모두 서버에 존재하므로 요청과 응답의 과정을 거쳐 리소스를 파싱하여 렌더링한다.

서버에 요청을 전송하기 위해 브라우저는 주소창을 제공한다. 주소창에 URL을 입력하면 URL의 호스트 이름이 DNS를 통해 IP주소로 변환되고 이 IP 주소를 갖는 서버에게 요청을 전송한다.

<p align='center'>
<img width = '700px' src='https://bl3302files.storage.live.com/y4mZ-sf5XiO_lAzD9eSX_7J5d1QgFHtOngCxM0LxBxHUXMmbT3I0tOy-wuNMt0BDktW82K9Hb1D6xvfzZvQoamz-gU-dY97fXuX5DGqmEDhcU6_dSjXCwtMKn0z98QzDjHbgYlEcrP7Ww48bAe4dF6uNuYvfiRNGDz9TMmMV7_BKxsb2KCs8ABAEdIpTVQRebSS?width=2638&height=1300&cropmode=none'>
</p>


>  
> URI
> - Uniform Resource Identifier, 통합 자원 식별자의 줄임말
> - 인터넷에 있는 자원을 타나내는 유일한 주소이다.
> 
>  URL
> - Uniform Resource Locator, 통합 자원 지시자의 줄임말
> - 네트워크 상에서 각종 리소스 파일의 위치 정보를 나타냄.
> 
> URN
> - Uniform Resource Name,통합 자원 이름의 줄임말
> - 위치 정보가 아닌 이름으로 자원을 찾는다.
> - 세계적으로 유일성을 보장받는다.
> - 최종적으로는 URL로 변환되어 정보를 수집한다.
> - example: [https://urn.issn.org/urn:issn:2639-5983](https://urn.issn.org/urn:issn:2639-5983)



예를 들어, 브라우저 주소창에 [https://poiemaweb.com](https://poiemaweb.com/) 을 입력하면 루트 요청( Scheme, / , 호스트만으로 이루어진 요청)이 poiemaweb.com 서버로 전송된다. 리소스를 요청하는 내용이 없지만 암묵적으로 `index.html` 을 응답하도록 기본 설정되어 있다.

개발자 모드 네트워크 탭에서 확인해보자

<p align='center'>
<img width = '700px' src='https://bl3302files.storage.live.com/y4mZ-sf5XiO_lAzD9eSX_7J5d1QgFHtOngCxM0LxBxHUXMmbT3I0tOy-wuNMt0BDktW82K9Hb1D6xvfzZvQoamz-gU-dY97fXuX5DGqmEDhcU6_dSjXCwtMKn0z98QzDjHbgYlEcrP7Ww48bAe4dF6uNuYvfiRNGDz9TMmMV7_BKxsb2KCs8ABAEdIpTVQRebSS?width=2638&height=1300&cropmode=none'>
</p>

위 그림을 살펴보면 `index.html` 뿐만 아니라 CSS나, 이미지, 자바스크립트 파일들도 응답된 것을 확인할 수 있다. 

이는 브라우저의 렌더링 엔진이 HTML(index.html)을 파싱하는 도중에 외부 리소르를 로드하는 태그(`<link>` , `<img>` , `<script>` 태그 등을 만나면 HTML 파싱을 일시 중단하고 해당 리소스 파일을 서버로 요청하기 때문이다.

<br/><br/>

# 38.2 HTTP 1.1과 HTTP 2.0

HTTP는 웹에서 브라우저와 서버가 통신하기 위한 프로토콜이다.

HTTP/1.1 은 기본적으로 커넥션당 하나의 요청과 응답만 처리한다. 따라서 여러 개의 요청을 한 번에 전송할 수 없고 응답도 마찬가지다. 이처럼 HTTP/1.1은 리소스의 동시 전송이 불가능한 구조이므로 요청할 리소스의 개수에 비례하여 응답 시간도 증가하는 단점이 있다.

<p align='center'>
<img width = '400px' src='https://bl3302files.storage.live.com/y4m8APDe7lxj1OF1Eyy0380daxYwyZh3Kn52541qFFtjDh2zHg_rEkmsFgyDXR0mPE5u-FhAf2R88gTrZmRP4Z8sHqnSvuqBklFnysOstP2jt6jzYvH3yR_yqJL-KH_3DbNzWMr1GmizjjMas03IiozCgay4FhOFlBXZg4REL1qu9rmUvpP1dtPiKs3Y11cbSTZ?width=1060&height=892&cropmode=none'>
</p>

HTTP/2.0 에서는 다중 요청/응답이 가능하다.

<p align='center'>
<img width = '400px' src='https://bl3302files.storage.live.com/y4mEDHjjSK5f-mCgotzVWMNKmOJqx28ZgUnyYhO3MkgwEAjvKD7I22m2wV5LhOXaNdBdSUZ0Iypy0oB_FLSuKcDEFVeFPhjq-sbYZZxrcUt3isFBdJAM3mYKlUcQgtBtYfDs1fw4iuzIBf4e9UW1YVysXTEs6j3xiqIY0FhsrPX5DA0m9sk2hpyos64twTnk-AA?width=1060&height=892&cropmode=none'>
</p>

## HTTP/3

HTTP/3는 HTTP의 차기 주요 버전이며 아직 개발중이다. 하지만 현재 많은 서비스에서 시범적으로 사용하고 있다.

HTTP/3의 큰 특징은 다음과 같다.

1. TCP위에서 동작하지 않고 UDP 기반의 프로토콜인 구글에서 개발한 QUIC을 이용하여 동작한다.
    - 이는 TCP 방식의 특징인 handshake 방식의 단점을 보완했다.
2. HOL(Head-of-Line) Blocking 최소화
    - 기존 HTTP는 여러 리소스 중 하나의 리소스 파일의 패킷만 손상되어도 모든 리소스의 전송을 지연시켰다. 하지만 QUIC은 여러 개의 동시 바이트스트림이 존재하고 스트림별로 손실을 처리하기 때문에 손실 외에 데이터들은 전송된다.
3. 네트워크가 바뀌어도 연결이 유지된다. (e.g. 셀룰러 → 와이파이)

<br/><br/>

# 38.3 HTML 파싱과 DOM 생성

HTML 문서를 브라우저에 시각적인 픽셀로 렌더링하려면 HTML 문서를 브라우저가 이해할 수 있는 자료구조로 변환하여 메모리에 저장해야 한다.

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
      <li id="banana">Banana</li>
      <li id="orange">Orange</li>
    </ul>
    <script src="app.js"></script>
  </body>
</html>
```

브라우저의 렌더링 엔진은 다음 그림과 같은 과정을 통해 DOM을 생성한다.

<p align='center'>
<img width = '400px' src='https://bl3302files.storage.live.com/y4m2nPcSbi_erTNz3iN9ghbtBH9o3Idj_yxwiqQk_-jPEFbOaDOisz2sSZg_xkBxObhHa7ESimahv74DLJPJxS6zKBv2yzPtASCMfqurGmi1POKVR56yeASEqpkyY6VyniNDHA6Vq1KO5nkI6qjRgMHXqxv6zhb_PpF4XEM9itr7IzmfyV7TDbazzzEdQyjuywq?width=1094&height=894&cropmode=none'>
</p>

1. 서버에 존재하는 HTML 파일이 요청에 의해 응답된다. 이때 서버는 바이트로 응답한다.
2. 응답된 바이트 형태의 HTML 문서는 `meta` 태그의 `charset` 어트리뷰트에 의해 지정된 인코딩 방식을 기준으로 문자열로 변환된다. 참고로 인코딩 방식은 response header에 담겨 응답된다. 브라우저는 이를 확인하고 문자열로 변환한다.
3. 문자열로 변환된 HTML 문서를 읽어 문법적 의미를 갖는 최소 단위인 토큰들로 분해한다.
4. 각 토큰들을 객체로 변환하여 노드를 생성한다. 노드는 이후 DOM을 구성하는 기본 요소가 된다.
5. HTML 문서는 HTML 요소들의 집합으로 이루어지며 중첩 관계를 갖는다. 이때 HTML 요소 간에는 중첩 관계에 의해 부모/자식 관계가 형성된다. 이러한 부모/자식 관계를 반영하여 모든 노드들을 트리 자료구조로 구성한다. 이 노드들로 구성된 트리 자료 구조를 DOM이라 부른다.

즉, **DOM은 HTML 문서를 파싱한 결과물이다.**

<br/><br/>

# 38.4 CSS 파싱과 CSSOM 생성

렌더링 엔진이 DOM을 생성해 나가다가 CSS를 로드하는 `link` 태그나 `style` 태그를 만나면 DOM 생성을 일시 중단한다.

그리고 CSS를 HTML과 동일한 파싱 과정을 거치며 CSSOM을 생성한다. CSS 파싱을 완료하면 HTML 파싱이 중단된 지점부터 다시 HTML을 파싱하기 시작하여 DOM 생성을 재개한다.

<br/><br/>

# 38.5 렌더 트리 생성

DOM과 CSSOM을 생성하고 렌더링을 위해 둘은 **렌더 트리**로 결합 된다.

렌더 트리는 렌더링을 위한 트리 구조의 자료구조다. 따라서 화면에 렌더링되지 않는 노드(`meta` 태그, `script` 태그 등)와 CSS에 의해 비표시(예: `display: none` )되는 노드들은 포함하지 않는다.

이후 완성된 렌더 트리는 각 HTML 요소의 레이아웃을 계산하는 데 사용되며 브라우저 화면에 픽셀을 렌더링하는 페인팅 처리에 입력된다.

<p align='center'>
<img width = '600px' src='https://bl3302files.storage.live.com/y4mcu34Ame2i8YYBrquHTVJXcRl0G4NyJKh0_49ZP-vQ3YDysq_o-RgUKHCTyPg4MvKQnyw9bvWUQAoQp3J38y2Vk_8eUj2rSMxaXtoRjQKPnj5yzMt-0T_5zqB6OFxSUO2bWlbG2YE6SekxcVjxcSisvMoTX7sUHeSxJ01rk2MXeiYxs5vlH2mi69c77rO6Rj6?width=976&height=156&cropmode=none'>
</p>

지금 까지 살펴본 브라우저의 렌더링 과정은 반복해서 실행될 수 있다.

- 자바스크립트에 의한 노드 추가 또는 삭제
- 브라우저 창의 리사이징에 의한 뷰포트 크기 변경
- HTML 요소의 레이아웃에 변경을 발생 시키는 `width/height, margin, padding` 등의 스타일 변경

리렌더링은 비용이 많이 드는 성능에 악영향을 주는 작업으로 가급적 리렌더링이 빈번하게 발생하지 않도록 주의할 필요가 있다.

<br/><br/>

# 38.6 자바스크립트 파싱과 실행

DOM은 HTML 문서의 구조와 정보뿐만 아니라 프로그래밍 인터페이스로서 DOM API를 제공한다. 즉, 자바스크립트 코드에서 이미 생성된 DOM을 동적으로 조작할 수 있다.

렌더링 엔진이 DOM을 생성해 나가다 `script` 태그를 만나면 DOM 생성을 일시 중단한다.

그리고 자바스크립트 코드를 파싱하기 위해 자바스크립트 엔진에 제어권을 넘긴다. 자바스크립트 파싱과 실행이 종료되면 다시 렌더링 엔진으로 제어권을 넘겨 HTML 파싱이 중단된 지점부터 DOM 생성을 재개한다.

자바스크립트 엔진은 렌더링 엔진이 DOM과 CSSOM을 생성하듯이 자바스크립트를 해석하여 AST(Abstract Syntax Tree, 추상적 구문트리)를 생성한다. 그리고 AST를 기반으로 인터프리터가 실행할 수 있는 중간 코드인 바이트코드를 생성하여 실행한다.

<p align='center'>
<img width = '600px' src='https://bl3302files.storage.live.com/y4mBr82PxPwe4AcICNLPo_60d9cDBRvuycaAdICCH6PvvPSz6zDeZKRkhUfhP7qhUJDHXca6qWP9am-mZunepeFn6zybfLLaTtd3D8GMcx0wY_0lLHGYL2nEJ3QuuHc5jixEZtVHoiJuZRLKeo8ka1IV47rTDBMRn4allTcSox_vrfnGKdPKeyfEF4gTndHAMNj?width=1264&height=606&cropmode=none'>
</p>

### 토크나이징

자바스크립트 소스코드를 토큰들로 분해한다.

### 파싱

토큰들의 집합을 구문 분석하여 AST를 생성한다.

### 바이트코드 생성과 실행

AST는 인터프리터가 실행할 수 있는 바이트코드로 변환되고 인터프리터에 의해 실행된다.

<br/><br/>

# 38.7 리플로우와 리페인트

만약 자바스크립트 코드에서 DOM이나 CSSOM을 변경하는 DOM API가 사용된 경우 DOM이나 CSSOM이 변경된다. 이때 병경된 DOM과 CSSOM은 다시 렌더 트리로 결합되고 변경된 렌더 트리를 기반으로 다시 렌더링한다. 이를 리플로우, 리페인트라 한다.

리플로우는 레이아웃 계산을 다시 하는 것을 말하며, 노드 추가/삭제, 요소의 크기/위치 변경 등 레이아웃에 영향을 주는 변경이 발생한 경우에 한하여 실행된다.

리페인트는 재결합된 렌더 트리를 기반으로 다시 페인트를 하는 것을 말한다.

따라서 리플로우와 리페인트가 반드시 동시에 실행되는 것은 아니다. 레이아웃에 변화가 없으면 리페인트만 실행된다.

<br/><br/>

# 38.8 자바스크립트 파싱에 의한 HTML 파싱 중단

지금까지 살펴본 바와 같이 렌더링 엔진과 자바스크립트 엔진은 병렬이 아니라 직렬적으로 파싱을 수행한다.

이처럼 브라우저는 동기적으로, 즉 위에서 아래 방향으로 순차적으로 HTML, CSS, 자바스크립트를 파싱하고 실행한다. 이것은 `script` 태그 위치에 따라 DOM 생성이 지연될 수 있다는 것을 말한다. 따라서 `script` 태그의 위치는 중요한 의미를 갖는다.

```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="UTF-8">
    <link rel="stylesheet" href="style.css">
		<script src="app.js"></script>
  </head>
  <body>
    <ul>
      <li id="apple">Apple</li>
      <li id="banana">Banana</li>
      <li id="orange">Orange</li>
    </ul>
  </body>
</html>
```

위 예제는 `app.js` 의 파싱과 실행 이전까지는 DOM의 생성이 일시 중단된다. 이때 자바스크립트 코드에서 DOM API를 사용할 경우 이미 DOM이나 CSSOM이 생성되어 있어야 한다. 그렇지 않다면 문제가 발생할 수 있다.

```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="UTF-8">
    <link rel="stylesheet" href="style.css">
    <script>
      /*
      DOM API인 document.getElementById는 DOM에서 id가 'apple'인 HTML 요소를
      취득한다. 아래 DOM API가 실행되는 시점에는 아직 id가 'apple'인 HTML 요소를 파싱하지
      않았기 때문에 DOM에는 id가 'apple'인 HTML 요소가 포함되어 있지 않다.
      따라서 아래 코드는 정상적으로 id가 'apple'인 HTML 요소를 취득하지 못한다.
      */
      const $apple = document.getElementById('apple');

      // id가 'apple'인 HTML 요소의 css color 프로퍼티 값을 변경한다.
      // 이때 DOM에는 id가 'apple'인 HTML 요소가 포함되어 있지 않기 때문에 에러가 발생한다.
      $apple.style.color = 'red'; // TypeError: Cannot read property 'style' of null
    </script>
  </head>
  <body>
    <ul>
      <li id="apple">Apple</li>
      <li id="banana">Banana</li>
      <li id="orange">Orange</li>
    </ul>
  </body>
</html>
```

위 예제에서는 id가 apple인 요소를 생성하기 전에 접근하려하였다. 따라서 정상적으로 동작하지 않는다.

이러한 문제를 회피하기 위해 `body` 요소의 가장 아래에 자바스크립트를 위치시키는 것이 좋다. 그 이유는 다음과 같다.

- DOM이 완성되지 않은 상태에서 자바스크립트가 DOM을 조작하면 에러가 발생할 수 있다.
- 자바스크립트 로딩/파싱/실행으로 인해 렌더링에 지정받는 일이 발생하지 않아 로딩 시간이 단축된다.

<br/><br/>

# 38.9 script 태그의 async/defer 어트리뷰트

앞에서 살펴본 자바스크립트 파싱에 의한 DOM 생성이 중단되는 문제를 근본적으로 해결하기 위해 HTML5부터 `script` 태그에 `async` 와 `defer` 어트리뷰트가 추가되었다.

두 어트리뷰트는 `src` 어트리뷰트를 통해 외부 자바스크립트 파일을 로드하는 경우에만 사용할 수 있다. 즉,  인라인 자바스크립트에는 사용할 수 없다.

```html
<script async src="extern.js"></script>
<script defer src="extern.js"></script>
```

`async` 와 `defer` 어트리뷰트를 사용하면 외부 자바스크립트 파일의 로드가 비동기적으로 동시에 진행된다. 하지만 실행 시점에 차이가 있다.

### async 어트리뷰트

HTML 파싱과 외부 자바스크립트 파일의 로드가 비동기적으로 동시에 진행된다. 단, 파싱과 실행은 자바스크립트 파일이 로드가 완료된 직후 진행되며, 이때 HTML 파싱이 중단된다.

<p align='center'>
<img width = '600px' src='https://bl3302files.storage.live.com/y4mG5v-6mTGlS6ig5feD5AJMjbQuNDpvZLMuQo2XjWSlugy9RsYJhQMKpL7_a2uKQgyVrodCZX1lX-vfV0LBw1mDq0WNMt0vU4gQAxc2sTjN7hHjHXDII9qCGZS0rDLqkoRw86R-gaxM3pCdRDNgoZDFrxiJQOgH7557o6fhNYX50UR4N2gqzSpYZzCPN7MX2AU?width=1018&height=160&cropmode=none'>
</p>

여러 `script` 태그에 `async` 어트리뷰트를 지정하면 태그의 순서와는 상관없이 로드가 완료된 자바스크립트부터 먼저 실행되므로 순서가 보장되지 않는다. 따라서 순서 보장이 필요한 `script` 태그에는 `async` 어트리뷰트를 지정하지 않는다.

### defer 어트리뷰트

`defer` 어트리뷰트도 파일의 로드가 비동기적으로 동시에 진행된다. `defer` 어트리뷰트는 `async` 와 달리  HTML 파싱이 완료된 직후, 즉 DOM 생성이 완료된 직후 진행된다.
<p align='center'>
<img width = '600px' src='https://bl3302files.storage.live.com/y4mtvqhx5YBKM9hIloRRw_kOlcEuMMydd4yPn0jIf2_ZPjyYPWmsp7VAJJZZa0TzNBO61rQMRonMD4fLPCrZf_khYjqlRcDavX53IXGZEVFWHlWL3SyLVFJcefQoPfn1OwuGUQTWQMLZHEiVrd2QkGKRN4G15u6PYGKOVFyIgPG3m5Ht45EwnwkbDta7FPKokwT?width=1018&height=168&cropmode=none'>
</p>