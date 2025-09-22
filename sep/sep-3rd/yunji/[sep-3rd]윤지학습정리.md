# 📝 16장 ~ 17장 요약 정리

## 1. 객체 = 데이터 + 숨겨진 정보

자바스크립트에서 **객체**는 단순히 `{}`에 값만 담는 게 아니라,
엔진이 몰래 관리하는 **숨겨진 슬롯([[Prototype]], [[Call]] 등)** 과
**자동으로 실행되는 메서드( [[Get]], [[Set]])** 까지 포함된다.

```
객체 = { 프로퍼티들 } + [[슬롯]] + [[메서드]]
```

<br />

## 2. 함수 객체 = 특별한 객체

함수도 **객체**이다. 그런데 함수라서 특별히 두 가지 슬롯을 가질 수 있다.

1. **[[Call]]** → 함수처럼 실행 가능 (`f()`)
2. **[[Construct]]** → new 연산자로 실행 가능 (`new f()`)

👉 **모든 함수는 객체지만, 모든 함수가 생성자(new) 가능한 건 아님**.

<br />

## 3. 함수의 종류

### (1) callable 함수 (호출 가능한 함수)

- `[[Call]]` 슬롯을 가진 함수
- 그냥 `f()` 형태로 실행 가능

```javascript
function sayHi() {
  console.log("hi");
}
sayHi(); // 실행됨
```

### (2) constructor 함수 (생성자 함수 가능)

- `[[Call]]` + `[[Construct]]` 둘 다 있는 함수
- 즉, `f()`도 되고, `new f()`도 되는 함수

  #### → function 선언문, function 표현식, class

  #### → 즉, 보통 function 키워드로 만든 함수는 다 constructor

```js
function Foo() {} // ✅ constructor
const Bar = function () {}; // ✅ constructor
class Baz {} // ✅ constructor
```

### (3) non-constructor 함수 (생성자 불가)

- `[[Call]]`만 있고 `[[Construct]]` 없다
- 즉, 그냥 `f()`만 되고, new로 호출할 수 없다
  #### → 화살표 함수, 메서드 축약 표현

```javascript
const arrow = () => {}; // ❌ non-constructor
const obj = {
  hi() {}, // ❌ 메서드 축약형 → non-constructor
};
```

<br />

## 4. this 바인딩 원리

자바스크립트에서 `this는 "누가 호출했냐"에 따라 달라진다.`

1. 그냥 함수 호출 → 전역 객체(window/undefined)
2. 객체의 메서드로 호출 → 그 객체가 this
3. new로 생성자 호출 → 새로 만들어진 객체가 this
4. `bind`, `call`, `apply` 사용 → 지정한 값이 this

<br />

예시:

### 1. 그냥 함수 호출

```js
function sayHi() {
  console.log(this);
}

sayHi();
```

- 엄격 모드(strict mode) → undefined

- 비엄격 모드 → window (브라우저 환경에서 전역 객체)

#### 👉 아무도 주인이 없으니까, 기본적으로 전역 객체가 this가 됨.

<br />

### 2. 객체의 메서드로 호출

```js
let user = {
  name: "철수",
  greet: function () {
    console.log(this.name);
  },
};

user.greet(); // "철수"
```

- `user.greet()`라고 객체가 앞에 붙어서 호출되면 → 그 객체(`user`)가 `this`

#### 👉 "누가 불렀냐? → user가 불렀네 → this = user"

<br />

### 3. new로 생성자 호출

```js
function Person(name) {
  this.name = name;
}

let p1 = new Person("영희");
console.log(p1.name); // "영희"
```

- `new` 키워드로 부르면 → **새로운 객체가 자동으로 만들어지고** 그 객체가 this

- `this.name = name` 은 결국 `새로 만들어진 객체에 name을 붙이는 것.`

#### 👉 "new는 새로운 객체를 만들어서 this로 줌"

<br />

### 4. bind, call, apply 사용

```js
function sayHello() {
  console.log(this.name);
}

let user1 = { name: "민수" };
let user2 = { name: "지수" };

sayHello.call(user1); // "민수"
sayHello.apply(user2); // "지수"

let bound = sayHello.bind(user1);
bound(); // "민수"
```

- call, apply, bind는 this를 강제로 지정할 수 있는 방법

- `.call(user1)` 하면 → `this = user1`

- `.apply(user2)` 하면 → `this = user2`

- `.bind(user1)` 하면 → 새로운 함수가 만들어지고 그 안에서 항상 `this = user1`

#### 👉 "this를 내가 원하는 객체로 강제 고정시킬 수 있다

<br />

## 5. new 연산자의 동작

`new Foo()`가 실행될 때 실제로 일어나는 일:

1. **새 객체 생성** `{}`
2. 그 객체의 `[[Prototype]]`을 `Foo.prototype`으로 설정
3. `Foo` 함수 실행 (this = 새 객체)
4. `Foo` 안에서 객체를 명시적으로 return하면 그 객체 반환, 아니면 새 객체 반환

```js
function Foo() {
  this.value = 10;
}
const obj = new Foo();

console.log(obj); // Foo { value: 10 }
console.log(obj.__proto__ === Foo.prototype); // true
```

<br />
<br />
<br />

# + 추가로

## this 바인딩 규칙 (우선순위 중심)

this는 함수 호출 문맥(호출 방식)에 따라 결정된다. 일반적으로 우선순위는 다음과 같다:

### 1. new 바인딩(생성자 호출)

- `new F()` → this는 새로 생성된 객체(항상 객체). (가장 높은 우선순위)

### 2. 명시적 바인딩 (explicit binding)

- `f.call(obj, ...), f.apply(obj, ...)` → this는 obj로 고정.

- `f.bind(obj)`로 만든 바운드 함수는 내부적으로 [[BoundThis]]를 가진다.

### 3. 암시적(객체) 바인딩 (implicit / method call)

- `obj.m()` → this는 호출 시점의 베이스 객체(obj).

- 주의: `const fn = obj.m; fn();` 처럼 추출하면 암시적 바인딩 상실.

### 4. 기본 바인딩(default)

- 위 규칙에 해당하지 않으면:

  - 엄격 모드(strict): this === undefined

  - 비엄격(sloppy): this === globalThis (브라우저에선 window, 모던 환경에선 globalThis)

그리고 특수 규칙:

- 화살표 함수(arrow): this를 바인딩하지 않음. 선언 시점(렉시컬)의 this를 캡처(= lexical this). 따라서 call/apply/bind로 바꾸지 못함.

- 바운드 함수: bind로 만든 함수는 내부 슬롯에 [[BoundThis]]를 갖고 일반 호출에서 그 값이 사용됨. 다만 new로 호출하면(대상 생성자이면) new 우선권 때문에 바운드된 this는 무시될 수 있음(아래 예시 참조).

예시

```js
function show() {
  console.log(this);
}

const obj = { a: 1, show };

obj.show(); // implicit → this === obj
const f = obj.show;
f(); // default → this === undefined (strict) or globalThis (non-strict)

show.call({ x: 1 }); // explicit → this === {x:1}

const arrow = () => console.log(this);
arrow(); // lexical this (선언된 환경의 this)
```

우선순위 예시 (bind vs new):

```js
function Person(name) {
  this.name = name;
}
const Bound = Person.bind({ name: "BOUND" });

Bound("A"); // 일반 호출: 바운드된 this({name:'BOUND'})에 적용
new Bound("B"); // new 호출: new가 우선 → this는 새 객체, name === 'B'
```

요점: new로 생성하면 새 객체가 this가 되고, 바운드된 this는 무시됩니다(단, 내부 동작은 spec 처리가 약간 복잡하지만 사용자 입장에선 이렇게 이해하면 된다).

<br />

## prototype과 constructor 관계

(실무에서 자주 헷갈리는 부분)

- 함수 객체가 생성될 때(일반적인 함수) 자동으로 그 함수의 prototype 프로퍼티(객체)가 만들어진다. 이 prototype 객체에는 기본적으로 constructor 프로퍼티가 있고, 그 값은 다시 원래 함수(자기 자신)를 가리킨다.

```js
function Foo() {}
Foo.prototype.constructor === Foo; // true
```

- `new Foo()`로 만든 인스턴스의 내부 `[[Prototype]](a.k.a. __proto__)`는 Foo.prototype을 가리킨다.

주의: 프로토타입을 덮어쓰면(constructor 연결이 깨질 수 있음) constructor가 자동 복구되지 않으니 필요하면 수동으로 설정해야 함.

```js
Foo.prototype = { hello: function () {} }; // 이제 Foo.prototype.constructor !== Foo
Foo.prototype.constructor = Foo; // 복구
```
