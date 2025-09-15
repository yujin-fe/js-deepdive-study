# Common JS VS ES Modules

## 모듈 시스템은 어떻게 전역 변수를 억제하는가?

- 모듈 시스템은 각각의 모듈(파일)이 자체적인 스코프를 가지게 됩니다.
- 그 안에서 정의한 변수/함수는 기본적으로 전역에 노출되지 않습니다.
- 외부에서 쓰려면 export 해야 하고, 가져오려면 import 해야 합니다.
- 즉, 명시적으로 노출하지 않으면 숨겨집니다(캡슐화).

## Common JS

CommonJS는 Node.js 초기 표준입니다.  
`module.exports = {}` 같이 객체의 형태로 내보내며,  
런타임에 해석되어 모듈이 로딩됩니다.

- `require()`로 모듈을 불러옴 → 동기적 로딩
- 모듈 전체(`module.exports`)가 반환되므로 트리 셰이킹 불가
- 브라우저 환경에서는 기본적으로 지원되지 않아 번들러 필요
- 레거시 npm 패키지 상당수가 CommonJS 기반이라 호환성이 높음

```js
// math.js
function add(a, b) {
  return a + b;
}
function sub(a, b) {
  return a - b;
}
module.exports = { add, sub };

// app.js
const { add } = require("./math");
console.log(add(2, 3));
```

## ES Modules (ESM)

ES Modules는 ES2015(ES6)에서 도입된 공식 표준 모듈 시스템입니다.  
`export`, `import` 키워드를 사용하며,  
정적 분석을 통해 모듈 관계가 컴파일 단계에서 확정됩니다.

- `import` / `export` 문법으로 작성
- **정적 로딩** → 트리 셰이킹·코드 스플리팅 가능
- 비동기 동적 임포트(`import()`) 지원
- 브라우저와 Node.js에서 모두 지원 (Node.js는 `"type": "module"` 설정 필요)

```js
// math.js
export function add(a, b) {
  return a + b;
}
export function sub(a, b) {
  return a - b;
}

// app.js
import { add } from "./math.js";
console.log(add(2, 3));
```

### 정적로딩, 트리 셰이킹·코드 스플리팅

1. 정적로딩

**컴파일/파싱 단계에서 import/export가 고정**되어 있어서,  
실행 전에 어떤 모듈이 필요한지 미리 알 수 있는 방식.

```js
// app.js
import { add } from "./math.js";
```

- 실행 전에 이미 **math.js의 add 함수만 필요하다**는 걸 알 수 있음.

- 그래서 **트리 셰이킹**(안 쓰는 코드를 빼버리는 최적화)이 가능해짐.

- 반대로 CommonJS(require)는 런타임에 실행되므로 정적 분석이 힘듦

```js
const lib = require(someVar); // 실행해봐야 뭘 불러오는지 앎
```

2. 코드 스플리팅(Code Splitting)

앱 전체 코드를 하나로 번들하지 않고, 필요한 부분만 잘라서(스플릿) 로딩하는 최적화 기법.

(번들: 여러개의 JS, CSS, 이미지 등 리소스를 **하나의 파일(또는 몇 개의 큰 파일)**로 만듬)

보통 ESModules + 번들러(Webpack, Vite, Rollup 등)에서 지원.

초기에 꼭 필요한 코드만 로드하고, 나머지는 **lazy load(지연 로딩)**로 가져오기에 초기 로딩 속도가 빨라짐.

```js
// app.js
import { init } from "./core.js";

// 무거운 라이브러리 → 필요할 때만 불러옴
button.addEventListener("click", async () => {
  const { heavyFunc } = await import("./heavyLib.js");
  heavyFunc();
});
// 한 번 불러온 모듈은 캐싱돼서 이후에 다시 호출하더라도 캐싱된 모듈 객체를 그대로 반환함
```

초기 로딩 시 heavyLib.js는 안 불러옴.
버튼 클릭 순간에만 네트워크 요청 하기 때문에 효율적임.

## CommonJS VS ES Modules

| 구분          | CommonJS                    | ES Modules                     |
| ------------- | --------------------------- | ------------------------------ |
| 등장 배경     | Node.js 초기 모듈 표준      | ES2015(ES6) 공식 표준          |
| 구문          | `require`, `module.exports` | `import`, `export`             |
| 로딩 방식     | 런타임 로딩                 | 정적 로딩                      |
| 동작 방식     | 동기적                      | 비동기 지원                    |
| 트리 셰이킹   | 불가능                      | 가능                           |
| 코드 스플리팅 | 어려움                      | 용이                           |
| 지원 환경     | Node.js 기본                | 브라우저 & Node.js (설정 필요) |
