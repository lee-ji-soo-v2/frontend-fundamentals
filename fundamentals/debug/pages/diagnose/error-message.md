# 에러 메시지로 진단하기

에러 메시지는 문제의 범위를 빠르게 좁힐 수 있는 가장 강력한 힌트예요.
에러를 진단할 때는 단순히 메시지를 보는 것에 그치지 않고, 해당 메시지가 **무엇을 의미하는지**, **어떤 맥락에서 발생했는지**, **어떤 가능성부터 확인하면 좋은지** 차근히 살펴보는 게 중요해요.

## 문법 오류 (`SyntaxError`)

코드에 문법적인 문제가 있을 때는 보통 `"SyntaxError: ~"`로 시작하는 에러 메시지가 출력돼요. 이 메시지는 자바스크립트 엔진이 **코드를 실행하기도 전에 문법을 해석하다가 실패했음을 알려주는 신호**예요.

예를 들어, 괄호가 닫히지 않았거나, `export`나 `return` 같은 예약어를 잘못 썼을 때 아래와 같은 메시지를 볼 수 있어요.
```tsx 5
function run() {
  const name = 'Hello'
  if (true) {
    console.log(name)

```
```
SyntaxError: Unexpected token '{'
```

<br/>
작은따옴표와 큰따옴표의 매칭이 잘못된 경우엔 이런 메세지가 보여요.

```tsx
JSON.parse('{ foo: "bar" }");
```
```
SyntaxError: The string did notmatch
the expected pattern.
```

### 확인할 것
- 문법에 맞게 작성되었는지 확인해요
- eslint rule을 적용해 문법오류를 쉽게 잡을 수 있도록 환경을 세팅해요.

## 모듈 import 오류

모듈을 import하는 과정에서도 문법 오류(`SyntaxError`)가 발생할 수 있어요. 특히 ES 모듈(ESM)과 CommonJS(CJS) 방식이 혼합된 환경에서는 설정이 어긋나기 쉽고, 이로 인해 문법 오류처럼 보이는 에러가 나타날 수 있어요.

예를 들어, 다음과 같은 메시지를 보면 단순 문법 문제가 아니라 **모듈 시스템 설정에 문제가 있을 가능성**을 유추할 수 있어요:
```
SyntaxError: Cannot use import statement outside a module
```

### 확인할 것 
- 프로젝트의 모듈 시스템 설정이 올바른지 확인해요
  - ESM 사용 시: `package.json`에 `"type": "module"` 설정을 확인해요
  - CommonJS 사용 시: `"type": "commonjs"` (기본값) 또는 설정을 제거해요
- `.mjs`, `.cjs`, `.js` 확장자가 적절히 사용됐는지 확인해요
- 잘못된 번들 경로로 `esm` 전용 모듈을 가져오지 않았는지 확인해요


## 타입 오류 (`TypeError`)

값의 타입이 예상과 다를 때는 `"TypeError: ~"`로 시작하는 에러 메시지가 출력돼요. 이 에러는 주로 **정의되지 않았거나 잘못된 값에 접근하거나, 함수가 아닌 값을 함수처럼 호출했을 때** 발생해요.

예를 들어 객체가 `null`이나 `undefined`인 상태에서 속성에 접근하려고 하면 이런 메시지를 볼 수 있어요.
```tsx 2
const user = null;
console.log(user.name);
```
```
TypeError: Cannot read property 'name' of null  
```


함수가 아닌 객체를 호출하면 다음과 같은 에러 메세지가 나요
```tsx 2
const num = 42;
num();

```
```
TypeError: num is not a function
```

### 확인할 것
- 객체가 실제로 존재하는지 확인해요
- API 응답 데이터 구조가 맞는지 확인해요 
- `await` 누락 여부를 확인해요
- `typeof`, `Array.isArray()` 등으로 미리 검사했는지 확인해요


## 참조 오류 (`ReferenceError`)

`ReferenceError`는 **정의되지 않은 식별자(변수나 함수 이름 등)를 사용하려고 할 때** 발생해요. 즉, 자바스크립트 엔진이 해당 이름을 찾을 수 없을 때 나타나는 에러예요.

예를 들어, 아래처럼 변수를 선언하지 않고 사용하면 에러가 발생해요.

```tsx 1
console.log(userName);
let userName = 'Alice';

```
```
ReferenceError: userName is not defined
```

### 확인할 것 
- 변수가 선언되었는지 확인해요
- 선언보다 먼저 접근한 건 아닌지 확인해요
- 외부 스코프 참조가 의도한 것인지 확인해요

## 리소스 로딩 오류

외부 자원을 불러오지 못할 때 주로 발생해요.
```tsx 3
fetch('https://api.example.com/data')
  .then(res => res.json())
  .catch(err => console.error(err));

```
```
TypeError: Load failed
```

### 확인할 것
- 요청이 실패했는지 개발자 도구의 Network 탭에서 확인해요
- CORS 정책 위반 여부를 확인해요
- URL 오타나 네트워크 연결 문제를 확인해요 
- iframe 삽입 허용 여부 (`X-Frame-Options`)를 확인해요

---

### 📚 더 알아보기
- [MDN: SyntaxError](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/SyntaxError)
- [MDN: TypeError](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/TypeError)
- [MDN: ReferenceError](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/ReferenceError)
- [Node.js ESM 가이드](https://nodejs.org/api/esm.html)
- [CORS 이해하기](https://developer.mozilla.org/ko/docs/Web/HTTP/CORS)