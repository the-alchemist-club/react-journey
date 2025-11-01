# const와 let

## jQuery 시대의 변수 선언

jQuery를 사용하던 시절에는 `var`로 모든 변수를 선언했습니다.

```javascript
var name = "홍길동";
var age = 25;
var isStudent = true;
```

## 왜 const와 let을 사용할까?

`var`는 몇 가지 문제점이 있습니다:
- 같은 변수를 여러 번 선언할 수 있어서 실수하기 쉽습니다
- 함수 스코프만 지원해서 블록 스코프가 없습니다
- 호이스팅(hoisting)으로 인한 예상치 못한 동작

현대 JavaScript에서는 `const`와 `let`을 사용합니다.

## const: 재할당 불가능한 변수

`const`는 한 번 할당하면 다시 변경할 수 없는 상수를 선언할 때 사용합니다.

```javascript
const PI = 3.14159;
const userName = "홍길동";

// 에러! 재할당 불가능
// PI = 3.14;
// userName = "김철수";
```

### 언제 사용할까?

- 변하지 않는 값 (상수)
- 함수
- 객체나 배열 (내용은 변경 가능)

```javascript
// 좋은 예: 함수는 const로 선언
const greet = (name) => {
  return `안녕하세요, ${name}님!`;
};

// 좋은 예: 객체는 const로 선언해도 속성 변경 가능
const user = {
  name: "홍길동",
  age: 25
};

user.age = 26; // OK! 객체의 속성은 변경 가능
// user = {}; // 에러! 재할당은 불가능
```

## let: 재할당 가능한 변수

`let`은 값을 변경할 수 있는 변수를 선언할 때 사용합니다.

```javascript
let count = 0;
count = 1; // OK!
count = 2; // OK!

let message = "안녕하세요";
message = "반갑습니다"; // OK!
```

### 언제 사용할까?

- 반복문의 카운터
- 조건에 따라 변경되는 값
- 상태가 변하는 변수

```javascript
// 좋은 예: 반복문 카운터
for (let i = 0; i < 5; i++) {
  console.log(i);
}

// 좋은 예: 조건에 따라 변하는 값
let status = "pending";

if (isSuccess) {
  status = "success";
} else {
  status = "error";
}
```

## 블록 스코프

`const`와 `let`은 블록 스코프를 가집니다. 중괄호 `{}` 안에서만 유효합니다.

```javascript
// var의 문제점
if (true) {
  var x = 10;
}
console.log(x); // 10 - 블록 밖에서도 접근 가능!

// const/let은 블록 스코프
if (true) {
  const y = 20;
  let z = 30;
}
// console.log(y); // 에러! 블록 밖에서 접근 불가
// console.log(z); // 에러! 블록 밖에서 접근 불가
```

## jQuery vs 현대 JavaScript 비교

### jQuery 스타일 (var 사용)

```javascript
// 클릭 카운터
var clickCount = 0;

$('#myButton').click(function() {
  clickCount++;
  $('#counter').text(clickCount);
});

// 문제: clickCount가 어디서든 변경될 수 있음
```

### 현대 JavaScript 스타일 (const/let 사용)

```javascript
// 클릭 카운터
let clickCount = 0;
const button = document.querySelector('#myButton');
const counter = document.querySelector('#counter');

button.addEventListener('click', () => {
  clickCount++;
  counter.textContent = clickCount;
});

// 장점: button과 counter는 const로 보호되어 실수로 재할당 불가
```

## 실전 팁

### 1. 기본적으로 const 사용

```javascript
// 좋은 습관
const apiUrl = "https://api.example.com";
const maxAttempts = 3;
const users = [];

// 나쁜 습관
let apiUrl = "https://api.example.com"; // 변경하지 않을 거면 const 사용
```

### 2. 변경이 필요할 때만 let 사용

```javascript
// let이 적절한 경우
let currentPage = 1;
let isLoading = false;

function nextPage() {
  currentPage++; // 값이 변경됨
}
```

### 3. var는 사용하지 않기

```javascript
// 피해야 할 코드
var oldStyle = "레거시 코드";

// 대신 이렇게
const modernStyle = "최신 코드";
```

## 연습 문제

다음 코드를 const 또는 let으로 다시 작성해보세요:

```javascript
// jQuery 스타일
var userName = "홍길동";
var userAge = 25;
var messages = [];

$('#updateAge').click(function() {
  userAge++;
  $('#age').text(userAge);
});

$('#addMessage').click(function() {
  var newMessage = $('#messageInput').val();
  messages.push(newMessage);
  displayMessages();
});
```

### 정답

```javascript
// 현대 JavaScript 스타일
const userName = "홍길동"; // 변경되지 않으므로 const
let userAge = 25; // 증가하므로 let
const messages = []; // 배열 자체는 변경되지 않으므로 const

const updateButton = document.querySelector('#updateAge');
const ageDisplay = document.querySelector('#age');

updateButton.addEventListener('click', () => {
  userAge++;
  ageDisplay.textContent = userAge;
});

const addButton = document.querySelector('#addMessage');
const messageInput = document.querySelector('#messageInput');

addButton.addEventListener('click', () => {
  const newMessage = messageInput.value; // 블록 내에서만 사용되므로 const
  messages.push(newMessage);
  displayMessages();
});
```

## 요약

| 구분 | const | let | var |
|------|-------|-----|-----|
| 재할당 | ❌ 불가능 | ✅ 가능 | ✅ 가능 |
| 재선언 | ❌ 불가능 | ❌ 불가능 | ✅ 가능 |
| 스코프 | 블록 | 블록 | 함수 |
| 사용 권장 | ✅ 기본 | ✅ 필요시 | ❌ 사용 금지 |

**핵심 원칙:**
1. 기본적으로 `const` 사용
2. 재할당이 필요하면 `let` 사용
3. `var`는 절대 사용하지 않기
