# 8장 제어문
- 조건에 따라 코드 블록을 실행(조건문)하거나 반복 실행(반복문)할 때 사용

## 8.1 블록문

- 0개 이상의 문을 **중괄호**로 묶은 것
- 코드 블록 or 블록이라고 부르기도 함.
- 단독 사용도 가능하나 일반적으로 제어문, 함수를 정의할 때 사용
- 자체 종결성을 가져 끝에 세미콜론을 붙이지 않는다.

## 8.2 조건문

- 주어진 조건식의 평가 결과에 따라 코드 블록의 실행을 결정한다.
- 조건식은 불리언 값으로 평가될 수 있어야 한다.
- if else문과 switch문이 있다.

### 8.2.1 if...else 문

- 주어진 조건식의 논리적 참/거짓에 따라 실행할 코드 블록을 결정
- 조건식은 불리언 값으로 평가되어야 하며
    
    불리언 값이 아닐땐 암묵적 타입변환에 의해 불리언 값으로 강제 변환된다.
    
- 조건식을 추가하고 싶을 때 :  else if 문을 추가한다.
- else if와 else는 옵션이다.

```jsx
if(조건식){
  //조건식이 참일 때 실행.
} else{
  //조건식이 거짓일 때 실행.
}

//조건식 추가시 else if문
if(조건식1){
	//조건식1이 참일때
} else if(조건식2){
	//조건식2가 참일때
} else{
	//조건식1,2가 모두 거짓이면 이 코드블록이 실행된다.
}
```

- 블록 내의 문이 하나일때는 중괄호 생략 가능.

```jsx
var num = 2;
var kind;

if(num >0)      kind = '양수';
else if(num <0) kind = '음수';
else            kind='영';

console.log(kind);//양수
```

- 삼항 조건 연산자로 바꿔 쓸 수 있다.
- 값으로 평가되는 표현식을 만들기 때문에 변수에 할당할 수 있다.
- if else문은 값처럼 사용할 수 없기때문에 변수할당 x
- 코드가 짧을 때는 삼항 조건 연산자가 유용, 코드가 길어질 때는 else if문

```jsx
var x = 2;

//if else문
var result;
if(x%2){
    result="홀";
} else{
    result="짝";
}

// 삼항 조건 연산자
var result = x%2 ? "홀" : "짝";

console.log(result);

// 경우의 수가 세가지일 때
var num = 2;
var kind = num ? (num > 0 ? '양수' : '음수') : '영';

console.log(kind);
```

### 8.2.2 switch 문

- if else문은 논리적 참, 거짓으로 실행할 코드를 결정
- switch문은 다양한 상황에 따라 실행할 코드 블록을 결정할 때 사용.

```jsx
switch (표현식){
    case 표현식1:
        switch문의 표현식과 표현식1이 일치하면 실행될 문;
        break;
    case 표현식2:
        switch문의 표현식과 표현식2이 일치하면 실행될 문;
        break;
    case 표현식3:
        switch문의 표현식과 표현식3이 일치하면 실행될 문;
        break;
    default:
        switch문의 표현식과 일치하는 case문이 없을 때 실행될 문
}
```

- 폴스루(fall through) : 문을 실행한 후 switch문을 탈출하지 않고 모든 case문과 default문을 실행하는 것.
- break : 코드 블록에서 탈출하는 역할

```jsx
var month = 1;
var monthName;

switch(month){
    case 1 : monthName = "January";
    case 2 : monthName = "Februay";
    case 3 : monthName = "March";
    default : monthName = 'invalid month'
}

console.log(monthName) //invalid month

switch(month){
    case 1 : monthName = "January";
    break;
    case 2 : monthName = "Februay";
    break;
    case 3 : monthName = "March";
    break;
    default : monthName = 'invalid month'
}

console.log(monthName) //January
```

- break문을 생략한 폴스루가 유용한 경우
- 여러 개의 case 문을 하나의 조건으로 사용할 수 있다.

```jsx
var year = 2022;
var month = 2;
var days = 0;

switch(month){
    case 1: case 3: case 5: case 7: case 8: case 10: case 12:
        days = 31;
        break;
    case 4: case 6: case 9: case 11: 
        days = 30;
        break;
    case 2:
        days = ((year%4 === 0 && year % 100 !== 0) || (year % 400 === 0)) ? 29 : 28;
        break;
    default:
        console.log('invalid month');
}
console.log(days);
```

- if else문으로 해결할 수 있다면 switch 문보다는 if else를 사용하는 편이 좋다.
    - 다양한 키워드를 사용하고, 폴스루가 발생하며 문법도 복잡하기 때문.
- 조건이 너무 많아서 switch문의 가독성이 더 좋다면 switch 문으로 사용.

## 8.3 반복문

- for문, while문, do...while문을 제공

### 8.3.1 for 문

- 조건식이 거짓으로 평가될 때까지 코드 블록을 반복 실행

```jsx
for(변수 선언문 or 할당문; 조건식; 증감식){
	조건식이 참인 경우 반복 실행될 문;
}
```

- 중첩 for 문

```jsx
for( var i=1; i<=6; i++){
    for(var j=1; j<=6; j++){
        if(i+j ===6) console.log(`${i}+${j}`);
    }
}
//1+5
//2+4
//3+3
//4+2
//5+1
```

### 8.3.2 while 문

- while 문은 평가 결과가 참이면 코드 블록을 계속 반복 실행
- for 문은 반복횟수가 명확할때 사용
- while문은 반복횟수가 불명확할 때 줄 주로 사용.

```jsx
var count = 0;
while(count < 3){
    console.log(count); //0 1 2
    count ++;
}
```

- 조건식의 평가결과가 참이면 무한루프가 된다.
- 이때는 if문으로 탈출 조건을 만들어 break로 코드 블록을 탈출한다.

```jsx
var count = 0;

while(true){
    console.log(count);
    count++;

    if(count===3) break;
}
```

### 8.3.2 do... while 문

- 코드 블록을 먼저 실행하고 조건식을 평가한다.
- 코드블록은 무조건 한 번 이상 실행된다.

```jsx
var count = 0;

do{
    console.log(count);
    count++;
} while(count<3);
```

## 8.4 break 문

- break문은 코드블록을 탈출한다.
- 레이블문, 반복문, switch 문의 코드블록을 탈출하는 것.
- 위 문들 외에 break 문 사용시 syntaxError 발생

## 8.5 continue 문

- 반복문의 코드 블록 실행을 현 지점에서 중단하고,
- 증감식으로 실행 흐름을 이동시킨다.
- break문처럼 반복문을 탈출하진 않는다.

```jsx
//특정 문자 개수 세기
var string = 'Hello world';
var search = 'l';
var count = 0;

for(var i=0; i<string.length; i++){
    if(string[i]!==search) continue;
    count++;
}

console.log(count); ++

//for(var i=0; i<string.length; i++){
//    if(string[i]===search) count++;
//}
```

- if 문 내에서 코드가 길때 continue 문을 사용하는 편이 가독성이 좋다.