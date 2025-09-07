# 9장 타입 변환과 단축 평가

## Quiz

```javascript
"10" + 2        // 결과는?
true && "Hello" // 결과는?
```

이런 결과가 나오는 이유를 알고 계신가요?
> JavaScript의 **타입 변환**과 **단축 평가** 때문입니다!

---

## 2. 명시적 타입 변환 (=타입 캐스팅)

> 개발자가 의도적으로 타입을 변환하는 것

### 주요 변환 함수
- `String()` - 문자열로 변환
- `Number()` - 숫자로 변환  
- `Boolean()` - 불린으로 변환
- `parseInt()`, `parseFloat()` - 문자열 => 숫자로 변환
- `toString()` - 문자열 변환

### 핵심 포인트
> **의도가 명확**하고 **예측 가능**한 변환

---

## 3. 암묵적 타입 변환 (=타입 강제 변환)

> JavaScript 엔진이 자동으로 타입을 변환하는 것

### 주요 변환 규칙

#### 문자열 변환
+ 문자열 연결 연산자 이용
```javascript
1 + "2"          // "12" (숫자 → 문자열)
true + ""        // "true"
null + "값"      // "null값"
```

#### 숫자 변환
+ 단항, 상술 연산자 이용
```javascript
"10" - 2         // 8 (문자열 → 숫자)
"5" * 3          // 15
+"42"            // 42 (단항 연산자)
```

#### 불린 변환
+ 부정논리연산자 이용
```javascript
!0               // true
!!""             // false
!!"hello"        // true
```

### 주의사항
❌ **예측하기 어려운 경우**
```javascript
[] + {}          // "[object Object]"
{} + []          // 0 (브라우저에 따라 다름)
"2" + 1 + 1      // "211" 
2 + 1 + "1"      // "31"
```

### 실무 권장사항
- 가능한 한 **명시적 변환** 사용
- 암묵적 변환이 필요하다면 **주석** 추가

---

## 4. 단축 평가

> 논리 연산자가 결과를 확정하는 순간 평가를 멈추는 것

### 논리 AND (&&)
```javascript
true && "Hello"   // "Hello" (뒤 값 반환)
false && "Hello"  // false (앞 값 반환)
"Cat" && "Dog"    // "Dog" (마지막 truthy 값)
```

### 논리 OR (||)
```javascript
false || "Hello"  // "Hello" (첫 번째 truthy 값)
null || "World"   // "World"
"A" || "B"        // "A" (첫 번째 truthy 값)
```

### 실무 활용 패턴

#### 기본값 설정
```javascript
const name = userName || "Guest";
const config = userConfig || defaultConfig;
```

#### 조건부 실행
```javascript
isLogin && showDashboard();
hasPermission && executeAction();
```

#### 안전한 객체 접근 (ES2020 이전)
```javascript
const city = user && user.address && user.address.city;
```

### S2020 이후 도입된 옵셔널 체이닝
```javascript
// Nullish coalescing (ES2020)
const name = userName ?? "Guest";

// Optional chaining (ES2020)  
const city = user?.address?.city;
```

---

## 5. 정리 및 마무리

### 핵심 요약
1. **명시적 변환**: 개발자 의도 → 예측 가능 ✅
2. **암묵적 변환**: JS 엔진 자동 → 주의 필요 ⚠️
3. **단축 평가**: 조건부 실행과 기본값 설정에 유용 🔄

### 실무 가이드라인
- 타입 변환은 **명시적으로** 하기
- 암묵적 변환 활용 시 **주석** 달기  
- 단축 평가로 **간결한 코드** 작성하기
- 최신 문법(`??`, `?.`) 활용하기

### 마무리 퀴즈 🤔
```javascript
1. "false" && true  // 결과는?
2. 0 || null || "Hi" // 결과는?
3. console.log(true + 1); // 결과는?
4. console.log(false + "1"); // 결과는?
```

<details>
<summary>정답 보기</summary>

```javascript
1. true  //  "false"는 truthy이므로 뒤 값 반환
2. Hi // 첫 번째 truthy 값
3. 2 //true → 1
4. "false1" //false → "false"
```
</details>
