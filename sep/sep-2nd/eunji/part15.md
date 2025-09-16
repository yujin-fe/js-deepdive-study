# 15장 let, const 키워드와 블록 레벨 스코프
> JavaScript에서 변수를 선언하는 `var`, `let`, `const` 키워드의 특징과 차이점을 다룹니다. 특히, ES6에서 도입된 `let`과 `const`가 기존 `var` 키워드의 문제점을 어떻게 해결하는지에 초점을 맞춥니다.

### 🎯 QUIZ TIME
문제 1
```
javascriptconsole.log(x); // ?
var x = 5;

console.log(y); // ?
let y = 10;
```
> var x는 호이스팅되면서 undefined로 초기화됨
let y는 호이스팅되지만 TDZ(일시적 사각지대)에 있어서 참조 불가

문제 2

```
const user = { name: 'Alice', age: 25 };
user.age = 30;
console.log(user); // ?
```
> { name: 'Alice', age: 30 }
const는 변수의 참조(주소)만 고정함, 객체의 내용(프로퍼티)은 변경 가능

## 15.1 var 키워드로 선언한 변수의 문제점
> `var` 키워드는 다음과 같은 문제점을 가지고 있습니다:
*   **변수 중복 선언 허용**: `var`는 같은 이름으로 변수를 여러 번 선언할 수 있어, 예상치 못한 값으로 덮어쓰여 프로그램 오류를 유발할 수 있습니다.
*   **함수 레벨 스코프**: `if` 문이나 `for` 문과 같은 블록 내에서 선언된 `var` 변수도 블록 외부에서 접근 가능하여, 코드의 예측 가능성을 떨어뜨리고 잠재적 오류를 발생시킵니다.
*   **변수 호이스팅**: `var` 변수는 선언 이전에 참조하면 `undefined`로 초기화되는 문제가 있습니다. 이는 코드의 가독성을 해치고 오류를 찾기 어렵게 만듭니다.

## 15.2 let 키워드
> `let` 키워드는 `var`의 단점을 보완하기 위해 ES6에서 도입되었습니다.
*   **변수 중복 선언 금지**: `let`은 동일한 이름의 변수 중복 선언을 허용하지 않아 `SyntaxError`를 발생시켜 잠재적 오류를 방지합니다.
*   **블록 레벨 스코프**: `let`으로 선언된 변수는 `if`, `for`, `while` 등 모든 코드 블록 내에서만 유효하며, 블록 외부에서는 접근할 수 없습니다. 이는 보다 예측 가능하고 안전한 코드 작성을 가능하게 합니다.
*   **변수 호이스팅**: `let` 변수는 `var`와 달리 호이스팅 시 **일시적 사각지대(TDZ: Temporal Dead Zone)** 에 놓여, 선언 이전에 참조하면 `ReferenceError`가 발생합니다. 이는 `undefined`로 초기화되는 `var`의 문제점을 해결합니다.
*   **전역 객체와 let**: `let`으로 선언된 전역 변수는 `window`와 같은 전역 객체의 프로퍼티가 되지 않습니다.

## 15.3 const 키워드
`const` 키워드는 상수를 선언하는 데 사용되며, `let`과 유사하게 블록 레벨 스코프를 가지며 호이스팅 시 TDZ가 적용됩니다.
*   **선언과 초기화**: `const` 변수는 **선언과 동시에 초기화**해야 합니다. 초기화하지 않으면 `SyntaxError`가 발생합니다.
*   **재할당 금지**: `const`로 선언된 변수는 **재할당이 금지**되며, 재할당을 시도하면 `TypeError`가 발생합니다.
*   **상수**: `const`의 주된 목적은 변경 불가능한 값을 나타내는 상수를 선언하는 것입니다.
*   **const 키워드와 객체**: `const`로 객체를 선언한 경우, 변수 자체에 대한 재할당은 금지되지만, **객체 내부의 프로퍼티는 변경 가능**합니다. 즉, 객체의 참조(메모리 주소)는 불변이지만, 객체의 내용은 변경될 수 있습니다.

## 15.4 var vs. let vs. const
JavaScript에서 변수를 선언할 때는 **`var` 사용은 권장되지 않으며**, 재할당이 필요한 변수는 `let`으로, 재할당이 필요 없는 상수에는 `const`를 사용하는 것이 바람직합니다. 이는 코드의 가독성, 예측 가능성, 유지보수성을 크게 향상시킵니다.


### 전역 객체 바인딩 상세
```
javascript// 브라우저 환경에서
window.var변수    // ✅ 접근 가능
window.function선언  // ✅ 접근 가능  
window.let변수    // ❌ undefined
window.const변수  // ❌ undefined
```
---

### 🎯 QUIZ TIME 
```
var a = 1;
let b = 2; 
const c = { value: 3 };

console.log(window.a); // ?
console.log(window.b); // ?
console.log(window.c); // ?

c.value = 30;

console.log(c.value); // ?

c = { value: 300 }; // ?
```
> 1,
undefined,
undefined,
30,
TypeError