# 9월 1주차 스터디 


## 숙제 내용 및 정리 

var 는 전역 선언시 window에 붙는다
Let, const 는 전역 선언 시 어디에 붙을까?

Hint. 랙시컬

```

 var -> 전역에 선언 -> windows 객체에 붙음 

 let, const -> 전역에 선언 -> 렉시컬 환경에만 존재

 ```

 ``` 
 렉시컬 환경??

 우리가 {} 코드 블록이나 함수를 작성하고 실행할 때, 
 
 자바스크립트 엔진이 알아서 
 
 변수를 저장하고 스코프 체인을 유지할 수 있도록 
 
 내부 메모리 공간(렉시컬 환경)**을 
 
 자동으로 만들어 준다는 뜻
```

### 뭔소리지? 고민끝에 이렇게 결론을 지어 봤습니다 

+ 집 (JS 엔진) <br>
  → 집안에 있는 거니까 다른 방에서도 활용 가능 <br>
  → 실행할 때 자동으로 모든 방(코드 블록/함수)을 관리 

+ 방 (코드 블록/함수/전역)
  → 문을 닫아도 방 안에 서랍장은 존재

+ 서랍장 (렉시컬 환경) <br>
  → 방마다 하나씩 있고, 변수와 함수 선언이 다 이 서랍에 들어감

+ 서랍 내용물 (Environment Record) <br>
  → 실제로 저장된 변수 값들 (let, const, var, 함수 등)

+ 옆방 문 (Outer Reference) <br>
  → 방 안의 서랍에서 찾을 수 없으면, 문을 열고 바깥 방 서랍을 확인
   → 이게 바로 스코프 체인

## 질의내용 

자바스크립트에서 '10'+2 와 10-'2' 는값이 똑같아? 다르다면 왜 다른결과일까?

+ +연산자는 숫자덧셈뿐만 아니라 문자열 연결기능이 있기에 

+ 피연산자 중 하나가 문자열이면 다른쪽도 문자열로 변환해서 이어붙임

+ -연산자는 순수하게 숫자연산만 가능 

객체와 빈 배열이 아닌 배열은 undefined는 왜 변환되지 않을까?? NaN 값으로 출력됨

## 발표내용 정리

### p.72 동적 타입언어 
+ 자바스크립트의 변수는 선언이 아닌 할당에 의해 타입이 결정된다 
+ 재할당에 의해 변수의 타입은 언제든지 동적으로 변할 수 있다 



```
javascript

 let a ;  // 선언했지만 타입은 없음 (typeof) 사용시 undefined 출력

 a = 21;    // number
 a = "기훈" // string           //할당하는 순간 타입이 정해짐
 a = false  // boolean
 
```

## 재할당에 의해 변수 타입은 변화할수 있다 

### 변수는 유동성이 좋은대신 타입을 잘못예측해 오류발생률이 높다

+ 스코프의 범위를 최대한 좁게 만들어서 관리

+ 전역변수 사용 권장x

+ 변수보다는 상수의 값을 사용 ex) const 

+ 네이밍 잘하기 목적과 의미를 파악하기 쉽게 



# 연산자

### p.78 문자열 연결 연산자 

```
'1'+ 2 = '12'

// 산술 연산자 

1 + true = 2  // true 는 1로 변환

1 + false = 1 // false 는 0으로 변환

1 + numll = // null 은 0으로 변환
```

### p. 84 삼항 조건 연산자 

+  조건식의 평가 결과(true ,false ) 에 따라 반환값을 결정

+ true 일경우 ( : ) 기준 왼쪽 값을 false의 경우 오른쪽 값을 출력

``` 
ex)

let result = score >= 70 ? 'pass' : 'fail'

score = 80 // 변수 선언 (호이스팅되서 위로 올라감)

// 출력결과
console.log(result) 'pass'
```
### p.99 switch 문 

+ 다양한 상황에 따라 실행할 코드블럭을 결정할때 사용

+ break 문의 필수성 (코드 블록 탈출용 case 문의 표현식과 일치한다면 탈출)


### For 반복문

```
ex) 

for(let i = 1; i<=6; i++){
    for(let a =i; a<=6 a++){
        if( i + a === 6)
        console.log(`[${i}, ${a}]`)
    }
}
```

### 객체 리터럴

자바스크립트는 프로토타입기반 객체지향 언어? 

+ 모든 객체는 내부적으로 **[[Prototype]] (또는 __proto__)**라는 숨겨진 링크를 가지고 있음

+ 객체는 “다른 객체”를 상속(prototype chain) 받아서 만들어짐

+ 객체에서 어떤 속성이나 메서드를 찾을 때:

자기 자신 안에 있으면 사용

없으면 [[Prototype]]을 따라 올라가며 찾음 → 이 과정을 **프로토타입 체인(prototype chain)**이라고 함


## 1. 프로토타입(Prototype)란?

+ 자바스크립트 객체는 다른 객체를 상속할 수 있음
+ 상속 대상이 되는 객체를 프로토타입 객체라고 함
+ 모든 객체는 내부적으로 [[Prototype]] 링크를 가지고 있음

```
const obj = { name: "codeit" };
console.log(obj.__proto__); // Object.prototype
```

## 2. 프로토타입 체인(Prototype Chain) 여기가 발표자료

+ 객체에서 속성을 찾을 때,

  1. 객체 자신에 속성이 있는지 확인
  2. 없으면 [[Prototype]]을 따라가며 검색
  3. Object.prototype까지 없으면 undefined


```
  function Person(name) {
  this.name = name;
}
Person.prototype.sayHello = function() {
  console.log(`안녕, 나는 ${this.name}`);
};

const p1 = new Person("기훈");
p1.sayHello(); // Person.prototype에 있는 메서드 호출
```

4. Prototype과 Class

+ ES6 이후: class 문법 도입

```
class Person {
  constructor(name) { this.name = name; }
  sayHello() { console.log(`안녕, 나는 ${this.name}`); }
}
```

5. 핵심 정리

   1. JS는 프로토타입 기반 객체지향 언어

   2. 모든 객체는 [[Prototype]] 링크를 가지고 있음

   3. 객체에서 속성/메서드를 찾을 때 프로토타입 체인을 따라 검색

   4. 메서드 공유로 메모리 효율적

   5. ES6 class는 문법적 설탕, 내부는 프로토타입





### 프로퍼티 접근법

+ .(점) 표기법 
+  [...] 대괄호 표기법

```
ex

let person = {
    name: 'gihun'
}

console.log(person.name) // 'gihun'

// 대괄호 표기법
console.log (person['name']) // 'gihun'

대괄호 표기법은 내부에 지정하는 프로퍼티 키는 반드시 따옴표를 감싼 문자열로 반환
