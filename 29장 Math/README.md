# 29장 Math
## 29.1 Math 프로퍼티
### 29.1.1 Math.PI
- 원주율 PI 값(π = 3.14159265358979…)을 반환
```jsx
console.log(Math.PI); //3.141592653589793
```
## 29.2 Math 메서드
### 29.2.1 Math.abs
- 인수로 전달된 숫자의 절대값을 반환
- 절대값은 반드시 0 또는 양수
```jsx
console.log(Math.abs(-1)); //1
console.log(Math.abs('-1')); //1
console.log(Math.abs(0)); //0
console.log(Math.abs([])); //0
console.log(Math.abs(null)); //0
console.log(Math.abs(undefined)); //NaN
console.log(Math.abs({})); //NaN
console.log(Math.abs('string')); //NaN
console.log(Math.abs()); //NaN
```

### 29.2.2 Math.round
- 인수로 전달된 숫자의 소수점 이하를 반올림한 정수를 반환.
```jsx
console.log(Math.round(1.4));
console.log(Math.round(1.6));
console.log(Math.round(-1.4));
console.log(Math.round(-1.6));
console.log(Math.round(1));
console.log(Math.round());
```
### 29.2.3 Math.ceil
- 인수로 전달된 숫자의 소수점 이하를 올림한 정수를 반환
```jsx
console.log(Math.ceil(1.3)); //2
console.log(Math.ceil(-1.3)); //-1
console.log(Math.ceil(1)); //1
console.log(Math.ceil()); //NaN
```

### Math.floor
- 인수로 전달된 숫자의 소수점 이하를 내림한 정수를 반환.
```jsx
    console.log(Math.floor(1.9)) //1
    console.log(Math.floor(9.1)) //9
    console.log(Math.floor(-1.9)) //-2
    console.log(Math.floor(-9.1)) //-10
    console.log(Math.floor(1)) //1
    console.log(Math.floor()) //NaN
```

### 29.2.5 Math.sqrt
- 인수로 전달된 숫자의 제곱근을 반환

```jsx
console.log(Math.sqrt(9)); //3
console.log(Math.sqrt(-9)); //NaN
console.log(Math.sqrt(2)); //1.4142135623...
console.log(Math.sqrt(1)); //1
console.log(Math.sqrt(0)); //0
console.log(Math.sqrt()); //NaN
```

### 29.2.6 Math.random
- 임의의 랜덤 숫자를 반환
- 0~1미만의 실수, 즉 1은 포함하지 않는다.
```jsx
console.log(Math.random()) //0.6509421110462617

//1~10 범위의 랜덤 정수 획득
const random = Math.floor((Math.random() * 10) + 1); 
console.log(random);
```

### 29.2.7 Math.pow
- Math.pow 메서드는 첫번째 인수를 밑으로, 두번째 인수를 지수로 거듭제곱한 결과를 반환.
```jsx
Math.pow(2,3); //8
Math.pow(2,-1); //0.5
Math.pow(2);

//ES7에서 도입된 지수 연산자를 사용하면 가독성이 더 좋다.
console.log(2**3); //8

```

### 29.2.8 Math.max
- 전달받은 인수 중에서 가장 큰 수를 반환
- 인수가 전달되지 않으면 `-Infinity`를 반환
```jsx
console.log(Math.max(1,2,3)); //3
console.log(Math.max()); //-Infinity
```

- 배열의 요소 중에서 최대값을 구하려면 `Function.prototype.apply`메서드 or 스프레드 문법을 사용
```jsx
//배열 요소 중에서 최대값 취득
Math.max.apply(null,[1,2,3]);

//ES6 스프레드 문법
Math.max(...[1,2,3]);

```

## 29.2.9 Math.min
- 전달받은 인수 중에서 가장 작은 수를 반환
- 인수가 전달되지 않으면 `Infinity`를 반환
```jsx
    console.log(Math.min(1));//1
    console.log(Math.min(1,2,3));//1
    console.log(Math.min());//Infinity
```
- 배열의 요소 중에서 최소값을 구하려면 Function.prototype.apply 메서드 또는 스프레드 문법을 사용한다.
```jsx
// 배열 요소 중에서 최소값 획득
Math.min.apply(null, [1,2,3]);

// ES6 스프레드 문법
Math.min(...[1,2,3]);
```

29장 Math
1. 아래 참조값을 말해주세요.
```jsx
console.log(Math.abs(-1));

console.log(Math.round(1.5));
console.log(Math.round(-1.2));

console.log(Math.ceil(1.2));
console.log(Math.ceil(-1.2));

console.log(Math.floor(9.1)) ;
console.log(Math.floor(-1.9));

console.log(Math.sqrt(9));

console.log(Math.pow(2,3));

console.log(Math.max());
console.log(Math.min());

```

