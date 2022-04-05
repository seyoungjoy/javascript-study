클로저는 난해하기로 유명한 자바스크립트의 개념 중 하나이다. 그러나 클로저는 자바스크립트 고유의 개념이 아니다. 할수를 일급 객체로 취급하는 함수형 프로그래밍 언어(하스켈, 리스프, 스칼라 등)에서 사용하는 중요한 특성이다.

클로저는 자바스크립트 고유의 특성이 아니라 ECMAScript 에서는 등장하지 않는다.

MDN에서는 클로저에 대해 다음과 같이 정의하고 있다.

> “A closure is the combination of a function and the lexical environment within which that function was declared”
클로저는 함수와 그 함수가 선언된 렉시컬 환경과의 조합이다.
> 

위 정의에서 먼저 이해해야할 핵심 키워드는 “함수가 선언된 렉시컬 환경" 이다.

```jsx
const x = 1;

function outerFunc() {
	const x = 10;

	function innerFunc() {
		console.log(x); // 10
	}
	innerFunc();
}

outerFunc();
```

만약 `innerFunc` 함수가 `outerFunc` 함수의 내부에서 정의된 중첩 함수가 아니면 `innerFunc` 함수를 `outerFunc` 함수 내부에서 호출하더라도 `outerFunc` 의 지역변수에 접근할 수 없다.

```jsx
const x = 1;

function outerFunc() {
	const x = 10;

	innerFunc();
}

function innerFunc() {
	console.log(x); // 10
}

outerFunc();
```

이는 자바스크립트가 렉시컬 스코프(정적 스코프)를 따르기 때문이다.

<br/>

# 24.1 렉시컬 스코프

💭 복습: **자바스크립트 엔진은 함수를 어디서 호출했는지가 아니라 함수를 어디서 정의했는지에 따라 상위 스코프를 결정하는 렉시컬 스코프(정적 스코프)를 따른다.**

함수의 상위 스코프를 결정한다는 것은 “렉시컬 환경의 외부 렉시컬 환경에 대한 참조(OuterLexicalEnvironmentReference)에 저장할 참조값을 결정한다.” 와 같다. 

**렉시컬 환경의 “외부 렉시컬 환경에 대한 참조" 에 저장할 참조값, 즉 상위 스코프에 대한 참조는 함수 정의가 평가되는 시점에 정의된 위치에 의해 결정된다. 이것이 바로 렉시컬 스코프다.**

<br/>

# 24.2  함수 객체의 내부 슬롯 [[Environment]]

함수가 정의된 환경과 호출되는 환경은 다를 수 있다. 따라서 렉시컬 스코프가 가능하려면 함수는 자신의 상위 스코프를 기억하고 있어야 한다.

이를 위해 함수는 **자신의 내부 슬롯 [[Environment]]에 자신이 정의된 환경, 즉 상위 스코프의 참조를 저장한다.**

함수 정의가 되어 함수 객체를 생성하는 시점은 함수가 정의된 환경, 즉 상위 함수가 평가 또는 실행되고 있는 시점이고, 이때 현재 실행 중인 실행 컨텍스트는 상위 함수의 실행 컨텍스트이고, [[Enviroment]] 에 저장되는 것은 현재 실행중인 실행 컨텍스트의 렉시컬 환경의 참조값이기 때문이다.

이때 함수 렉시컬 환경의 구성 요소인 **외부 렉시컬 환경에 대한 참조 에는 [[Environment]] 에 저장된 렉싴컬 환경의 참조가 할당된다.**

따라서 함수는 내부 슬롯 [[Environment]] 에 저장한 렉시컬 환경의 참조, 즉 상위 스코프를 자신이 존재하는 한 기억한다.

<br/>

# 24.3 클로저와 렉시컬 환경

```jsx
const x = 1;

// ①
function outer() {
	const x = 10;
	const inner = function () { console.log(x) }; // ②
	return inner;
}

const innerFunc = outer(); // ③
innerFunc(); // ④ 10
```

`outer` 함수를 호출(③) 하면 `outer` 함수는 중첩 함수 `inner` 를 반환하고 생명 주기를 마감한다. 즉, `outer` 함수의 실행 컨텍스트는 실행 컨텍스트 스택에서 제거된다. 따라서 `outer` 함수의 지역 변수 `x` 도 생명 주기를 마감하여 이 `x` 에 접근하는 방법은 달리 없어 보인다.

그러나 ④ 코드의 실행 결과는 `outer` 함수의 지역 변수 `x` 의 값인 10이다. `outer` 함수의 생명주기는 종료되어 실행 컨텍스트 스택에서도 제거 되었는데 살아있는 것처럼 동작하고 있다.

이처럼 **외부 함수보다 중첩 함수가 더 오래 유지되는 경우 중첩 함수는 이미 생명 주기가 종료한 외부 함수의 변수를 참조할 수 있다. 이러한 함수를 클로저(Closure) 라고 부른다.**

다시 MDN이 설명한 클로저의 정의로 돌아가보면

> “A closure is the combination of a function and the lexical environment within which that function was declared”
클로저는 함수와 그 함수가 선언된 렉시컬 환경과의 조합이다.
> 

그 함수가 선언된 렉시컬 환경이란 함수가 정의된 위치의 상위 스코프를 의미하는 실행 컨텍스트의 렉시컬 환경을 말한다.

자바스크립트의 모든 함수는 자신의 상위 스코프를 기억하고, 모든 함수가 기억하는 상위 스코프는 어디서 호출하든 상관없이 유지된다. 따라서 함수를 어디서 호출하든 함수 자신이 기억하는 상위 스코프의 식별자를 참조하고, 그 값을 변경할 수도 있다.

위 예제는 다음 그림과 같다.

<p align='center'>
<img width = '700px' src='https://bl3302files.storage.live.com/y4mKOAtfuyWQQ1q4ZEN7rYNHthCAPtlZnnQlR-wKUU9ooS7AuQet7HgJ2AEKv_VdE-VUWBJqi3uyBT3urQvpdZzbfbRj4RvOkc-szTykFic-TEvPF6bdZDJ04RwKBk53AxqVEoze5G7ttEWCDphU2hvKV1HsvWc-Fe-IDMb-5FhdTk7zIV_EM13a2gYNInSQhf8?width=2892&height=406&cropmode=none'>
</p>

`outer` 함수가 평가되어 함수 객체를 생성할 때(①) 현재 실행 중인 실행 컨텍스트의 렉시컬 환경, 전역 렉시컬 환경을 `outer` 함수 객체의 [[Environment]] 내부 슬롯에 저장한다.

`outer` 함수를 호출하면 `outer` 함수의 렉시컬 환경이 생성되고, `outer` 함수 객체의 [[Environment]] 내부 슬롯에 저장된 전역 렉시컬 환경을 `outer` 함수 렉시컬 환경의 `OuterLexicalEnvironmentReference` 에 할당한다.

그리고 `inner` 함수는 자신의 [[Environment]] 내부 슬롯에 현재 실행 중인 실행 컨텍스트, 즉 `outer` 함수의 렉시컬 환경을 상위 스코프로서 저장한다.


<p align='center'>
<img width = '700px' src='https://bl3302files.storage.live.com/y4md-rVoeTdPC1crOFpHmTuMmvXoSxZ16QbAHjv27DEJQ5YFIEmHR6RfygpL3I8MeeU3IKCR0Ie-LgA0XKdbEyGLybBGw7K1qu29WV2XRLTS7ADcBB5uZioIH6T3mikQc8Lmu8s5kfcPjbhGKmovaDaVDR0nf42EMIMplbpAmmhS77TxRU2yZigejSV4akL7uAa?width=2890&height=806&cropmode=none'>
</p>


<br/>


`outer` 함수의 실행이 종료하면 `inner` 함수를 반환하면서 `outer` 함수의 생명주기가 종료된다(③). 이때, `outer` 함수의 실행 컨텍스는 실행 컨텍스트 스택에서 제거되지만 `outer` 함수의 렉시컬 환경까지 소멸하는 것은 아니다.

 `outer` 함수의 렉시컬 환경은 `inner` 함수의 [[Environment]] 내부 슬롯에 의해 참조되고 있고, `inner` 함수는 젼역 변수 `innerFunc` 에 의해 참조되고 있으므로 가비지 컬렉터가 메모리를 해제하지 않는다.

<p align='center'>
<img width = '700px' src='https://bl3302files.storage.live.com/y4m8gkGMvl559PFXydCMDGt9bp_DcCiihD9lu5Ms9rSvXiZUKRNC8UyHgkJ13gn6jmATkvMsQAk7HSkY4mgJc3jPsX0p56BiIEjqVz19Ex-0YY63ZBxgh2f5MnozLwgaUQ3fJ0vzA6s7i4mo5-Wsw7QQ27IPeP3nF0OMbeh5qGnq1Jp-ISo7eLT8Yuf4oVlWI90?width=2988&height=800&cropmode=none'>
</p>

<br/>


`outer` 함수가 반환한 `inner` 함수를 다시 호출하면(④) `inner` 함수의 실행 컨텍스트가 생성되고 실행 컨텍스트 스택에 푸시된다. 그리고 `OuterLexicalEnvironmentReference` 에는 `inner` 함수 객체의 [[Environment]] 내부 슬롯에 저장되어 있는 참조값이 할당된다.

<p align='center'>
<img width = '700px' src='https://bl3302files.storage.live.com/y4mQeh9qBkj-7cP7IDyznk2hA-X09HQNlc2HJzMAWAacHaR0_boZLTc668xxXiZ9f2X_-EcrqC2-ECszL6kzE58ptwV4SzuZao9otLgDhVNf5GzKc0RUMFuOQwaKzP-_S-2bXzWRn6WBld0nRg6o69IEYquxNhYL3pcda_AIDLjZFj9ms7EYqO8uw3P6CFEob9x?width=2988&height=1268&cropmode=none'>
</p>

중첩 함수 `inner` 는 외부 함수 `outer` 보다 더 오래 생존했다. 이때 더 오래 생존한 중첩 함수는 외부 함수의 생존 여부(실행 컨텍스트의 생존 여부)와 상관없이 자신이 정의된 위치에 의해 결정된 상위 스코프를 기억한다. 따라서 `inner` 으 내부에서 상위 스코프인 `outer` 의 렉시컬 환경을 참조할 수 있고, 식별자의 값도 변경할 수 있다.

자바스크립트의 모든 함수는 상위 스코프를 기억하므로 이론적으로 모든 함수는 클로저다.  하지만 모든 일반적으로 모든 함수를 클로저라고 하지는 않는다.

다음 코드를 디버깅 모드에서 실행해보자.

```html
<!DOCTYPE html>
<html>
<body>
  <script>
    function foo() {
      const x = 1;
      const y = 2;

      // 일반적으로 클로저라고 하지 않는다.
      function bar() {
        const z = 3;

        debugger;
        // 상위 스코프의 식별자를 참조하지 않는다.
        console.log(z);
      }

      return bar;
    }

    const bar = foo();
    bar();
  </script>
</body>
</html>
```

<p align='center'>
<img width = '700px' src='https://bl3302files.storage.live.com/y4mrPqHEaiXKmWTpY8EsOm5Xxe6gMSdYw3KoGd9LyeQQGz2vjSJQ_ZZSuzEYQ4oS0vNY7Qv7jdBi2lA7SpLA-Mt3AkvAIT5A72-3FhKFUu3L0plnOUVDwPYvtUwJltyu2-jG_0CpgacwWhKhk9bga8vmmMMNSgIv58L4ZVEWmPPN6-bV9iyqJZNW0V1IZ-M5Tto?width=1242&height=454&cropmode=none'>
</p>

`bar` 는 `foo` 보다 더 오래 유지되지만 상위 스코프의 어떤 식별자도 참조하지 않는다. 이처럼 상위 스코프의 어떤 식별자도 참조하지 않는 경우 모던 브라우저는 최적화를 위해 상위 스코프를 기억하지 않는다. 따라서 `bar` 는 클로저라고 할 수 없다.

또 다른 예제를 디버깅 모드에서 실행해보자.

```html
<!DOCTYPE html>
<html>
    <body>
        <script>
            function foo() {
                const x = 1;

                // 일반적으로 클로저라고 하지 않는다.
                // bar 함수는 클로저였지만 곧바로 소멸한다.
                function bar() {
                    debugger;
                    // 상위 스코프의 식별자를 참조한다.
                    console.log(x);
                }
                bar();
            }

            foo();
        </script>
    </body>
</html>
```

<p align='center'>
<img width = '700px' src='https://bl3302files.storage.live.com/y4mG-WuxMhZ_O2keDr0gnChd-AEDFCFcw0YiJWhNeej6bZPIn-SHWB9LYw7xHssJmzGRdGoRLMdIu_Hj-jBkkrmH5UsWYes5wQ7yMzuvNclA9kkyBot17Vz8enJb0lqeIYZ36J5hbY94X2WhTb7TweUKG1t_Qw_Xh7-ahP48DG9J4sZ6e-re_oeOMq4HpQLsLHZ?width=1242&height=448&cropmode=none'>
</p>

위 예제는 중첩 함수 `bar` 가 상위 스코프의 식별자를 참조하고 있으므로 클로저다. 하지만 외부로 `bar` 함수가 반환되지 않으므로 외부 함수보다 일찍 소멸하기 때문에 클로저의 본질에 부합하지 않는다.

또 다른 예제를 디버깅 모드로 실행해보자.

```html
<!DOCTYPE html>
<html>
    <body>
        <script>
            function foo() {
                const x = 1;
                const y = 2;

                // 클로저
                // 중첩 함수 bar는 외부 함수보다 더 오래 유지되며 상위 스코프의 식별자를 참조한다.
                function bar() {
                    debugger;
                    console.log(x);
                }
                return bar;
            }

            const bar = foo();
            bar();
        </script>
    </body>
</html>
```

<p align='center'>
<img width = '700px' src='https://bl3302files.storage.live.com/y4mAEC2Us6onXiXwh6lmPg81iJ0jH3CcPc3NdQW5oxOToNEtDmJw-kCaoQog8O9161Tvgq4FaoLcoCmt-jOQH3DQfdlemkZSmTEkm3rxGDoaWzqNRm0xtcAG0jOQddzC46PWyYlPKeGuCPWAQxODAq3aukdGqkc7KDA8VnsqeAFQH1t6jTuz8wcZeFgA927lNnG?width=1232&height=464&cropmode=none'>
</p>

위 예제의 중첩 함수 `bar` 는 상위 스코프의 식별자를 참조하고 있으므로 클로저다. 그리고 외부 함수의 외부로 반환되어 외부 함수보다 더 오래 살아남는다.

이처럼 **클로저는 중첩 함수가 상위 스코프의 식별자를 참조하고 있고 중첩 함수가 외부 함수보다 더 오래 유지되는 경우에 한정하는 것이 일반적이다.**

위 예제에서 `bar` 는 상위 스코프의 식별자 중 `x` 만 참조하고 있다. 이런 경우 대부분의 모던 브라우저는 최적화를 통해 참조하고 있는 식별자만 기억한다.

클로저에 의해 참조되는 상위 스코프의 변수를 **자유 변수**라고 부른다. 클로저란 “함수가 자유 변수에 닫혀있다" 라는 의미고, 이를 쉽게 해석하면 “자유 변수에 묶여있는 함수" 라고 할 수 있다.

<br/>

# 24.4 클로저의 활용

**클로저는 상태(state)를 안전하게 변경하고 유지하기 위해 사용한다.** 다시 말해, 상태가 의도치 않게 변경되지 않도록 **상태를 안전하게 은닉하고 특정 함수에게만 상태 변경을 허용**하기 위해 사용한다.

다음 예제는 호출된 횟수(`num`)이 안전하게 변경하고 유지해야 할 상태다.

```jsx
let num = 0; // 카운트 상태

const increase = function () {
	return ++num;
};

console.log(increase()); // 1
console.log(increase()); // 2
console.log(increase()); // 3
```

위 코드는 잘 동작하지만 오류를 발생시킬 가능성을 내포하고 있다. 위 예제가 바르게 동작하려면 다음과 같은 전제 조건이 지켜져야 한다.

1. `num` 은 `increase` 함수가 호출되기 전까지 변경되지 않고 유지되어야 한다.
2. 이를 위해 `num`은  `increase`만이 변경할 수 있어야 한다.

하지만 현재 `num`은 전역 변수를 통해 관리되고 있기 때문에 언제든지 누구나 접근할 수 있고 변경할 수 있다. 따라서 카운트 상태를 안전하게 변경하고 유지하기 위해서는 `increase` 함수만이 `num` 변수를 참조하고 변경할 수 있게 하는 것이 바람직하다.

```jsx
const increase = function () {
	let num = 0; // 카운트 상태
	
	return ++num;
}

// 이전 상태를 유지하지 못한다.
console.log(increase()); // 1
console.log(increase()); // 1
console.log(increase()); // 1
```

카운트 상태를 안전하게 변경하기 위해 전역 변수 `num`을 `increase` 함수의 지역 변수로 변경하여 의도치 않은 상태 변경은 방지 했다. 이제 `num` 변수의 상태는 `increase` 만이 변경할 수 있다.

하지만 `increase` 상태가 호출될 때마다 지역 변수가 초기화 되므로 이전 상태를 유지하지 못한다. 이전 상태를 유지할 수 있도록 클로저를 사용해보자.

```jsx
const increase = (function () {
	let num = 0;
	
	return function () {
		return ++num;
	};
}());

console.log(increase()); // 1
console.log(increase()); // 2
console.log(increase()); // 3
```

위 코드가 실행되면 즉시 실행 함수가 호출되어 반환된 함수가 `increase` 변수에 할당된다. `increase` 변수에 할당된 함수는 자신이 정의된 위치에 의해 결정된 상위 스코프(즉시 실행 함수)의 렉시컬 환경을 기억하는 클로저다.

즉시 실행 함수는 호출 이후 즉시 소멸되지만 반환된 클로저는 `increase` 변수에 할당되어 호출된다. 이 클로저가 상위 스코프의 렉시컬 환경을 기억하고 있기 때문에 자유 변수 `num`을 언제 어디서 호출하든지 참조하고 변경할 수 있다.

**이처럼 클로저는 상태가 의도치 않게 변경되지 않도록 안전하게 은닉하고 특정 함수에게만 상태 변경을 허용하여 상태를 안전하게 변경하고 유지하기 위해 사용한다.**

앞선 예제를 카운터 상태를 감소시킬 수도 있도록 발전 시켜보자.

```jsx
const counter = (function () {
	let num = 0;
	
	return {
		increase() {
			return ++num;
		},
		decrease() {
			return num > 0 ? --num : 0;
		}
	};
}());

console.log(counter.increase()); // 1
console.log(counter.increase()); // 2
console.log(counter.decrease()); // 1
console.log(counter.decrease()); // 0
```

이때 객체 리터럴의 중괄호는 코드 블록이 아니므로 별도의 스코프를 생성하지 않는다.

위 예쩨의 `increase` , `decrease` , 메서드의 상위 스코프는 두 메서드가 평가되는 시점에 실행 중인 실행 컨텍스트인 즉시 실행 함수 실행 컨텍스트의 렉시컬 환경이다.

위 예제를 생성자 함수로 표현하면 다음과 같다.

```jsx
const Counter = (function () {
	let num = 0;

	// 생성자 함수
	function Counter() {
		// this.num = 0; // 프로퍼티는 public 하므로 은닉되지 않는다.
	}

	Counter.prototype.increase = function () {
		return ++num;
	};

	Counter.prototype.decrease = function () {
		return num > 0 ? --num : 0;
	};

	return Counter;
}());

const counter = new Counter();

console.log(counter.increase()); // 1
console.log(counter.increase()); // 2

console.log(counter.decrease()); // 1
console.log(counter.decrease()); // 0
```

위 예제의 `num` 은 생성자 함수 `Counter` 가 생성할 인스턴스의 프로퍼티가 아니라 즉시 실행 함수 내에서 선언된 변수다. 만약 `num` 이 인스턴스의 프로퍼티가 되면 인스턴스를 통해 외부에서 접근이 자유로운 public한 프로퍼티가 된다.  하지만 즉시 실행 함수 내에서 선언된  `num` 은 외부에서도, 인스턴스를 통해서도 접근할 수 없다.

생성자 함수의 `increase` , `decrease` 메서드 모두 즉시 실행 함수의 렉시컬 환경을 기억하는 클로저다. 따라서 `num` 변수의 값은 `increase` , `decrease` 만이 변경할 수 있다.

외부 상태 변경이나 가변 데이터를 피하고 불변성을 지향하는 함수형 프로그래밍에서 부수 효과를 최대한 억제하여 오류를 피하고 안정성을 높이기 위해 클로저는 적극적으로 활용된다. 다음 예제는 함수형 프로그래밍에서 클로저를 활용하는 간단한 예제다.

```jsx
// 함수를 인수로 전달받고 함수를 반환하는 고차 함수
// 이 함수는 카운트 상태를 유지하기 위한 자유 변수 counter를 기억하는 클로저를 반환한다.
function makeCounter(aux) {
  // 카운트 상태를 유지하기 위한 자유 변수
  let counter = 0;

  // 클로저를 반환
  return function () {
    // 인수로 전달 받은 보조 함수에 상태 변경을 위임한다.
    counter = aux(counter);
    return counter;
  };
}

// 보조 함수
function increase(n) {
  return ++n;
}

// 보조 함수
function decrease(n) {
  return --n;
}

// 함수로 함수를 생성한다.
// makeCounter 함수는 보조 함수를 인수로 전달받아 함수를 반환한다
const increaser = makeCounter(increase); // ①
console.log(increaser()); // 1
console.log(increaser()); // 2

// increaser 함수와는 별개의 독립된 렉시컬 환경을 갖기 때문에 카운터 상태가 연동하지 않는다.
const decreaser = makeCounter(decrease); // ②
console.log(decreaser()); // -1
console.log(decreaser()); // -2
```

`makeCounter` 는 함수를 인자로 받고 함수를 반환하는 고차 함수다. 여기서 주의해야 할 것은 `makeCounter` **함수를 호출해 함수를 반환할 때 반환된 함수는 자신만의 독립된 렉시컬 환경을 갖는다**는 것이다. 

<p align='center'>
<img width = '700px' src='https://bl3302files.storage.live.com/y4m8DMb8ko3vEyso5qayywmcHu2F0WmC5mxz5zUYwodXEIvwqgGGV99Cb67ivbNJaTjHS6SbssO_mttrAzCMyCPM0P_9uEGEDjsmdyzadcj6h-VuYbn0LBQPzr9klevofuFc9-NJcutuZV9vbZSmIUGBp5W4TpoGcHRpYYVgyWlpVdppOg-hjx_1ypfksgDxx1_?width=2372&height=786&cropmode=none'>
</p>

처음 `makeCounter` 함수를 호출하면 `makeCounter`함수의 실행 컨텍스트가 생성되고, 렉시컬 환경을 구성한 뒤 이 렉시컬 환경은 `increase` 함수의 [[Environment]] 내부 슬롯이 참조하고 있기 때문에 실행 컨텍스트가 제거되어도 렉시컬 환경은 소멸되지 않는다.

<p align='center'>
<img width = '700px' src='https://bl3302files.storage.live.com/y4mBw7TXZ7gAoz1j6W_EHoHRdtt7Zpeb-NoQ15HGqmCBEbU4aY8NjTeGlszVl1_lR1HRUcCXopuq1tyNiCoUCkVSN6DsUcdwhRqavOxEZjXIkGqcBI9LdCWElJ0XAzh9SeMJPJvC4LS4AR5oGal63FR77oF3Do7WTYnQhPAlBUUX3CmnMlJ6qelwpcYxIfYcAqT?width=2418&height=1320&cropmode=none'>
</p>

다시 `makeCounter` 함수를 호출하면 새로운 `makeCounter` 함수의 실행 컨텍스트를 생성하기 때문에 카운터를 유지하기 위한 자유 변수 `counter` 를 공유하지 않아 카운터의 증감이 연동되지 않는다.  따라서 연동하여 증감이 가능한 카운터를 만들려면 렉시컬 환경을 공유하는 클로저를 만들어야 하고 이를 위해서는 `makeCounter` 함수를 두 번 호출하면 안된다.

```jsx
// 함수를 반환하는 고차 함수
// 이 함수는 카운트 상태를 유지하기 위한 자유 변수 counter를 기억하는 클로저를 반환한다.
const counter = (function () {
  // 카운트 상태를 유지하기 위한 자유 변수
  let counter = 0;

  // 함수를 인수로 전달받는 클로저를 반환
  return function (aux) {
    // 인수로 전달 받은 보조 함수에 상태 변경을 위임한다.
    counter = aux(counter);
    return counter;
  };
}());

// 보조 함수
function increase(n) {
  return ++n;
}

// 보조 함수
function decrease(n) {
  return --n;
}

// 보조 함수를 전달하여 호출
console.log(counter(increase)); // 1
console.log(counter(increase)); // 2

// 자유 변수를 공유한다.
console.log(counter(decrease)); // 1
console.log(counter(decrease)); // 0
```

<br/>

# 24.5 캡슐화와 정보 은닉

캡슐화는 객체의 프로퍼티와 메서드를 하나로 묶는 것을 말한다. 캡슐화는 객체의 특정 프로퍼티나 메서드를 감출 목적으로 사용하기도 하는데 이를 정보 은닉이라 한다.

정보 은닉은 외부에 공개할 필요가 없는 일부를 공개되지 않도록 감추어 적절치 못한 접근으로부터 보호하고, 객체 간의 상호 의존성, 즉 결합도를 낮추는 효과가 있다.

대부분의 객체 지향 프로그래밍 언어는 public, private, protected 같은 접근 제한자를 선언하여 공개 범위를 한정할 수 있다.

자바스크립트는 이러한 접근 제한자를 제공하지 않는다. 따라서 자바스크립트 객체의 모든 프로퍼티와 메서드는 기본적으로 외부에 공개되어 있다. 즉, 기본적으로 public 하다.

```jsx
function Person(name, age) {
  this.name = name; // public
  let _age = age;   // private

  // 인스턴스 메서드
  this.sayHi = function () {
    console.log(`Hi! My name is ${this.name}. I am ${_age}.`);
  };
}

const me = new Person('Lee', 20);
me.sayHi(); // Hi! My name is Lee. I am 20.
console.log(me.name); // Lee
console.log(me._age); // undefined

const you = new Person('Kim', 30);
you.sayHi(); // Hi! My name is Kim. I am 30.
console.log(you.name); // Kim
console.log(you._age); // undefined
```

위 예제의 `name` 프로퍼티는 외부로 공개되어 있어 자유롭게 참조, 변경할 수 있다. 하지만 `_age` 변수는 생성자 함수의 지역 변수이므로 `Person` 생성자 함수 외부에서 참조하거나 변경할 수 없다.

하지만 위 예제의 `sayHi`는 인스턴스 메서드이므로 `Person` 객체가 생성될 때마다 중복 생성된다. `sayHi` 메서드를 프로토타입 메서드로 변경하여 중복 생성을 방지하자.

```jsx
function Person(name, age) {
  this.name = name; // public
  let _age = age;   // private
}

// 프로토타입 메서드
Person.prototype.sayHi = function () {
  // Person 생성자 함수의 지역 변수 _age를 참조할 수 없다
  console.log(`Hi! My name is ${this.name}. I am ${_age}.`);
};
```

이때 `Person.prototype.sayHi` 메서드 내에서 지역 변수 `_age` 를 참조할 수 없는 문제가 발생한다. 따라서 다음과 같이 즉시 실행 함수를 사용하여 `Person` 생성자 함수와 `Person.prototype.sayHi` 메서드를 하나의 함수 내에 모아보자.

```jsx
const Person = (function () {
  let _age = 0; // private

  // 생성자 함수
  function Person(name, age) {
    this.name = name; // public
    _age = age;
  }

  // 프로토타입 메서드
  Person.prototype.sayHi = function () {
    console.log(`Hi! My name is ${this.name}. I am ${_age}.`);
  };

  // 생성자 함수를 반환
  return Person;
}());

const me = new Person('Lee', 20);
me.sayHi(); // Hi! My name is Lee. I am 20.
console.log(me.name); // Lee
console.log(me._age); // undefined

const you = new Person('Kim', 30);
you.sayHi(); // Hi! My name is Kim. I am 30.
console.log(you.name); // Kim
console.log(you._age); // undefined
```

위 패턴을 사용하면 접근 제한자를 제공하지 않는 자바스크립트에서도 정보 은닉이 가능한 것처럼 보인다. 하지만 위 코드도 문제가 있다. `Person` 생성자 함수가 여러 개의 인스턴스를 생성할 경우 `_age` 변수의 상태가 유지되지 않는다는 것이다.

```jsx
const me = new Person('Lee', 20);
me.sayHi(); // Hi! My name is Lee. I am 20.

const you = new Person('Kim', 30);
you.sayHi(); // Hi! My name is Kim. I am 30.

// _age 변수 값이 변경된다!
me.sayHi(); // Hi! My name is Lee. I am 30.
```

이는 `Person.prototype.sayHi` 메서드가 단 한 번 생성되는 클로저이기 때문에 발생하는 현상이다. `Person.prototype.sayHi` 메서드는 즉시 실행 함수가 호출될 때 생성된다. 이때 `Person.prototype.sayHi` 메서드는 자신의 상위 스코프인 즉시 실행 함수의 렉시컬 환경의 참조를 [[Environment]]에 저장하기 때문에 `Person` 생성자 함수의 모든 인스턴스가 상속을 통해 호출할 수 있는 `Person.prototype.sayHi` 메서드의 상위 스코프는 동일한 상위 스코프를 사용하게 된다.

이처럼 자바스크립트는 정보 은닉을 완전하게 지원하지 않는다. 다행히도 현재 private 필드를 정의할 수 있는 새로운 표준 사양이 제안되어 있다. 표준 사양으로 승급이 확실시되는 이 제안은 현재 최신 브라우저에 이미 구현되어 있다. 이에 대해서는 25장에서 더 자세히 살펴본다.

<br/>

# 24.6 자주 발생하는 실수

아래는 클로저를 사용할 때 자주 발생할 수 있는 실수를 보여주는 예제다.

```jsx
var funcs = [];

for (var i = 0; i < 3; i++) {
  funcs[i] = function () { return i; }; // ①
}

for (var j = 0; j < funcs.length; j++) {
  console.log(funcs[j]()); // ②
}
```

②의 결과로 1, 2, 3이 출력될 것을 기대하지만 결과는 그렇지 않다.

`for` 문 안에서 선언한 `i` 가 `var` 로 선언되었기 때문에 블록 레벨 스코프가 아닌 함수 레벨 스코프를 갖기 때문에 전역 변수다. 따라서 `funcs` 에 들어가게 되는 함수는 같은 전역 변수 `i` 를 보게되고, 마지막으로 할당된 3이 3번 출력된다.

클로저를 사용해 위 예제를 바르게 동작하게 하면 다음과 같다.

```jsx
var funcs = [];

for (var i = 0; i < 3; i++){
  funcs[i] = (function (id) { // ①
    return function () {
      return id;
    };
  }(i));
}

for (var j = 0; j < funcs.length; j++) {
  console.log(funcs[j]());
}
```

`func` 에 들어갈 값에 즉시 실행 함수가 반환하는 함수를 넣게 되면 반환 된 함수는 자신위 상위 스코프(즉시 실행함수)를 기억하는 클로저이고, 자유 변수 `id` 를 기억하여 그 값이 유지된다.

위 예제는 `for` 문 안에 `var` 키워드를 사용해 생긴 현상으로 `let` 을 사용하면 깔끔하게 해결된다.

```jsx
var funcs = [];

for (let i = 0; i < 3; i++) {
  funcs[i] = function () { return i; }; // ①
}

for (let j = 0; j < funcs.length; j++) {
  console.log(funcs[j]()); // ②
}
```

`let` 은 블록 레벨 스코프를 따르므로 코드 블록마다 새로운 렉시컬 환경을 생성한다.

또 다른 방법으로는 고차 함수를 사용하는 방법이 있다.
```jsx
// 요소가 3개인 배열을 생성하고 배열의 인덱스를 반환하는 함수를 요소로 추가한다.
// 배열의 요소로 추가된 함수들은 모두 클로저다.
const funcs = Array.from(new Array(3), (_, i) => () => i); // (3) [ƒ, ƒ, ƒ]

// 배열의 요소로 추가된 함수 들을 순차적으로 호출한다.
funcs.forEach(f => console.log(f())); // 0 1 2
```