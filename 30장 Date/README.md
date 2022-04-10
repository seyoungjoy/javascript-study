# 30장 Date
- 표준 빌트인 객체 Date는 날짜와 시간을 위한 메서드를 제공하는 빌트인 객체이면서 생성자 함수다.
- UTC(협정 세계시)는 국제 표준시를 말하며 KST(한국 표준시)는 UTC에 9시간을 더한 시간이다.
  - 즉 KST는 UTC보다 9시간이 빠르다. (Ex : UTC 00:00 AM은 KST 09:00 AM)
- 현재 날짜와 시간은 자바스크립트 코드가 실행된 시스템의 시계에 의해 결정된다.

## 30.1 Date 생성자 함수
- Date 생성자 함수로 생성한 Date 객체는 내부적으로 날짜와 시간을 나타내는 정수값을 갖는다.
- 이 값은 1970년 1월 1일 00:00:00(UTC)을 기점으로 Date 객체가 나타내는 날짜와 시간까지의 밀리초를 나타낸다.
- 예를 들어 모든 시간의 기점인 1970년 1월 1일 0시를 나타내는 Date 객체는 내부적으로 정수값 0을 가지며,
- 1970년 1월 1일 0시를 기점으로 하루가 지난 1970년 1월 2일 0시를 나타내는 Date 객체는 내부적으로 정수값 86,400,000(24h * 60m * 60s * 1000ms)
- Date 생성자 함수로 객체를 생성하는 방법은 다음과 같이 4가지가 있다.

### 30.1.1 new Date()
- new 연산자와 함께 호출하면 현재 날짜와 시간을 가지는 Date 객체를 반환한다.
- Date 객체는 내부적으로는 날짜와 시간을 나타내는 정수값을 갖지만 Date 객체를 콘솔에 출력하면 기본적으로 날짜와 시간 정보를 출력한다.
- Date 생성자 함수를 new 연산자 없이 호출하면 Date 객체를 반환하지 않고 날짜와 시간 정보를 나타내는 문자열을 반환한다.
```jsx
const date = new Date();

console.log(date); //Fri Apr 08 2022 14:50:48 GMT+0900 (한국 표준시)
console.log(typeof date); //object

const stringDate = Date();
console.log(stringDate)//Fri Apr 08 2022 14:50:48 GMT+0900 (한국 표준시)
console.log(typeof stringDate)//string
```

### 30.1.2 new Date(Millisecond)
```jsx
// 한국 표준시 KST는 협정 세계시 UTC에 9시간을 더한 시간이다.
const date = new Date(0);
console.log(date) //Thu Jan 01 1970 09:00:00 GMT+0900 (한국 표준시)
// 86400000ms 는 1day를 의미힌다.
const day = new Date(86400000);
console.log(day); //Fri Jan 02 1970 09:00:00 GMT+0900 (한국 표준시)

```

### 30.1.3 new Date(dateString)
- Date 생성자 함수에 날짜와 시간을 나타내는 문자열을 인수로 전달하면 지정된 날짜와 시간을 나타내는 Date 객체를 반환한다.
- 이때 인수로 전달한 문자열은 Date.parse 메서드에 의해 해석 가능한 형식이어야 한다.
```jsx
const date = new Date('May 26, 2020 10:00:00');
console.log(date) //Tue May 26 2020 10:00:00 GMT+0900 (한국 표준시)
```

### 30.1.4 new Date(year, month[, day, hour, minute, second, millisecond])
- Date 생성자 함수에 연, 월, 일, 시, 분, 초, 밀리초를 의미하는 숫자를 인수로 전달하면 지정된 날짜와 시간을 나타내는 Date 객체를 반환한다.
```jsx
const date = new Date(2020, 2, 26, 10, 00, 00, 0);
console.log(date) //Thu Mar 26 2020 10:00:00 GMT+0900 (한국 표준시)
```

## 30.2 Date 메서드

### 30.2.1 Date.now
- 1970년 1월 1일 00:00:00(UTC)을 기점으로 현재 시간까지 경과한 밀리초를 숫자로 반환한다.
```jsx
    const now = Date.now();
    console.log(now); //1649398244033
```

### 30.2.2 Date.parse
- 1970년 1월 1일 00:00:00(UTC) 기점으로 인수로 전달된 시간까지의 밀리초를 숫자로 반환한다.
```jsx
const date = Date.parse('Jan 2, 1970 00:00:00 UTC');
console.log(date) //86400000
```

### 30.2.3 Date.UTC
- 1970년 1월 1일 00:00:00(UTC) 기점으로 인수로 전달된 시간까지의 밀리초를 숫자로 반환한다.

```jsx
const date = Date.UTC(1970, 0, 2);
const date = Date.UTC('1970/1/2');

console.log(date); //86400000
```

### 30.2.4 Date.prototype.getFullYear
- Date 객체의 연도를 나타내는 정수를 반환

### 30.2.5 Date.prototype.setFullYear
- Date 객체에 연도를 나타내는 정수를 설정

### 30.2.6 Date.prototype.getMonth
- 객체의 월을 나타내는 0~11의 정수를 반환
- 1월은 0, 12월은 11

### 30.2.7 Date.prototype.setMonth
- 객체의 월을 나타내는 0~11의 정수를 설정

### 30.2.8 Date.prototype.getDate
- Date 객체의 날짜(1~31)를 나타내는 정수를 반환

### 30.2.9 Date.prototype.setDate
- Date 객체의 날짜(1~31)를 나타내는 정수를 설정.

```jsx
const today = new Date();

//년도 지정
today.setFullYear(2000);
console.log(today.getFullYear());//2000

//월 지정
today.setMonth(1);
console.log(today.getMonth());//1

//날짜 정지정
today.setDate(28);
console.log(today.getDate());//28
```

### 30.2.10 Date.prototype.getDay
- Date 객체의 요일(0~6)을 나타내는 정수를 반환
<p align="center">

|요일|반환값|
|:---:|:---:|
|일요일|0|
|월요일|1|
|화요일|2|
|수요일|3|
|목요일|4|
|금요일|5|
|토요일|6|
</p>

```jsx
const date = new Date('2020/07/24').getDay();
console.log(date) //5
```
### 30.2.11 Date.prototype.getHours
- Date 객체에 시간(0~23)을 나타내는 정수를 반환

### 30.2.12 Date.prototype.setHours
- Date 객체에 시간(0~23)을 나타내는 정수를 설정
- 시간 이외에 옵션으로 분, 초, 밀리초도 설정할 수 있다.

### 30.2.13 Date.prototype.getMinutes
- Date 객체의 분(0~59)을 나타내는 정수를 반환

### 30.2.14 Date.prototype.setMinutes
- Date 객체의 분(0~59)을 나타내는 정수를 설정
- 분 이외에 옵션으로 초, 밀리초도 설정할 수 있다.

### 30.2.15 Date.prototype.getSeconds
- Date 객체의 초(0~59)를 나타내는 정수를 반환

### 30.2.16 Date.prototype.setSeconds
- Date 객체에 초(0~59)를 나타내는 정수를 설정한다.
- 초 이외에 옵션으로 밀리초도 설정할 수 있다.

### 30.2.17 Date.prototype.getMilliseconds
- Date 객체의 밀리초(0~999)를 나타내는 정수를 반환

### 3.2.18 Date.prototype.setMilliseconds
- Date 객체의 밀리초(0~999)를 나타내는 정수를 설정
```jsx
const today = new Date();

//시간 지정
today.setHours(7);
console.log(today.getHours()) //7

//옵션으로 분, 초, 밀리초도 설정 가능
//today.setHours(0, 0, 0, 0);

//분 지정
today.setMinutes(22);
console.log(today.getMinutes());//22

//초 지정
today.setSeconds(30);
console.log(today.getSeconds())//30

//밀리초 지정
today.setMilliseconds(100);
console.log(today.getMilliseconds());//100

console.log(today); //Fri Apr 08 2022 07:22:30 GMT+0900 (한국 표준시)
```

### 30.2.19 Date.prototype.getTime
- 1970년 1월 1일 00:00:00을 기점으로 Date 객체의 시간까지 경과된 밀리초를 반환
```jsx
new Date('2020/07/24/12:30').getTime();
//1595561400000
```
### 30.2.20 Date.prototype.setTime
- Date 객체에 1970년 1월 1일 00:00:00를 기점으로 경과된 밀리초를 설정한다.
```jsx
const today = new Date();
today.setTime(1595561400000);
console.log(today)
//Fri Jul 24 2020 12:30:00 GMT+0900 (한국 표준시)
```

### 30.2.21 Date.prototype.getTimezoneOffset
- UTC와 Date 객체에 지정된 로켈 시간과의 차이를 분 단위로 반환한다.
- KST는 UTC에 9시간을 더한 시간이다.

### 30.2.22 Date.prototype.toDateString
- 사람이 읽을 수 있는 형식의 문자열로 Date 객체의 날짜를 반환한다.

### 30.2.23 Date.prototype.toTimeString
- 사람이 읽을 수 있는 형식으로 Date 객체의 시간을 표현한 문자열을 반환한다.

### 30.2.24 Date.prototype.toISOSring
- ISO 8601 형식으로 Date 객체의 날짜와 시간을 표현한 문자열을 반환한다.

### 30.2.25 Date.prototype.toLocaleString
- 인수로 전달한 로캘을 기준으로 Date 객체의 날짜와 시간을 표현한 문자열을 반환

### 30.2.26 Date.prototype.toLocaleTimeString
- 인수로 전달한 로캘을 기준으로 Date 객체의 시간을 표현한 문자열을 반환한다.

## 30.3 Date를 활용한 시계 예제
다음 예제는 현재 날짜와 시간을 초 단위로 반복 출력한다.
```jsx
    (function printNow(){
        const today = new Date();

        const dayNames = [
            '(일요일)','(월요일)','(화요일)','(수요일)','(목요일)','(금요일)','(토요일)',
        ];

        //getDay 메서드는 해당 요일 (0~6)을 나타느낸 정수를 반환한다.
        const day = dayNames[today.getDay()];
        const year = today.getFullYear();
        const month = today.getMonth();
        const date = today.getDate();
        let hour = today.getHours();
        let minute = today.getMinutes();
        let second = today.getSeconds();

        const ampm = hour <= 12 ? 'PM' : 'AM';

        //12시간제로 변경
        hour %= 12;
        hour = hour || 12;

        //10 미만인 분과 초를 2자리로 변경
        minute = minute < 10 ? '0' + minute : minute;
        second = second < 10 ? '0' + second : second;

        const now = `${year}년 ${month}월 ${date}일 ${day} ${hour}:${minute}:${second} ${ampm}`;

        console.log(now);

        setTimeout(printNow, 1000);
    }());
```

