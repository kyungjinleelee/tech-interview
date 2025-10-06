# JavaScript 비동기 처리(Promise, async/await)와 콜백 함수

## 콜백 함수(Callback Function)
- 함수의 인자로 전달되어 특정 이벤트 이후 실행되는 함수.
- 비동기 처리에서 대표적으로 사용됨(예: setTimeout, 이벤트 리스너 등).
- **단점:** 콜백이 중첩될수록 코드가 복잡해지고 가독성이 떨어짐(“콜백 지옥”).

```javascript
setTimeout(function() {
  console.log("비동기 작업 완료!");
}, 1000);
```

## Promise
- 비동기 작업을 효율적으로 처리할 수 있게 해주는 ES6 문법.
- 성공(resolve) 또는 실패(reject) 상태를 표현하며, `.then()`, `.catch()`로 결과 처리.
- 콜백과 달리 state(대기, 이행, 거부)를 가짐.

```javascript
const myPromise = new Promise((resolve, reject) => {
  setTimeout(() => {
    resolve("완료!");
  }, 1000);
});
myPromise.then(result => console.log(result));
```

## async/await
- Promise를 더욱 간결하고 동기적으로 보이게 작성할 수 있는 ES2017 문법.
- `async` 함수 내에서 `await` 키워드로 프로미스가 처리될 때까지 대기.
- 코드 흐름이 간단해짐.

```javascript
async function run() {
  const result = await myPromise;
  console.log(result);
}
run();
```

## 비교 요약

|                | 콜백 함수            | Promise           | async/await      |
|----------------|---------------------|-------------------|------------------|
| 도입 시기      | 오래됨              | ES6(2015)         | ES8(2017)        |
| 가독성         | ❌ (중첩→콜백지옥)  | ⭕                | ⭕⭕              |
| 에러 처리      | 어렵고 복잡         | .catch() 사용     | try/catch 사용   |
| 코드 흐름      | 복잡                | 명확해짐          | 거의 동기 방식   |

---

### 참고 자료
- [MDN - Callbacks](https://developer.mozilla.org/en-US/docs/Glossary/Callback_function)
- [MDN - Promise](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Promise)
- [MDN - async/await](https://developer.mozilla.org/ko/docs/Learn/JavaScript/Asynchronous/Async_await)
