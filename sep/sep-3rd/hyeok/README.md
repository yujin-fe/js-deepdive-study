# Object.prototype VS Object.**proto**

## Object.prototype

생성자 함수가 가지는 프로퍼티.  
기본으로 `constructor` 프로퍼티를 가지고 있고, 값으로 자기 자신을 가르킨다.

```js
let a = {};
console.log(a.prototype); //{}; 열어보면 constructor라는 프로퍼티를 가지고 있다.
```

`new` 키워드로 생성자 함수를 호출하면, 생성자 함수 내부에서 `prototype`과 같은 객체를 생성,  
`this`와 바인딩한다.

```js
function a(name) {
  // a.prototype === this 실제로 같은 객체는 아님
  this.name = name;
  this.sayName = function () {
    console.log(this.name);
  };
}

a.prototype.sayHi = () => console.log("Hi!"); // {}

const b = new a("혁");
b.sayName(); // "혁"
b.sayHi(); // "Hi!"
```

쉽게 말해 미리 가지고 있는 객체 설계도이며, 생성자 함수로 생성한 객체는 이 설계도를 바탕으로 만들어진다.

## Object.**proto**

그리고 `__proto__`는 해당 설계도를 가르킨다.

```js
function a(name) {}

a.prototype.sayHi = () => console.log("Hi!");

const b = new a("혁");
b.__proto__ === a.prototype; // true
```

b객체는 생성자함수 a의 설계도를 바탕으로 만들어졌기 때문에,  
b객체의 `__proto__`는 생성자함수 a의 `prototype`과 완전히 동일하다.
