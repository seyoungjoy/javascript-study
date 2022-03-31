실행 컨텍스트란 코드를 실행하기 위한 환경을 의미하는 추상적 개념이다. 추상적 개념이기 때문에 실행 컨텍스트는 뭐다! 라고 설명하기 어렵지만 자바스크립트 코드의 실행과정을 살펴보며 느껴보자.

# 23.1 소스코드의 타입

ECMAScript 사양에서는 소스코드를 4가지의 타입으로 구분한다. 각 타입의 소스코드는 실행 컨텍스트를 생성한다. 소스코드를 타입으로 구분하는 이유는 타입에 따라 실행컨텍스트를 생성하는 과정과 내용이 다르기 때문이다.

## 1. 전역 코드

전역에 존재하는 소스코드를 말하며, 전역에 정의된 함수, 클래스 등의 내부 코드는 포함되지 않는다.

전역 코드는 `var` 키워드로 생성된 전역 변수와 함수 선언문으로 정의된 전역 함수를 전역 객체의 프로퍼티와 메서드로 바인딩 하기 위해 최상위 스코프인 전역 스코프를 생성해야 한다.

이를 위해 전역 코드를 평가하고 전역 실행 컨텍스트를 생성한다.

<p align='center'>
<img width = '300px' src='https://bl3302files.storage.live.com/y4mD5Mi_gKbM0D1qKa1LqzgklJrDasNbz4mht0_YUHzJp_tWPyOo36LCXE_UhB238TtZU57afHjUtEkZMY2ksvjdWTg5TbwPywl7lY3yL09ICV1nvMlfWjxBN0rn4_EvcWiFAlvUMgc4Cd7MMeUzEN9mzkc79Op_gaCaa9AL0YIfroqAolu71lp-sNjW0X6uo7R?width=658&height=128&cropmode=none'>
</p>

## 2. 함수 코드

함수 코드는 지역 변수, 매개변수, `arguments` 객체를 관리하는 지역 스코프를 생성하고, 지역 스코프를 전역 스코프와 스코프 체인으로 연결해야한다.

이를 위해 함수 코드를 평가하고 함수 실행 컨텍스트를 생성한다.

<p align='center'>
<img width = '300px' src='https://bl3302files.storage.live.com/y4m7TgWLgnGwCrp3OGKE9sGTBl7ea2aMz0jrSAwEMx9bQBZYtdg6-sbed7LaQdsPvcP7_X55ijWbX0p88VPT3zBVDz9Cf6_rDSrBdUOE6SKC0qMuMQGQ8juwBVeJ_75417FSuSsG8vqCsiRDdulRqfGKMHlt9KdEv4DwdZdXglowDI8BCXHN6BvjZXQ15h9CBdt?width=658&height=128&cropmode=none'>
</p>

## 3. eval 코드

eval 코드는 strict mode에서 자신만의 독자적인 스코프를 생성한다.

이를 위해 eval 코드를 평가하고 eval 실행 컨텍스트를 생성한다.

<p align='center'>
<img width = '300px' src='https://bl3302files.storage.live.com/y4m5hQ8mnXdQCrpLqwcyrFtmPj54aFv_HYpu0xgYeJ6rVrQUQnmfY0HmtcDww2XC8si8ApZIP6uqpNkkAfHkYSLUJvs2KULWLlZ4_O3GtzAlu6ujWNxT7SKXNmMacufe3mpSXyZ-SMG6SWTIOILKtXKS4o8no9uHT5B-Glvzw1bgD90bB_8KvKAmbtykFZvmuCM?width=658&height=128&cropmode=none'>
</p>

## 4. 모듈 코드

모듈 코드는 모듈별로 독립적인 모듈 스코프를 생성한다.

이를 위해 모듈 코드를 평가하고 모듈 실행 컨텍스트를 생성한다.

<p align='center'>
<img width = '300px' src='https://bl3302files.storage.live.com/y4ml1fhXCwWe7tZwLLeRC7NyX6tyKlVPEL8qYDAj3LDrgurMg64QEa_oj9K014ZeoA8xo1BaXN09MqWFFdQmNiFhOeNt7awqGAECtbD2GG05uypObWXMbcaJ2kbufktfLkqRcbRVgsNkUspfjLhVPZf-sRj9SbHZzlv0jhIehIsyQPZ2YeHfqxU-VdYBLzEfAKo?width=658&height=128&cropmode=none'>
</p>

# 23.2 소스코드의 평가와 실행

모든 소스코드는 실행에 앞서 평가 과정을 거치며 실행하기 위한 준비를 한다.

자바스크립트 엔진은 소스코드를 **소스코드의 평가**와 **소스코드의 실행** 과정으로 나누어 처리한다.

1. 소스코드 평가
    - 실행 컨텍스트를 생성하고 변수, 함수 등의 선언문만 먼저 실행하여 생성된 식별자를 키로 실행 컨텍스트가 관리하는 스코프에 등록한다.
2. 소스코드 실행
    - 평가 과정이 끝나면 선언문을 제외한 소스코드가 순차적으로 실행된다. 이때 소스코드 실행에 필요한 정보, 변수나 함수의 참조를 실행 컨텍스트가 관리하는 스코프에서 참조한다. 소스코드의 실행 결과는 다시 실행 컨텍스트가 관리하는 스코프에 등록된다.

예를 들어, 다음과 같은 소스코드가 실행된다고 생각해보자.

```jsx
var x;
x = 1;
```

1. 소스코드 평가
    - 변수 선언문 `var x;` 를 먼저 실행한다. 이때 식별자 `x` 는 실행 컨텍스트가 관리하는 스코프에 등록되고 `undefined` 로 초기화 된다.
        
<p align='center'>
<img width = '300px' src='https://bl3302files.storage.live.com/y4mUptbVPV3jvkNfUli_9LXCChg_8K0il5Wnk6dEaxRnr9jT7sUmM5NwP3-otyJeiwLVCbBaEK_UKzsR2j-aWCQXSdmdCb79NyQo9P0vJJ2IXEn_Mnrmhq2wj1ptMeAE61cNlrx7aSmY8zIMjt4qOWBUWSIrENSo9SbB9Sc2TyThHNhejJip-n8Nsk4OQgIK_rJ?width=462&height=146&cropmode=none'>
</p>
        
1. 소스코드 실행
    - 평가 과정이 끝나면 소스코드 실행 과정이 시작된다. 변수 선언문은 평가 과정에서 이미 실행되었고, 따라서 위의 코드에서는 할당문 `x = 1` 만 실행된다. 만약 `x` 가 선언된 변수라면 값을 할당하고 할당 결과를 실행 컨텍스트에 등록하여 관리한다.
        
<p align='center'>
<img width = '300px' src='https://bl3302files.storage.live.com/y4mZjRLg88kUgY1zJ-swB6bIwthLUJZgB1P7aZOv1UVfq06k44RFtgEWm99xQxeBkYfSkxph_8lT3o18smiSOMHD-_ec3KniU_bf81l9r2qMwyGUjQo8uaA7gCxLjtHJlGpYlrKJ4BzxonqqRprFK4jvUILgx1vM6TZS3K1fb2wFwe1xMpP-DW4Ik2je61nwHYR?width=462&height=146&cropmode=none'>
</p>
        

# 23.3 실행 컨텍스트의 역할

자바스크립트 엔진이 이 예제를 어떻게 평가하고 실행하는지 생각해보자.

```jsx
// 전역 변수 선언
const x = 1;
const y = 2;

// 함수 정의
function foo(a) {
	// 지역 변수 선언
	const x = 10;
	const y = 20;

	// 메서드 호출
	console.log(a + x + y); // 130
}

// 함수 호출
foo(100);

// 메서드 호출
console.log(x + y); // 3
```

1. 전역 코드 평가
    - 전역 코드의 변수 선언문과 함수 선언문이 실행되고, 실행 컨텍스트가 관리하는 전역 스코프에 등록된다.
2. 전역 코드 실행
    - 전역 코드 평가 과정이 끝나면 런타임이 시작되어 전역 코드가 순차적으로 실행된다. 전역 변수에 값이 할당되고, 함수가 호출된다. 함수가 호출되면 전역 코드의 실행을 일시 중단하고 코드 실행 순서를 변경하여 함수 내부로 진입한다.
3. 함수 코드 평가
    - 함수 내부의 매개변수와 지역 변수 선언문이 실행되고, 실행 컨텍스트가 관리하는 지역 스코프에 등록된다.
4. 함수 코드 실행
    - 매개변수와 지역 변수에 값이 할당되고 `console.log` 메서드가 호출된다.
    - `console.log` 메서드를 호출하기 위해 식별자 `console` 을 스코프 체인을 통해 검색한다. 이를 위해 지역 스코프는 상위 스코프인 전역 스코프와 연결되어야 한다.
    

**이처럼 코드가 실행되려면 다음과 같이 스코프, 식별자, 코드 실행 순서 등의 관리가 필요하다.**

1. 선언에 의해 생성된 모든 식별자를 스코프를 구분하여 등록하고 상태 변화를 지속적으로 관리할 수 있어야 한다.
2. 스코프는 중첩 관계에 의해 스코프 체인을 형성해야 한다. 즉, 스코프 체인을 통해 상위 스코프로 이동하며 식별자를 검색할 수 있어야 한다.
3. 현재 실행 중인 코드의 실행 순서를 변경할 수 있어야 하며 되돌아갈 수도 있어야 한다.

이 모든 것을 관리하는 것이 바로 실행 컨텍스트다. **실행 컨텍스트는 소스코드를 실행하는 데 필요한 환경을 제공하고 코드의 실행 결과를 실제로 관리하는 영역이다.**

모든 코드는 실행 컨텍스트를 통해 실행되고 관리된다.

식별자와 스코프는 실행 컨텍스트의 **렉시컬 환경**으로 관리하고 코드 실행 순서는 **실행 컨텍스트 스택**으로 관리한다.

# 23.4 실행 컨텍스트 스택

```jsx
const x = 1;

function foo() {
	const y = 2;

	function bar() {
		const z = 3;
		console.log(x + y + z);
	}
	bar();
}

foo(); // 6
```

위 예제는 전역 코드와 함수 코드로 이루어져 있다. 자바스크립트 엔진은 먼저 전역 실행 컨텍스트를 생성하고, 함수가 호출되면 함수 코드를 평가하여 함수 실행 컨텍스트를 생성한다. 이때 생성된 실행 컨텍스트는 스택 자료구조로 관리된다. 이를 **실행 컨텍스트 스택**이라고 부른다.

다음과 같이 실행 컨텍스트 스택에는 실행 컨텍스트가 추가되고 제거된다.

<p align='center'>
<img width = '600px' src='https://bl3302files.storage.live.com/y4mgY-yeMZJE6iKfdhgcX8uynrofQIxeJws4K8BQcHeSDzmoOyfG5TKVZ1_gnRq45Q2GhftTnS2eNzhS9vFaE_N9hXGWclLWTTyJYVH2n_9-49UdDJcaeCrvkZ4VXIURh9rQFYnuywXV2BmhUHr1VIGbmDCXKnrYJRS5Tlb3ogsS9nQxrOR_bDRc0sCW-7Nd_XY?width=1650&height=350&cropmode=none'>
</p>

1. 전역 코드의 평가와 실행
    - 전역 코드를 평가하여 전역 실행 컨텍스트를 생성하고 실행 컨텍스트 스택에 푸시한다.
    - 이후 전역 코드가 실행되기 시작하고 전역 변수 `x` 에 값이 할당되고 전역 함수 `foo` 가 호출된다.
2. foo 함수 코드의 평가와 실행
    - 전역 코드의 실행이 일시 중단되고 코드의 제어권이 `foo` 함수 내부로 이동한다.
    - `foo` 함수 내부의 코드를 평가하여 `foo` 함수 실행 컨텍스트를 생성하고 실행 컨텍스트 스택에 푸시한다. 이때 `foo` 함수의 지역 변수 `y` 와 중첩 함수 `bar` 가 `foo` 함수 실행 컨텍스트에 등록된다.
    - `foo` 함수 코드가 실행되기 시작하여 지역 변수에 값이 할당되고, `bar` 함수가 호출된다.
3. bar 함수 코드의 평가와 실행
    - `foo` 함수 코드의 실행이 일시 중단되고 코드의 제어권이 `bar` 함수 내부로 이동한다.
    - `bar` 함수 내부의 코드를 평가하여 `bar` 함수 실행 컨텍스트를 생성하고 실행 컨텍스트 스택에 푸시한다.
    - `bar` 함수 코드가 실행되기 시작하여 지역 변수에 값이 할당되고 `console.log` 메서드를 호출한 이후 `bar` 함수는 종료된다.
4. foo 함수 코드로 복귀
    - `bar` 함수가 종료되어 `bar` 함수 실행 컨텍스트를 실행 컨텍스트 스택에서 제거한다.
5. 전역 코드로 복귀
    - `foo` 함수가 종료되어 `foo` 함수 실행 컨텍스트를 실행 컨텍스트 스택에서 제거한다.
    - 그리고 더 이상 실행할 전역 코드가 남아 있지 않으므로 전역 실행컨텍스트도 실행 컨텍스트에서 제거한다.
    

이처럼 **실행 컨텍스트 스택은 코드의 실행 순서를 관리한다.** **실행 컨텍스트 최상위에 존재하는 실행 컨텍스트는 언제나 현재 실행 중인 코드의 실행 컨텍스트다.**

따라서 실행 컨텍스트 스택의 최상위에 존재하는 실행 컨텍스트를 **실행 중인 실행 컨텍스트**라 부른다.

# 23.5 렉시컬 환경

렉시컬 환경은 식별자와 식별자에 바인딩된 값, 그리고 상위 스코프에 대한 참조를 기록하는 자료구조로 실행 컨텍스트를 구성하는 컴포넌트다.

**렉시컬 환경은 스코프와 식별자를 관리한다.** 렉시컬 환경은 키와 값을 갖는 객체 형태의 스코프를 생성하여 식별자를 관리한다. 즉, 렉시컬 환경은 스코프를 구분하여 식별자를 등록하고 관리하는 저장소 역할을 하는 렉시컬 스코프의 실체다.

실행 컨텍스트는 `LexicalEnvironment` 컴포넌트와 `VariableEnvironment` 컴포넌트로 구성된다. 생성 초기에는 두 컴포넌트는 하나의 동일한 렉시컬 환경을 참조한다. 이 후 상황에 따라 `VariableEnvironment` 컴포넌트를 위한 새로운 렉시컬 환경을 생성하고, 이때부터 두 컴포넌트의 내용이 달라지는 경우도 있다.

<p align='center'>
<img width = '600px' src='https://bl3302files.storage.live.com/y4m-veyTwV4RTsAMbmQtMl_olzOWmqAzSYYwvQHFAhvv6cnjJpFJ13q62B98GjVrhqAJa7NctCxhq-fNcJk834vLL33XFhNqNTccxBETy-WLgW13mb8wn9eRMI-9XlF5Gxpa-box6vfo7Kd7T5CbxTaIvkJkVnV0MIZMSk6vxO7XrR-EX1y13iw1nf2hJi78LnK?width=1480&height=210&cropmode=none'>
</p>

이 책에서는 둘을 구분하지 않고 렉시컬 환경으로 통일해 간략하게 설명한다.

또한 렉시컬 환경은 다음과 같이 두 개의 컴포넌트로 구성된다.

1. 환경 레코드(Environment Record)
    - 스코프에 포함된 식별자를 등록하고 식별자에 바인딩된 값을 관리하는 저장소다. 소스코드의 타입에 따라 관리하는 내용에 차이가 있다.
2. 외부 렉시컬 환경에 대한 참조
    - 상위 스코프를 가리킨다. 이때 상위 스코프란 외부 렉시컬 환경, 즉 해당 실행 컨텍스트를 생성한 소스코드를 포함하는 상위 코드의 렉시컬 환경을 말한다. 외부 렉시컬 환경에 대한 참조를 통해 단방향 Linked List 스코프 체인을 구현한다.
    

---

# 23.6 실행 컨텍스트의 생성과 식별자 검색 과정

```jsx
var x = 1;
const y = 2;

function foo (a) {
	var x = 3;
	const y = 4;
	
	function bar(b) {
		const z = 5;
		console.log(a + b+ + x + y + z);
	}
	bar(10);
}

foo(20); // 42
```

## 23.6.1 전역 객체 생성

전역 객체는 전역 코드가 평가되기 이전에 생성된다. 전역 객체에는 빌트인 전역 프로퍼티와 전역 함수, 표준 빌트인 객체가 추가되며 동작 환경에 따라 클라이언트 사이드 Web API(DOM, Canvas, XMLHttpRequest, fetch 등..) 또는 특정 환경을 위한 호스트 객체를 포함한다.

전역 객체도 `Object.prototype` 을 상속받는다. 즉, 전역 객체도 프로토타입 체인의 일원이다.

## 23.6.2 전역 코드 평가

전역 코드 평가는 다음과 같은 순서로 진행된다.

### 1. 전역 실행 컨텍스트 생성

전역 실행 컨텍스트를 생성하고, 실행 컨텍스트 스택에 푸시한다.

<p align='center'>
<img width = '300px' src='https://bl3302files.storage.live.com/y4mnOFPx0iUrJK5TqotuIfl6ZrsCS4yX5Vou2oCwFhe8DYc_mUB2VhB-kr_bSh7ZqXrOgPNWBI8X4JvPWcWGFoMY6FA75TBg632H-Fl7mbSRtcs7lsaTTlJiFp9RkElyH7Ruz1D4zRu2EcSg9AFGXWQvEFPltGrglE4B2_w1GUMZ_bdQ48lm1X2Xn98FYatgKVm?width=636&height=400&cropmode=none'>
</p>

### 2. 전역 렉시컬 환경 생성

전역 렉시컬 환경을 생성하고 전역 실행 컨텍스트에 바인딩한다.

<p align='center'>
<img width = '500px' src='https://bl3302files.storage.live.com/y4m-Bp9xiFcaBYNU7AZRNqoIpuHlj7_TzrTixMuTUhTbMGnAsrGobweU_5_QAF5YCzuLmt7dpm_idyYNlbKa2NBNow7DkNXBfY76GklkZZSjc4QuZnowRPtByRiaXCxYKawncfIJnf8A70KyfyzqqQtOb6GbUQMrdN_CsQmkxN18VFC1Rx8Nfy8Mv_qfd_5bnOA?width=1484&height=406&cropmode=none'>
</p>

렉시컬 환경에는 2개의 컴포넌트 환경 레코드(Environment Record)와 외부 렉시컬 환경에 대한 참조(OuterLexicalEnvironmentReference)로 구성된다.

### 2.1. 전역 환경 레코드 생성

전역 환경 레코드에는 전역 변수를 관리하는 전역 스코프, 전역 객체의 빌트인 전역 프로퍼티와 빌트인 전역 함수, 표준 빌트인 객체를 제공한다.

ES6 이전에는 모든 전역 변수가 전역 객체의 프로퍼티가 됐기 때문에 전역 객체가 전역 환경 레코드의 역할을 수행했다. 하지만 `let` 과 `const` 의 등장 후 두 키워드로 선언한 전역 변수는 전역 객체의 프로퍼티가 되지 ㅇ낳고 개념적인 블록 내에 존재하게 된다.

이처럼 기존의 `var` 키워드로 선언한 전역 변수와 `let` , `const` 키워드로 선언한 전역 변수를 구분하여 관리하기 위해 **전역 환경 레코드는 객체 환경 레코드(Object Environment Record)와 선언적 환경 레코드(Declarative Environment Record)로 구성되어 있다.**   

**2.1.1. 객체 환경 레코드 생성**

전역 환경 레코드를 구성하는 컴포넌트인 객체 환경 레코드는 `BindingObject` 라고 부르는 객체와 연결된다. 이 `BindingObject` 가 23.6.1 전역 객체 생성에서 생성된 전역 객체다.

전역 코드 평가 과정에서 `var` 키워드로 선언한 전역 변수와 함수 선언문으로 정의된 전역 함수가 `BindingObject` 를 통해 전역 객체의 프로퍼티와 메서드가 된다.

그리고 이때 등록된 식별자를 환경 레코드에서 검색하면 전역 객체의 프로퍼티를 검색해 반환한다.

바로 이것이 `var` 키워드로 선언된 전역 변수와 함수 선언문으로 정의된 전역 함수가 전역 객체를 가리키는 식별자(`window`) 없이 전역 객체의 프로퍼티를 참조할 수 있는 메커니즘이다. (`window.alert → alert`)

<p align='center'>
<img width = '700px' src='https://bl3302files.storage.live.com/y4mFmzhPxUDrKH1DvOX1tFR2ODOd9LYt7no_Zu7T6Cy-WLDvJxSKU7ESd1Tbny2C6zqgDxQKSY7GhmOE_T-wjRCTc2v3FkC_rgXPtNuHTUSqt76S3Pv-5nOKY1cK4iiNJPhUf7Faed7kw66J1qvBWnwFCezrvINFujinfU_55CpHln0JgNz6uT81RFrtaX_QnNb?width=2432&height=474&cropmode=none'>
</p>

`var` 키워드로 선언한 변수는 코드 평가 단계에서 초기화까지 진행됐기 때문에 실행 단계에서 변수 선언문 이전에도 참조할 수 있다. 이것이 변수 호이스팅이 발생하는 원인이다.

함수 선언문으로 정의한 함수가 평가되면 함수 이름을 키로 `BindingObject` 를 통해 함수 객체를 즉시 할당한다. 이것이 변수 호이스팅과 함수 호이스팅의 차이다. 함수 선언문으로 정의한 함수는 함수 선언문 이전에 호출할 수 있다.

**2.1.2. 선언적 환경 레코드 생성**

`let`, `const` 키워드로 선언한 전역변수는 선언적 환경 레코드(Declarative Environment Record)에 등록되고 관리된다.

<p align='center'>
<img width = '700px' src='https://bl3302files.storage.live.com/y4mMJaC4N5Wu1EcbPm4ljcxkdQMQdXo8Fi_xZQbMxNi9cXbPpZhLgMd2JAlkqNTUOC7blXmNeAwEu_eDjJn_c7aeeR0jyud3QkWZN6eSLUgHXzWVOgpcLfMwkRtQHZ8H_f7ofnrjWpqa12Okqs_B6KLVVEP6PG26wwE2mLo1i6fWoRQ-8La_iqW2hexn2LAT40S?width=2432&height=474&cropmode=none'>
</p>

따라서 변수 `y` 는 `const` 키워드로 선언한 변수이므로 전역 객체의 프로퍼티가 되지 않기 때문에 `window.y` 로 참조할 수 없다. 또한 `const` 키워드로 선언한 변수는 “선언 단계" 와 “초기화 단계" 가 분리되어 진행한다. 따라서 실행 흐름이 변수 선언문에 도달하기 전까지 **일시적 사각지대**에 빠지게 된다.

<uninitialized> 는 초기화 단계가 진행되지 않아 변수에 접근할 수 없음을 나타내기 위해 사용한 표현이고, 실제로 <uninitialized>라는 값이 바인딩된 것이 아니다.

`let` , `const` 키워드로 선언한 변수도 변수 호이스팅이 발생하는 것은 변함 없다. 하지만 일시적 사각지대에 빠지기 때문에 참조할 수 없고, 호이스팅이 일어나지 않는것 처럼 보인다.

### 2.2 this 바인딩

`GlobalEnvironmentRecord` 의 [[GlobalThisValue]] 내부 슬롯에 `this`가 바인딩된다. 일반적으로 전역 코드에서 `this` 는 전역 객체를 가리키므로 [[GlobalThisValue]] 내부 슬롯에는 전역 객체가 바인딩 된다.

<p align='center'>
<img width = '700px' src='https://bl3302files.storage.live.com/y4m1HDniYUatLz6JQytSPRKPAjn_kcDL2pg5rQcjGKQJIHo7hkspXcNcOZTvWVRZ_7QVKPQEqe7GH8lFaioXCrjB11GO7yxFiExKsGZJcrT5xdaPexDPkjVSr9wzajhgHhPRhFB7jmsCov64X78j4WFG3Eschf1z6IGyVf6d1_EfGg9IMJ3wTWmAbHjRrI18rtY?width=2432&height=622&cropmode=none'>
</p>

참고로 this 바인딩은 객체 환경 레코드와 선언적 환경 레코드에는 없고 `GlobalEnvironmentRecord` 와 함수 환경 레코드에만 존재한다. 23.6.4 절에서 더 자세히 살펴본다.

### 2.3 외부 렉시컬 환경에 대한 참조 결정

`OuterLexicalEnvironmentReference` 는 현재 평가 중인 소스코드(지금은 전역 코드)를 포함하는 외부 소스코드의 렉시컬 환경, 즉 상위 스코프를 가리킨다. 이를 통해 단방향 Linked List인 스코프 체인을 구현한다.

전역 코드를 포함하는 소스코드는 없으므로 전역 렉시컬 환경의 `OuterLexicalEnvironmentReference` 에 `null` 이 할당된다.

<p align='center'>
<img width = '700px' src='https://bl3302files.storage.live.com/y4maBzap6mlWG-Y7MGtskJvHPiDdYZufzn68TMBZV9lBmANvpP-ANap41YCT_3ZPMF_aQh9o3lOcwMJOLLo-Z_pnL4Av12hsJcLGOLlCgCLeuxEic6R62GRJ_2L1X5kIZ9A3dLD7DiLquVLakcfVKcwGzdcgSPfS0f8h10KqD2uoB4pj2fWfrsOfbbrAg4_XcqI?width=2432&height=622&cropmode=none'>
</p>

`OuterLexicalEnvironmentReference` 를 통해 스코프 체인을 구현하는 메커니즘은 함수 코드 평가에서 더 자세히 살펴본다.

## 23.6.3 전역 코드 실행

이제 드디어 전역 코드가 순차적으로 실행된다. 변수 할당문이 실행되어 전역 변수 `x` , `y` 에 값이 할당되고 `foo` 가 호출된다.

<p align='center'>
<img width = '700px' src='https://bl3302files.storage.live.com/y4mMa5mjB0vc_peeBxzrlKTi5nDITSyykLo30XWfbvrXBeT25i8W5xamcK79W1SdLzJ7GvL5ie_vD1AtSH59m_CoPrrV0ftw7QGYUq7URN0b1HkFj_26ns3r0Vv_hixTPEjtZydKtI3T9v9Iks1bNDWmiVADlqz76W2piBVa01-NDENqE5uy5TS8-r8wE-59OXC?width=2432&height=622&cropmode=none'>
</p>

변수 할당문 또는 함수 호출문을 실행하려면 변수 또는 함수가 선언된 식별자인지 확인해야 한다. 식별자는 스코프가 다르면 같은 이름을 가질 수 있다. 즉, 동일한 이름의 식별자가 다른 스코프에 여러 개 존재할 수도 있고, 이를 식별할 필요가 있다. 이를 **식별자 결정**이라 한다.

**식별자 결정을 위해 식별자를 검색할 때는 실행 중인 실행 컨텍스트에서 식별자를 검색하기 시작한다.** 선언된 식별자는 실행 컨텍스트의 렉시컬 환경의 환경 레코드에 등록되어 있다.

현재 실행 중인 실행 컨텍스트는 전역 실행 컨텍스트이므로 전역 렉시컬 환경에서부터 식별자를 검색한다. 만약 실행 중인 실행 컨텍스트의 렉시컬 환경에서 식별자를 못찾으면 `OuterLexicalEnvironmentReference` 가 가리키는 렉시컬 환경, 상위 스코프로 이동하여 식별자를 검색한다.

이것이 바로 스코프 체인의 동작 원리다. 

전역 렉시컬 환경은 스코프 체인의 종점이고, 전역 렉시컬 환경에서 검색할 수 없는 식별자는 Reference Error를 발생시킨다.

## 23.6.4 foo 함수 코드 평가

`foo` 함수가 호출되면 전역 코드의 실행을 일시 중단하고 `foo` 함수 내부로 코드의 제어권이 이동한다. 그리고 `foo` 함수 코드를 평가하기 시작한다. 다음과 같은 순서로 진행된다.

### 1. 함수 실행 컨텍스트 생성

먼저 `foo` 함수 실행 컨텍스트를 생성한다. 생성된 함수 실행 컨텍스트는 함수 렉시컬 환경이 완성된 다음 실행 컨텍스트 스택에 푸시된다.

<p align='center'>
<img width = '700px' src='https://bl3302files.storage.live.com/y4mo6yaFq6QIsWMHBL7D_iN9XmODNShIUuhXdynyJP8ddek1nP0Hj4utNwiWhf70ijRYGsiTYpVcPIEH4UC6ZjnHbj0c2q-cj75w8v-ccSlH2Fzv2Ghr-a6Q1Om6vtgQsInoPRJfJLCKQybgNS_b-WOR131f2z1jxanX44I5rXKHWcebh1DquBrI1YtVMK30KXA?width=2432&height=826&cropmode=none'>
</p>

### 2. 함수 렉시컬 환경 생성

`foo` 함수 렉시컬 환경을 생성하고 `foo` 함수 실행 컨텍스트에 바인딩한다.


<p align='center'>
<img width = '700px' src='https://bl3302files.storage.live.com/y4mey_yEHhkRLIc8eYI8B6Re2qgHX9dy_09Dw2v7xl786T0FBiIRx47z1wD7NquHIREjL-ktERGMK5w9Bt3Yf8XxPlo0ub7o6nd5vIRQv0rFKlzWH5t0CfF2I4mqfKciFFGmmn58LL-bjRYD_KxmFRSROMGUwjmX6i1uqC6CmJIMR7OK-6pMnVlv8Hi2QNFqdZy?width=2432&height=910&cropmode=none'>
</p>

### 2.1 함수 환경 레코드 생성

함수 환경 레코드는 매개변수, `arguments` 객체, 함수 내부에서 선언한 지역 변수와 중첩 함수(예제에선 bar)를 등록하고 관리한다.

<p align='center'>
<img width = '700px' src='https://bl3302files.storage.live.com/y4m2HI0qvV04HN9q9n-VkpD1DJ9vbey273GwTxhg24SDopaN5OI6Nz-CQHX-TADYM05zbZ_WjdYu_f-9kI7bPxoLNg-B-BRYgCgsV6G1LfXTVbZJbcPOY2QD8feUz-3Kcfk3gwsl73E5pCPcICBNrVHpT818Qd24ZaMp7tIWGbXTQsg6M_jT8hU830AWREUD1Tc?width=2432&height=1068&cropmode=none'>
</p>

### 2.2 this 바인딩

함수 환경 레코드의 [[ThisValue]] 내부 슬롯에 `this` 가 바인딩된다. `foo` 함수는 일반 함수로 호출되었으므로 `this` 는 전역 객체를 가리킨다. 따라서 [[ThisValue]] 내부 슬롯에는 전역 객체가 바인딩 된다.

<p align='center'>
<img width = '700px' src='https://bl3302files.storage.live.com/y4mg4roZrlabrPv2wm-6iJ2E7XTL2xp3e9ZQFJVgE7GoIahraFay7ldKt0Cxi_yIbiRTGaKa1_0FmZkVbhIZ9gaM2IzckOqiRUrky1uDAslk712fwaJDLZuxWomEk4mQHThFqe7c9geKhwbZfHf_1i15joQY3-UH8LngHql0L7J05UMtkt83hcT0wsaUf-2sDUR?width=2432&height=1202&cropmode=none'>
</p>

### 2.3 외부 렉시컬 환경에 대한 참조 결정

외부 렉시컬 환경에 대한 참조에 `foo` 함수 정의가 평가된 시점에 실행 중인 실행 컨텍스트(예제에선 전역 실행 컨텍스트)의 렉시컬 환경의 참조가 할당된다.

<p align='center'>
<img width = '700px' src='https://bl3302files.storage.live.com/y4mwJkIDUwzXl0p44UjH7mwYMMXPBvQVqtgW4vsIe_Z8tNGj0_uxA0IJ9th1NI5qXpxKHNUciE1ZDN09OCPT6mNv8kU3eX6_Kqr2mODEMpZnLvCWAn-HPKl9_XaMZ2B5InN9R1F-doZb30oYZyn6fe6P-ifd_7z2EBM3_Q4HEEZxC-6dN0286xqSsGioIpHwY2s?width=2432&height=1202&cropmode=none'>
</p>


자바스크립트는 렉시컬 스코프(정적 스코프)를 따르기 때문에 **함수를 어디서 호출했는지가 아니라 어디에 정의했는지에 따라 상위 스코프를 결정한다** 

따라서 자바스크립트 엔진은 함수 정의를 **평가**할 때 함수의 상위 스코프를 함수 객체의 내부 슬롯 [[Environment]]에 저장한다. 즉, 함수 객체의 내부 슬롯 [[Environment]]가 바로 렉시컬 스코프를 구현하는 메커니즘이다.

또한 함수 환경 렉시컬 환경이 모두 완성되었기 때문에 `foo` 함수 실행 컨텍스트가 실행 컨텍스트 스택에 등록된다.

렉시컬 스코프는 클로저를 이해할 수 있는 중요한 단서다.

## 23.6.5 foo 함수 코드 실행

`foo` 함수 코드 평가가 끝나고 소스코드가 순차적으로 실행되기 시작한다. 매개변수 `a` 에 인수가 할당되고, 지역 변수에 `x`, `y`에 값이 할당된다. 그리고 `bar` 함수가 호출된다.

이때 **식별자 결정을 위해 실행 중인 실행 컨텍스트(foo 실행 컨텍스트)의 렉시컬 환경에서 식별자를 검색하기 시작한다.** 현재 실행중인 실행 컨텍스트의 렉시컬 환경에서 식별자를 검색할 수 없으면`OuterLexicalEnvironmentReference` 가 가리키는 렉시컬 환경, 상위 스코프로 이동하여 식별자를 검색한다.

<p align='center'>
<img width = '700px' src='https://bl3302files.storage.live.com/y4mA9UMSeE1vAB1HNOm0mQhq4UVhjqxmr8r6HcBXvElBh3yTRZZyxrj_G1xbgx_WjKXZGC5vGZtWhFSid4Uh8a1v_frGbqAsCjpKPMm_nYdtqBC2OyRgiQn2U0BlVT0Nutv0DwDwBTWyrR5L3NYfaAgyZXwgRkGyZ3i7M2LQersJUbbBEDF7gEYveelhBL77_Vg?width=2432&height=1202&cropmode=none'>
</p>


## 23.6.6 bar 함수 코드 평가

`bar` 함수가 호출되면 `bar` 함수 내부로 코드의 제어권이 이동한다. 그리고 `bar` 함수 코드를 평가하기 시작한다.

과정은 `foo` 함수 코드 평가와 같다.

<p align='center'>
<img width = '700px' src='https://bl3302files.storage.live.com/y4maJXr78P59vaOIJiZD7a-9a6ZQcDJkS55VflceiDh5EriHJTr162aSXKnWeCzjVwnC8WMTLwLs-3a-we1AvBVdIrVHjlp4XMrXT1qVJgZrOkYnE5L2dmW0druAI-hGbpmM8dkXtfIuqXPaYnso-ls7dInE9J1uUxJB2a382yqVdUGGUMbtR1uomMj7fRQRFc1?width=2432&height=1656&cropmode=none'>
</p>

## 23.6.7 bar 함수 코드 실행

`bar` 함수의 소스코드가 실행되며 매개변수와 지역변수에 값이 할당된다.

<p align='center'>
<img width = '700px' src='https://bl3302files.storage.live.com/y4mlGhHkVJZDBrc7H8cQyJSAjE-jx0Qjs2BNK9xY7yjx5WI6SPjyY3cer78fstT7irT_PkVGDNNqpaSObriplcNoB9s3pm6MAyiAPCeQe1NXqq5dSg7Zoo4oBYs3ZFN-8mlYmdZaDWrqHPbnipHJhJ7JRd7P8ARgZkXqz-x27LhGLo3T9sJfM8U6O9n4DWMNmt3?width=2432&height=1656&cropmode=none'>
</p>

그리고 `console.log(a + b + x + y + z);` 가 실행된다.

### 1. console 식별자 검색

먼저 `console` 식별자를 스코프 체인에서 검색한다. 현재 실행중인 실행 컨텍스트(`bar` 함수 실행 컨텍스트)의 렉시컬 환경에 `console` 이 없으므로 상위 스코프로 타고 타고 올라가서 전역 렉시컬 환경에서 `console` 식별자를 검색한다. `console` 식별자는 객체 환경 레코드의 `BindingObject` 를 통해 전역 객체에서 찾을 수 있다.

### 2. log 메서드 검색

`console` 객체에서 `log` 메서드를 검색한다. 이때는 `console` 객체의 프로토타입 체인을 통해 메서드를 검색한다.

### 3. 표현식 a + b + x + y + z의 평가

각 식별자를 스코프체인을 통해 `bar` 렉시컬 환경에서부터 foo 렉시컬 환경까지 검색하여 참조한다.

## 23.6.8 bar 함수 코드 실행 종료

`console.log` 메서드가 호출되고 종료하면 `bar` 함수에서 더 이상 실행할 코드가 없으므로 `bar` 함수의 실행이 종료된다. 이때 실행 컨텍스트에서 `bar` 함수 실행 컨텍스트가 제거되고, `foo` 실행 컨텍스트가 다시 실행 중인 실행 컨텍스트가 된다.

<p align='center'>
<img width = '700px' src='https://bl3302files.storage.live.com/y4mnMFRcJ7CrU3TwAS5NU_vi5Gm4oqsR2WWpocBx6KqT4V0kHsqUg14KWBbNJOYwhuhy8p92mpF53mS8K77GKTgL4k_py-mG0gcYFkZePPrKLFR8WF7Z07k0fulEjfoDnsQPqkT2dbijLaNBpHZVlGHwJpKbz3zUsCPQAQkh1y-pxFaVqfRGxdUhAipD0r2P0yr?width=2432&height=1656&cropmode=none'>
</p>


실행 컨텍스트 스택에서 `bar` 함수 실행컨텍스트가 제거되었다고 해서 `bar` 함수 렉시컬 환경까지 즉시 소멸하는 것은 아니다. 렉시컬 환경은 독립적인 객체이고 객체를 포함한 모든 값은 누군가에 의해 참조되지 않을 때 가비지 컬렉터에 의해 메모리가 해제된다.

## 23.6.9 foo 함수 코드 종료

`bar` 함수가 종료하면 더 이상 실행할 코드가 없으므로 `foo` 함수 코드의 실행이 종료된다. 실행 컨텍스트 스택에서 `foo` 함수 실행 컨텍스트가 제거된다.

<p align='center'>
<img width = '700px' src='https://bl3302files.storage.live.com/y4mlx-Vy3p7cE6jLFLiMV94luV0HnLT-FR7CbT5JceJrWIK8SXpWH9DsM7vTPH-Kj0a8gNHQalccHInLd1z7Iu1YQDKSYQaRsqRn736B_2Mw2R3C8ztJa5X-RephR2AypDZapgZLLcKR3LejJzke3OJI2TM23P-fXfZh2_m6RpleJrqznZ40JCWp5Vt0GVrB3iv?width=2432&height=1656&cropmode=none'>
</p>

## 23.6.10 전역 코드 실행 종료

`foo` 함수가 종료되면 더 이상 실행할 코드가 없으므로 전역 실행 컨텍스트도 제거되어 실행 컨텍스트 스택에는 아무것도 남지 않게 된다.

# 23.7 실행 컨텍스트와 블록 레벨 스코프

`var` 키워드로 선언한 변수는 함수 레벨 스코프만 따르고, `let` , `const` 키워드로 선언한 변수는 모든 코드 블럭을 지역 스코프로 인정하는 블록 레벨 스코프를 따른다.

```jsx
let x = 1;

if (true) {
	let x = 10;
	console.log(x); // 10
}
console.log(1);
```

`if` 문의 코드 블록이 실행되면 `if` 문의 코드 블록을 위한 블록 레벨 스코프를 생성해야 한다. 이를 위해 선언적 환경 레코드를 갖는 렉시컬 환경을 새롭게 생성하여 기존의 전역 렉시컬 환경을 교체한다. 새롭게 생성된 `if` 문의 렉시컬 환경의 `OuterLexicalEnvironmentReference` 는 전역 렉시컬 환경을 가리킨다.

<p align='center'>
<img width = '700px' src='https://bl3302files.storage.live.com/y4mpoK97UvuPHgZHgMQEVCSsr_kFWxHiOG3v2n82BtYwTV3_RAsLNiXrW7lVj9gOZgyPU8xgtnyGFRExgkwxIeN8lg-eFyNsf1M9RSlfr_umQJX-7AHPG59sthIDOCHWwsI7TR-YeDmtWsQjh2a3Wqt0_V8Qrv2Et3L6H4QFWZlVmgaZbwfY3HIzEVjFw61OEA2?width=2432&height=1202&cropmode=none'>
</p>

`if` 문이 끝나면 이전의 렉시컬 환경으로 되돌린다.

<p align='center'>
<img width = '700px' src='https://bl3302files.storage.live.com/y4mS2zDoA_pwPf_zqMRR9jG5ONUdHUYSHXXX0ZM2-0EThCOVXcb6pzJy5syxBEDVD4WFNDrcB19Oo-iq6HoBi9cojMlvooxhGWupX31OOE2i9GLIDR6ejMQ5KKx-oaQnHlMjqUev31pfdTb_1irx7_jvhGa5RoxlwLFPHYQBpeEORnTG9a_tEqnP44Z325xPDCf?width=2432&height=1202&cropmode=none'>
</p>

이는 `if` 문뿐 아니라 모든 블록 레벨 스코프에 적용된다.