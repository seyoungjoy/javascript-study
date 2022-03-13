# 4.1 변수란 무엇인가? 왜 필요한가? 
### 컴퓨터는 CPU를 사용해 연산하고, 메모리를 사용해 데이터를 기억한다.

컴퓨터의 메모리는 메모리 셀의 집합이며 메모리 셀은 각 8bit (1byte)이고, 컴퓨터는 메모리 셀 단위인 1byte로 데이터를 저장하거나 읽어들인다.

<p align='center'><img src='https://bl3302files.storage.live.com/y4mau5oKSOM-5-XQEoR3W3sG53ZK43FpdTj96YufR0u1qk3u5ScE_CnxLn6seLR09JRBP0UZDf6tbB-mHej1UMt6FcRqaFDg1dMWgzlbGFV2AjDnogJ2XrT_sTZnx5GfjqQHbIuXywHXCTwrjyk-81azP4quuhKOAMzA3IdgDay3OLVhd9rvjb8sL3qhanr3zbp?width=598&height=385&cropmode=none' width='250px'></p>


### 그렇다면 자바스크립트에서 10 + 20 을 수행하면 어떻게 될까?

컴퓨터는 10과 20이라는 값을 임의의 메모리 주소에 할당하고 CPU는 이 값을 읽어 + 연산자를 통해 30이라는 결과 값을 다시 임의의 메모리 주소에 저장한다.

<p align='center'>
<img src='https://bl3302files.storage.live.com/y4mbjZdPqfy3jwJ7R8mf458Le7HdiD8FuBeoT2tVGaSMul8gXfUAqCkG9-FxjxC4FQCUwbBz39Rqw1QJmWMzge6iXg1-0UIhstPVs9qD23DWX429UKpetFrT-ftFrScASrq_T5W0Li6LlPSixunW8ZMK7YD-qlngG52U12GlXxy1QF_KgJ-RJKtOGsteXnDBOYU?width=1558&height=596&cropmode=none' width='500px'>
</p>

하지만 지금 상황에서는 결과 값 30을 사용하기 위해서는 30이 저장된 메모리 주소값을 알아내 직접 접근하여 가져올수 밖에 없다.

하지만 다음과 같은 이유로 메모리에 직접 접근하는 것은 올바른 방법이 아니다.

- 자바스크립트는 치명적 오류 발생의 가능성을 이유로 개발자의 메모리 제어를 하용하지 않는다.
- 메모리 주소는 상황에 따라 임의로 결정되고, 코드를 실행할 때마다 달라진다.  
   
<br/>

### 따라서 프로그래밍 언어는 **변수**라는 메커니즘을 제공한다.

**변수 는 하나의 값을 저장하기 위해 확보한 메모리 공간 자체 또는 그 메모리 공간을 식별하기 위해 붙인 이름을 말한다.** (*여기서 확보한 메모리 공간 자체는 원시 값을 저장할 때를 말하며 메모리 공간을 식별하기 위해 붙인 이름은 참조 값을 저장할 때를 말한다.)*

- 간단히 말하면 변수는 **값의 위치(메모리 주소)를 가리키는 상징적인 이름이다.**
- 변수는 컴파일러 혹은 인터프리터에 의해 값이 저장된 메모리 공간의 주소로 치환되어 실행된다.
- 이제 개발자는 메모리 주소를 통해 값을 참조하지 않고 변수를 통해 접근한다.

<br/>

### 변수를 사용해보자

```jsx
var result = 10 + 20;
```

- 변수 이름: 메모리 공간에 저장된 값을 식별할 수 있는 고유한 이름
- 변수 값: 변수에 저장된 값
- 할당(대입, 저장): 변수에 값을 저장하는 행위
- 참조: 변수에 저장된 값을 읽는 행위
    
    변수 이름 `result`에 변수 값 `30` 을 할당하여 변수 이름 `result` 를 통해 참조를 요청하면 변수 이름과 매핑된 메모리 주소를 통해 메모리 공간에 접근해 할당된 값을 반환한다.
    

<br/><br/>

# 4.2 식별자

**식별자는 어떤 값을 구별해서 식별할 수 있는 고유한 이름을 말한다.**

- 변수 이름을 식별자 라고도 한다.
- 식별자는 메모리 공간에 저장되어 있는 값을 구별해서 식별해 낼 수 있어야 한다.

**식별자는 값이 아니라 메모리 주소를 기억하고 있다.**

- 즉, 식별자는 메모리 주소에 붙인 이름이라고 할 수 있다.

변수 이름만이 아니라 메모리 상에 존재하는 어떤 값을 식별 할 수 있는 이름은 모두 식별자라 부른다.

식별자는 네이밍 규칙을 준수해야 하며 **선언**에 의해 존재를 알린다.

<br/><br/>

# 4.3 변수 선언

### 변수 선언은 변수를 생성하는 것을 말한다.

- 메모리 공간을 확보하고 확보된 메모리 공간의 주소를 연결하는 것이다.
- 확보된 메모리 공간은 해제되기 전까지 다른 누군가가 사용할 수 없으므로 안전하게 사용할 수 있다.

변수를 선언할 때는 `var`, `let`, `const` 키워드를 사용한다.

- 키워드: 자바스크립트 엔진이 수행할 동작을 규정한 일종의 명령어. 자바스크립트 엔진이 키워드를 만나면 자신이 수행해야 할 약속된 동작을 수행한다.
e.g.) `var` 키워드를 만나면 뒤에 오는 변수 이름으로 새로운 변수를 선언한다.

<br/>

### 변수 선언을 해보자.

```jsx
var score; // 변수 선언
```
<p align='center'>
<img src='https://bl3302files.storage.live.com/y4mvyRxsP0TzR0SGfniIL0eerqmSATI1SzaVWxAMcskEI2VDmwieg6gLvOa-izj8-8J6KhhcP8YiNKTZ2bRjR_4I2WSukWPG1iZYsNOVF6gQn6WGvlrYjHUGqn3bC4Likosg_MFRXcHvHjPk8O6IziVXeqgBsCDFSFkSgFb6g-i5SMmP8T-QM7e3luZZJ3CLrkl?width=788&height=338&cropmode=none' width='500px'>
</p>
변수에 선언만 했을 뿐 할당은 하지 않았다. 이때 확보된 메모리 공간은 비어있는게 아니라 `undefined`라는 값이 암묵적으로 할당되어 초기화 된다.

자바스크립트 엔진은 변수 선언을 다음과 같은 2단계에 거쳐 수행한다.

- 선언 단계: 변수 이름을 등록해서 변수의 존재를 알린다.
- 초기화 단계: 값을 저장하기 위한 메모리 공간을 확보하고 암묵적으로 `undefined`를 할당해 초기화한다.

`var` 키워드를 사용한 변수 선언은 선언 단계와 초기화 단계가 동시에 진행된다.

일반적으로 `초기화` 란 변수가 선언된 이후 최초로 값을 할당하는 것을 말한다. 만약 초기화 단계를 거치지 않으면 이전에 다른 애플리케이션이 사용했던 쓰레기 값이 남아 있을 수 있다.

모든 식별자를 사용하기 위해선 반드시 선언이 필요하다. 선언하지 않은 식별자에 접근시 `ReferenceError` 가 발생한다.

<br/>

> 💡 **변수의 이름은 어디에 등록 될까?**  
> ➡️ 13장 스코프와 23장 실행 컨텍스트, 메모리힙, 콜스택에 대해 알아보자.

<br/><br/>

# 4.4 변수 선언의 실행 시점과 변수 호이스팅

```jsx
console.log(score); // undefined

var score; // 변수 선언문
```

위의 예제는 변수를 선언하기 전에 참조를 했지만 `ReferenceError` 가 발생하지 않았다.

그 이유는 **변수 선언이 인터프리터에 의해 한 줄씩 순차적으로 실행되는 런타임 시점이 아니라 그 이전 단계에서 먼저 실행되기 때문이다.**

자바스크립트 엔진은 코드를 실행하기에 앞서 소스코드의 평가 과정을 거친다. 이 평가 과정에서 모든 선언문을 코드에서 찾아내 먼저 실행한다. **즉, 자바스크립트 엔진은 변수 선언이 코드의 어디에 있든 상관없이 먼저 실행된다.**

**이처럼 선언문이 코드의 선두로 올려진 것처럼 동작하는 자바스크립트 고유의 특징을 변수 호이스팅이라 한다.**

<br/><br/>

# 4.5 값의 할당

할당 연산자 `=` 를 사용하여 변수에 값을 할당한다. 할당 연산자는 우변의 값을 좌변에 할당한다.

```jsx
var score; // 선언
score = 80; // 할당

var score = 80; // 하나의 문으로 단축 표현할 수 있다.
```

주의할 점은 변수 선언과 값의 할당의 실행 시점이 다르다는 것이다.

**변수 선언은 런타임 이전에 먼저 실행되지만. (호이스팅) 값의 할당은 런타임에 실행된다.**

다음 코드는 ① ~ ④의 순서로 실행된다.

```jsx
console.log(score); // ② score 변수 참조: undefined 

var score = 80; // ① 변수 score 할당, ③ 변수 score에 값 할당

console.log(score); // ④ score 변수 참조: 80
```
<br/><br/>

# 4.6 값의 재할당

재할당이란 이미 값이 할당되어 있는 변수에 새로운 값을 할당하는 것이다. 변수는 저장된 값을 다른 값으로 변경이 가능하기 때문에 변수라고 한다.

만약 값을 재할당할 수 없다면 변수가 아니라 **상수**라 한다.

```jsx
var score = 80;
score = 90; // 재할당
```

변수 값을 재할당 후 다시 `score` 변수를 참조하면 80에서 90으로 변경이 되어있다. 

이 때 80이 저장되어 있던 메모리 공간을 지우고 그 메모리 공간에 재할당 값 90을 새롭게 저장하는 것이 아니라 새로운 메모리 공간을 확보하고 그 메모리 공간에 값을 저장한다.
<p align='center'>
<img src='https://bl3302files.storage.live.com/y4m0ASSyAW0DaoAEuhoYtr2AfNvhYNI4RHD0sqoxbEi0bcO3Ng5xZ9WxNDqAf1xjPW_G2EKfEeRqoEw-kvpRsNmedSopdgeFxVUHs5FivN-xcPWTL24Z62MT_2CLQz1zmbwijPqTNmFZngCtDl4PmD8ZRprrKlZfxIAIw1YT6NfafYdEUC7QgkkGUmAe8TRvONC?width=1840&height=702&cropmode=none'>
</p>
재할당 후 음영처리된 부분은 어떤 식별자와도 연결되어 있지 않다. 이러한 불필요한 값들은 가비지 콜렉터에 의해 메모리에서 자동해제된다.

# 4.7 식별자 네이밍 규칙

식별자는 다음과 같은 네이밍 규칙을 준수해야 한다.

1. 특무순자를 제외한 문자, 숫자, 언더스코어(_), 달러 기호($)를 포함할 수 있다.
2. 숫자로는 시작할 수 없다.
3. 예약어는 식별자로 사용할 수 없다.

다음과 같은 식별자는 명명 규칙에 위배되므로 변수 이름으로 사용할 수 없다.

```jsx
var first-name; // 사용 불가한 문자(-)가 들어감.
var 1st; // 숫자로 시작함.
var this; // 예약어 사용
```

자바스크립트는 대소문자를 구별하므로 다음 변수는 각각 별개의 변수다.

```jsx
var fistname;
var firstName;
var FirstName;
```

네이밍 컨벤션은 식별자를 만들 때 가독성 좋게 단어를 한눈에 구분하기 위해 규정한 명명 규칙이다.

```jsx
// 카멜 케이스(camelCase)
var fisrtName;

// 스네이크 케이스(snake_case)
var first_name;

// 파스칼 케이스(PascalCase)
var FirstName;

// 헝가리언 케이스(typeHungarianCase)
var strFirstName;
var $elem = document.getElementById('myId');
var observable$ = fromEvent(document, 'click');
```

자바스크립트에서는 일반적으로 변수나 함수에서는 카멜 케이스를 사용하고 생성자 함수와 클래스 이름에는 파스칼 케이스를 사용한다.