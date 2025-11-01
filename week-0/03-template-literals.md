# 템플릿 리터럴 (Template Literals)

## jQuery 시대의 문자열 연결

예전에는 `+` 연산자로 문자열을 이어 붙였습니다.

```javascript
var name = "홍길동";
var age = 25;
var message = "안녕하세요, " + name + "님! 나이는 " + age + "세입니다.";

// HTML 생성
var userId = 123;
var userName = "김철수";
var html = '<div class="user">' +
           '  <h2>' + userName + '</h2>' +
           '  <p>ID: ' + userId + '</p>' +
           '</div>';

$('#container').html(html);
```

이 방법은 불편하고 실수하기 쉽습니다:
- 따옴표와 `+` 기호를 계속 써야 함
- 여러 줄 문자열은 더 복잡함
- 가독성이 떨어짐

## 템플릿 리터럴이란?

백틱(`` ` ``)을 사용하여 문자열을 만드는 방법입니다. 변수를 `${}`로 쉽게 삽입할 수 있습니다.

```javascript
const name = "홍길동";
const age = 25;
const message = `안녕하세요, ${name}님! 나이는 ${age}세입니다.`;

console.log(message);
// 출력: 안녕하세요, 홍길동님! 나이는 25세입니다.
```

## 기본 사용법

### 1. 변수 삽입

```javascript
// 기존 방식
var product = "노트북";
var price = 1200000;
var text = product + "의 가격은 " + price + "원입니다.";

// 템플릿 리터럴
const product = "노트북";
const price = 1200000;
const text = `${product}의 가격은 ${price}원입니다.`;
```

### 2. 표현식 사용

```javascript
const a = 10;
const b = 20;

// 계산도 가능
console.log(`${a} + ${b} = ${a + b}`);
// 출력: 10 + 20 = 30

// 함수 호출도 가능
const getDiscount = (price) => price * 0.1;
const price = 50000;
console.log(`할인 금액: ${getDiscount(price)}원`);
// 출력: 할인 금액: 5000원

// 삼항 연산자도 가능
const age = 20;
const status = `이 사용자는 ${age >= 18 ? '성인' : '미성년자'}입니다.`;
```

### 3. 여러 줄 문자열

```javascript
// 기존 방식 - 불편함
var html = '<div class="card">\n' +
           '  <h2>제목</h2>\n' +
           '  <p>내용</p>\n' +
           '</div>';

// 템플릿 리터럴 - 간편함
const html = `
<div class="card">
  <h2>제목</h2>
  <p>내용</p>
</div>
`;
```

## jQuery vs 템플릿 리터럴

### HTML 생성 비교

```javascript
// jQuery 스타일
var users = [
  { id: 1, name: "홍길동", age: 25 },
  { id: 2, name: "김철수", age: 30 }
];

var html = '';
users.forEach(function(user) {
  html += '<div class="user-card">' +
          '  <h3>' + user.name + '</h3>' +
          '  <p>나이: ' + user.age + '</p>' +
          '  <button data-id="' + user.id + '">상세보기</button>' +
          '</div>';
});

$('#userList').html(html);
```

```javascript
// 현대 JavaScript 스타일 (템플릿 리터럴)
const users = [
  { id: 1, name: "홍길동", age: 25 },
  { id: 2, name: "김철수", age: 30 }
];

const html = users.map(user => `
  <div class="user-card">
    <h3>${user.name}</h3>
    <p>나이: ${user.age}</p>
    <button data-id="${user.id}">상세보기</button>
  </div>
`).join('');

document.querySelector('#userList').innerHTML = html;
```

### Ajax 응답 처리 비교

```javascript
// jQuery 스타일
$.ajax({
  url: '/api/product/' + productId,
  success: function(product) {
    var message = product.name + '을(를) 장바구니에 담았습니다. ' +
                  '현재 가격: ' + product.price + '원';
    alert(message);
  }
});

// 현대 JavaScript 스타일
fetch(`/api/product/${productId}`)
  .then(response => response.json())
  .then(product => {
    const message = `${product.name}을(를) 장바구니에 담았습니다. 현재 가격: ${product.price}원`;
    alert(message);
  });
```

## 실전 예제

### 1. 동적 클래스 이름

```javascript
const isActive = true;
const isPrimary = false;

// 조건부 클래스
const className = `btn ${isActive ? 'active' : ''} ${isPrimary ? 'primary' : 'secondary'}`;
console.log(className);
// 출력: btn active secondary
```

### 2. URL 생성

```javascript
const baseUrl = 'https://api.example.com';
const userId = 123;
const page = 2;

// 기존 방식
var url = baseUrl + '/users/' + userId + '/posts?page=' + page;

// 템플릿 리터럴
const url = `${baseUrl}/users/${userId}/posts?page=${page}`;
console.log(url);
// 출력: https://api.example.com/users/123/posts?page=2
```

### 3. 복잡한 HTML 컴포넌트

```javascript
const createCard = (product) => {
  const { id, name, price, imageUrl, inStock } = product;
  const discountPrice = price * 0.9;

  return `
    <div class="product-card" data-id="${id}">
      <img src="${imageUrl}" alt="${name}">
      <h3>${name}</h3>
      <div class="price">
        <span class="original">${price.toLocaleString()}원</span>
        <span class="discount">${discountPrice.toLocaleString()}원</span>
      </div>
      <p class="stock ${inStock ? 'in-stock' : 'out-of-stock'}">
        ${inStock ? '재고 있음' : '품절'}
      </p>
      <button ${!inStock ? 'disabled' : ''}>
        ${inStock ? '장바구니 담기' : '재입고 알림'}
      </button>
    </div>
  `;
};

const product = {
  id: 1,
  name: "무선 마우스",
  price: 35000,
  imageUrl: "/images/mouse.jpg",
  inStock: true
};

document.querySelector('#products').innerHTML = createCard(product);
```

### 4. 에러 메시지

```javascript
const showError = (field, error) => {
  const messages = {
    required: `${field}은(는) 필수 입력 항목입니다.`,
    minLength: `${field}은(는) 최소 ${error.min}자 이상이어야 합니다.`,
    email: `올바른 이메일 형식이 아닙니다.`
  };

  return messages[error.type] || '알 수 없는 오류가 발생했습니다.';
};

console.log(showError('비밀번호', { type: 'required' }));
// 출력: 비밀번호은(는) 필수 입력 항목입니다.

console.log(showError('비밀번호', { type: 'minLength', min: 8 }));
// 출력: 비밀번호은(는) 최소 8자 이상이어야 합니다.
```

## React에서의 활용

템플릿 리터럴은 React에서 매우 자주 사용됩니다.

```javascript
// JSX에서 동적 스타일
function Button({ variant, size }) {
  return (
    <button className={`btn btn-${variant} btn-${size}`}>
      클릭
    </button>
  );
}

// 조건부 렌더링
function UserGreeting({ user }) {
  return (
    <div>
      <h1>{`안녕하세요, ${user.name}님!`}</h1>
      <p>{`마지막 로그인: ${user.lastLogin}`}</p>
    </div>
  );
}

// API 호출
function fetchUser(userId) {
  return fetch(`/api/users/${userId}`)
    .then(res => res.json());
}
```

## 고급 기능: Tagged Templates

함수와 함께 사용할 수 있습니다 (고급 기능).

```javascript
// 간단한 예제
function highlight(strings, ...values) {
  return strings.reduce((result, str, i) => {
    return `${result}${str}<mark>${values[i] || ''}</mark>`;
  }, '');
}

const name = "홍길동";
const age = 25;
const message = highlight`이름: ${name}, 나이: ${age}`;
console.log(message);
// 출력: 이름: <mark>홍길동</mark>, 나이: <mark>25</mark>

// Styled Components에서 많이 사용됨 (React)
const Button = styled.button`
  background: ${props => props.primary ? 'blue' : 'gray'};
  color: white;
  padding: 10px 20px;
`;
```

## 주의사항

### 1. HTML 이스케이프

```javascript
// 사용자 입력은 XSS 공격에 취약할 수 있음
const userInput = '<script>alert("해킹!")</script>';
const html = `<div>${userInput}</div>`;

// 보안을 위해 이스케이프 필요
const escapeHtml = (text) => {
  const div = document.createElement('div');
  div.textContent = text;
  return div.innerHTML;
};

const safeHtml = `<div>${escapeHtml(userInput)}</div>`;
```

### 2. 공백과 들여쓰기

```javascript
// 템플릿 리터럴은 공백과 줄바꿈을 그대로 유지
const html = `
  <div>
    내용
  </div>
`;

console.log(html);
// 출력:
//   <div>
//     내용
//   </div>

// 앞뒤 공백 제거하려면 trim() 사용
const trimmed = html.trim();
```

## 연습 문제

다음 jQuery 코드를 템플릿 리터럴을 사용하여 다시 작성해보세요:

```javascript
var products = [
  { id: 1, name: "키보드", price: 80000, stock: 5 },
  { id: 2, name: "마우스", price: 35000, stock: 0 },
  { id: 3, name: "모니터", price: 450000, stock: 3 }
];

var html = '';
products.forEach(function(p) {
  var stockText = p.stock > 0 ? '재고: ' + p.stock + '개' : '품절';
  html += '<div class="product">' +
          '  <h4>' + p.name + '</h4>' +
          '  <p>가격: ' + p.price + '원</p>' +
          '  <p class="' + (p.stock > 0 ? 'available' : 'soldout') + '">' +
          stockText + '</p>' +
          '</div>';
});

$('#products').html(html);
```

### 정답

```javascript
const products = [
  { id: 1, name: "키보드", price: 80000, stock: 5 },
  { id: 2, name: "마우스", price: 35000, stock: 0 },
  { id: 3, name: "모니터", price: 450000, stock: 3 }
];

const html = products.map(p => {
  const stockText = p.stock > 0 ? `재고: ${p.stock}개` : '품절';

  return `
    <div class="product">
      <h4>${p.name}</h4>
      <p>가격: ${p.price}원</p>
      <p class="${p.stock > 0 ? 'available' : 'soldout'}">
        ${stockText}
      </p>
    </div>
  `;
}).join('');

document.querySelector('#products').innerHTML = html;
```

## 요약

| 구분 | 기존 방식 | 템플릿 리터럴 |
|------|----------|--------------|
| 구분자 | `'` 또는 `"` | `` ` `` (백틱) |
| 변수 삽입 | `'text ' + var` | `` `text ${var}` `` |
| 여러 줄 | `\n` 또는 `+` | 그냥 줄바꿈 |
| 표현식 | 불가능 (계산 먼저) | `${a + b}` 가능 |
| 가독성 | 낮음 | ✅ 높음 |

**핵심 원칙:**
1. 문자열에 변수를 넣을 때는 템플릿 리터럴 사용
2. 여러 줄 문자열은 템플릿 리터럴이 필수
3. HTML 생성 시 가독성이 크게 향상됨
4. React/JSX에서는 기본적으로 사용
