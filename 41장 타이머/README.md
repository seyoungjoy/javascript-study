# 41.1 호출 스케줄링

함수를 명시적으로 호출하지 않고 일정 시간이 경과된 이후에 호출되도록 함수 호출을 예약하려면 타이머 함ㅅ ㅜ를 사용한다. 이를 호출 스케줄링이라 한다.

자바스크립트는 타이머 함수 `setTimeout`, `setInterval`, `clearTimeout`, `clearInterval` 을 제공한다. 이는 빌트인 함수가 아니라 브라우저와 Node.js에서 환경에서 제공하는 전역 객체의 메소드이다.

자바스크립트는 싱글 스레드로 동작하기 때문에 두 가지 이상의 태스크를 동시에 실행할 수 없다. 이런 이류로 타이머 함수는 **비동기 처리 방식**으로 동작한다.

# 41.2 타이머 함수

## 41.2.1 setTimeout / clearTimeout

`setTimeout` 함수는 두 번째 인수로 전달받은 시간(ms)으로 단 한 번 동작하는 타이머를 생성한다. 이후 타이머가 완료되면 첫 번째 인수로 전달받은 콜백 함수가 호출된다.

```jsx
const timeout = setTimeout(func|code[, delay, param1, pram2....]);
```

| 매개 변수                                                | 설명                                                                               |
| -------------------------------------------------------- | ---------------------------------------------------------------------------------- |
| func                                                     | 타이머가 만료된 뒤 호출될 콜백 함수                                                |
| delay                                                    | 타이머 만료 시간(밀리초(ms) 단위), 생략할 경우 기본 값 0이 지정된다.               |
| \* delay가 4ms 이하인 경우 최소 지연시간 4ms가 지정된다. |
| param1, param2, ...                                      | 콜백 함수에 전달해야 할 인수가 존재하는 경우 세 번째 이후의 인수로 전달할 수 있다. |

```jsx
// 1초 후 콜백 함수 호출
setTimeout(() => console.log("Hi"), 1000);

// 콜백 함수에 인수로 'Lee' 전달
setTimeout((name) => console.log(`Hi ${name}`), 1000, "Lee");
```

`setTimeout` 함수는 생성된 타이머를 식별할 수 있는 고유한 타이머 id를 반환한다. 브라우저 환경에서는 숫자이며 Node.js에서는 객체이다.

<p align='center'>
<img width = '400px' src='https://bl3302files.storage.live.com/y4mR_jNEkMMme6RzNRzJnkvv182sVlJ4Sv_F9r-Uqxy3bv9_rJsPf6YaF8jfOGi1YtcgImHkwo_WMmL3xnlrtOiPB-ZbZmWc11z8jDzSxphu2-eZcEgvJAcKcVJDKFyuLX2wXOq1zG79hyvTPgAgc5J2W5bF9p-RtAw7ImQpO-KNcIn5WP6bJBr2Gtj6husoAQ2?width=794&height=70&cropmode=none'>
</p>

<p align='center'>
<img width = '300px' src='https://bl3302files.storage.live.com/y4mAnOWpa-BPUOeYUPcOfZNG-a0NY613pQtIZsCDK6_0k1weUnoCptb1ZYIXtcuR2zalXcNcrQB5jppe7nNtQIjmoKXI53K1JvtIBbRnSl-Iu6JEIwH6CqvpZw7ZN8uLqOUjr_2chNS-WmDhQYtBT5l5-G2HsOwrnjzBtv5xo_bKY6apznJOTvr3l5vpeoJkTk4?width=806&height=749&cropmode=none'>
</p>

타이머 id를 `clearTimeout`함수의 인수로 전달하여 타이머를 취소할 수 있다.

```jsx
const timerId = setTimeout((name) => console.log(`Hi ${name}`), 1000, "Lee");
clearTimeout(timerId);
```

## 41.2.2 setInterval / clearInterval

`setInterval` 함수는 두 번째 인수로 전달받은 시간(ms)으로 반복 동작하는 타이머를 생성한다. 타이머가 만료될 때마다 첫 번째 인수로 전달받은 콜백 함수가 반복 호출된다. 인수는 `setTimeout` 함수와 동일하다.

```jsx
const timerId = setInterval(func|code[,delay, param1, param2, ...]);
```

`setInterval` 함수도 마찬가지로 생성된 타이머를 식별할 수 있는 고유한 타이머 id를 반환한다. 브라우저에서는 숫자, Node.js에서는 객체이다.

`clearInterval` 함수에 id를 전달하여 타이머를 취소할 수 있다.

```jsx
const timerId = setInterval((name) => console.log(`Hi ${name}`), 1000, "Lee");
clearInterval(timerId);
```

# 41.3 디바운스와 스로틀

`scroll`, `resize`, `input`, `mousemove` 같은 이벤트는 짧은 시간 연속적으로 발생하며 이러한 이벤트에 바인딩된 이벤트 핸들러는 과도하게 호출되어 성능에 문제를 일으킬 수 있다.

디바운스와 스로틀은 짧은 시간 간격으로 연속해서 발생하는 이벤트를 그룹화해서 과도한 이벤트 핸들러의 호출을 방지하는 프로그래밍 기법이다.

다음 예제를 통해 디바운스, 스로틀된 이벤트 핸들러의 호출 빈도가 어떻게 다른지 살펴보자. 클릭 버튼을 빠르게 눌러보자.

[https://codepen.io/4anghyeon/pen/WNMezGN](https://codepen.io/4anghyeon/pen/WNMezGN)

## 41.3.1 디바운스

디바운스는 짧은 시간 간격으로 이벤트가 연속해서 발생하면 이벤트 핸들러를 호출하지 않다가 일정 시간이 경과한 이후 이벤트 핸들러가 한 번만 호출되도록 한다.

예를 들어 input 이벤트가 짧은 시간 연속으로 발생한다고 해보자.

```jsx
<!DOCTYPE html>
<html>
<body>
  <input type="text">
  <div class="msg"></div>
  <script>
    const $input = document.querySelector('input');
    const $msg = document.querySelector('.msg');

    const debounce = (callback, delay) => {
      let timerId;
      // debounce 함수는 timerId를 기억하는 클로저를 반환한다.
      return event => {
        // delay가 경과하기 이전에 이벤트가 발생하면 이전 타이머를 취소하고
        // 새로운 타이머를 재설정한다.
        // 따라서 delay보다 짧은 간격으로 이벤트가 발생하면 callback은 호출되지 않는다.
        if (timerId) clearTimeout(timerId);
        timerId = setTimeout(callback, delay, event);
      };
    };

    // debounce 함수가 반환하는 클로저가 이벤트 핸들러로 등록된다.
    // 300ms보다 짧은 간격으로 input 이벤트가 발생하면 debounce 함수의 콜백 함수는
    // 호출되지 않다가 300ms 동안 input 이벤트가 더 이상 발생하면 한 번만 호출된다.
    $input.oninput = debounce(e => {
      $msg.textContent = e.target.value;
    }, 300);
  </script>
</body>
</html>
```

<p align='center'>
<img width = '400px' src='https://bl3302files.storage.live.com/y4meOZVsdoptJI9NtZRwcu-s_aVoFJzUOisyDRTNHXdfgHHBjQE9eq3wekHFr4krxs_yxLdF2qJfIYb0qKQXSOMbPa-18DE_iXyykUDpJZ1RAXdrdMco1XtVXXYLsOcQP6rUAf8sVijdKKUlRanyftUtuNQXJVft-9q23EXCsLolTMBAJsAhfJzSh8R06eUAO_6?width=1062&height=316&cropmode=none'>
</p>

이처럼 이벤트의 간격이 일정 이상이 됐을 때 한 번 동작되도록 하는게 디바운스다.

디바운스는 `resize` 이벤트 처리나, `input` 요소의 입력 필드 자동완성, 버튼 중복 클릭 방지 처리 등에 유용하게 사용된다.

실무에서는 Underscore의 `debounce` 함수나 Lodash의 `debounce` 함수를 사용하는 것을 권장한다.

## 41.3.2 스로틀

스로틀은 짧은 시간 연속해서 이벤트가 발생허더라도 일정 시간 간격으로 이벤트 핸들러가 최대 한 번만 호출되도록 한다. 즉, 스로틀은 짧은 시간 간격으로 발생하는 이벤트를 시간 단위로 그룹화하여 이벤트 핸들러가 호출되도록 호출 주기를 만든다.

예를 들어, `scroll` 이벤트가 짧은 시간 간격으로 연속해서 발생하는 경우를 살펴보자.

[https://codepen.io/4anghyeon/pen/zYROWdd](https://codepen.io/4anghyeon/pen/zYROWdd)

짧은 시간 간격으로 연속해서 발생하는 이벤트의 과도한 이벤트 호출을 방지하기 위해 `throttle` 함수는 이벤트를 글부화해서 일정 시간 단위로 호출되도록 호출 주기를 만든다.

<p align='center'>
<img width = '400px' src='https://bl3302files.storage.live.com/y4mnzE7F_GrwTmZQdNIKCCn4aBcbzup0vhpHKOzww5OKfEL4QnyPsEMJ9dudePncr39fOmvCpug-CjFNJwLZUTvR2KvppudnKbAKkbncFntzSbSmeHcOHtjpuMxpba3Ulull9ZtwQ_dsVTV-sn27Me1nVn6tRG3rOdm0uByKTKPjBOXDM-mHa0W8e2D9wlyMDxK?width=1084&height=320&cropmode=none'>
</p>

이처럼 일정 시간 간격으로 이벤트 핸들러를 호출하는 스로틀은 `scroll` 이벤트 처리나 무한 스크롤 구현 등에 유용하게 사용된다.

실무에서는 Underscore의 `throttle` 함수나 Lodash의 `throttle` 함수를 사용하는 것을 권장한다.
