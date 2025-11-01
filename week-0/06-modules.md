# 모듈 시스템 (ES6 Modules)

## jQuery 시대의 스크립트 관리

예전에는 HTML에 `<script>` 태그로 여러 파일을 불러왔습니다.

```html
<!DOCTYPE html>
<html>
<head>
  <title>My App</title>
</head>
<body>
  <!-- jQuery -->
  <script src="jquery.min.js"></script>

  <!-- 플러그인들 -->
  <script src="jquery.plugin1.js"></script>
  <script src="jquery.plugin2.js"></script>

  <!-- 내 코드 -->
  <script src="utils.js"></script>
  <script src="api.js"></script>
  <script src="app.js"></script>
</body>
</html>
```

**문제점:**
- 순서를 잘못 지정하면 에러 발생
- 전역 변수 충돌 위험
- 어떤 파일이 어떤 파일을 의존하는지 불명확
- 코드 재사용이 어려움

## 모듈 시스템이란?

파일을 모듈로 나누고, `import`와 `export`로 코드를 공유하는 시스템입니다.

```javascript
// utils.js
export const greet = (name) => `안녕하세요, ${name}님!`;
export const PI = 3.14159;

// app.js
import { greet, PI } from './utils.js';

console.log(greet('홍길동')); // 안녕하세요, 홍길동님!
console.log(PI);              // 3.14159
```

## Export (내보내기)

### 1. Named Export (이름 있는 내보내기)

여러 개를 내보낼 수 있습니다.

```javascript
// math.js
export const add = (a, b) => a + b;
export const subtract = (a, b) => a - b;
export const multiply = (a, b) => a * b;

// 또는 한 번에
const add = (a, b) => a + b;
const subtract = (a, b) => a - b;
const multiply = (a, b) => a * b;

export { add, subtract, multiply };
```

### 2. Default Export (기본 내보내기)

파일당 하나만 내보낼 수 있습니다.

```javascript
// User.js
const User = {
  name: "홍길동",
  age: 25,
  greet() {
    return `안녕하세요, ${this.name}입니다.`;
  }
};

export default User;

// 또는 바로 내보내기
export default {
  name: "홍길동",
  age: 25,
  greet() {
    return `안녕하세요, ${this.name}입니다.`;
  }
};
```

### 3. Named와 Default 혼용

```javascript
// api.js
export const API_URL = "https://api.example.com";

const fetchData = async (endpoint) => {
  const response = await fetch(`${API_URL}/${endpoint}`);
  return response.json();
};

export default fetchData;
export { fetchData as fetch }; // 이름도 함께 내보내기
```

## Import (가져오기)

### 1. Named Import

```javascript
// 특정 항목만 가져오기
import { add, subtract } from './math.js';

console.log(add(5, 3));      // 8
console.log(subtract(5, 3)); // 2

// 여러 개 가져오기
import { add, subtract, multiply } from './math.js';

// 이름 변경
import { add as plus, subtract as minus } from './math.js';

console.log(plus(5, 3));  // 8
console.log(minus(5, 3)); // 2

// 모든 것을 하나의 객체로
import * as math from './math.js';

console.log(math.add(5, 3));      // 8
console.log(math.subtract(5, 3)); // 2
```

### 2. Default Import

```javascript
// 이름은 자유롭게 지정 가능
import User from './User.js';
import MyUser from './User.js'; // 같은 파일, 다른 이름

console.log(User.name);   // 홍길동
console.log(MyUser.name); // 홍길동
```

### 3. Named와 Default 함께 가져오기

```javascript
import fetchData, { API_URL } from './api.js';

console.log(API_URL);           // https://api.example.com
fetchData('users').then(...);   // API 호출
```

## jQuery 프로젝트를 모듈로 변환

### Before: jQuery 스타일

```javascript
// utils.js
var Utils = {
  formatPrice: function(price) {
    return price.toLocaleString() + '원';
  },
  formatDate: function(date) {
    return date.toLocaleDateString('ko-KR');
  }
};

// api.js
var API = {
  baseUrl: 'https://api.example.com',
  getUsers: function() {
    return $.ajax({
      url: this.baseUrl + '/users',
      method: 'GET'
    });
  }
};

// app.js
$(document).ready(function() {
  API.getUsers().done(function(users) {
    users.forEach(function(user) {
      var html = '<div>' + user.name + '</div>';
      $('#users').append(html);
    });
  });
});
```

### After: 모듈 시스템

```javascript
// utils.js
export const formatPrice = (price) => {
  return price.toLocaleString() + '원';
};

export const formatDate = (date) => {
  return date.toLocaleDateString('ko-KR');
};

// api.js
const BASE_URL = 'https://api.example.com';

export const getUsers = async () => {
  const response = await fetch(`${BASE_URL}/users`);
  return response.json();
};

export const getUser = async (id) => {
  const response = await fetch(`${BASE_URL}/users/${id}`);
  return response.json();
};

// app.js
import { getUsers } from './api.js';
import { formatPrice } from './utils.js';

const init = async () => {
  const users = await getUsers();

  const html = users.map(user => `
    <div class="user">
      <h3>${user.name}</h3>
      <p>${user.email}</p>
    </div>
  `).join('');

  document.querySelector('#users').innerHTML = html;
};

init();
```

## React에서의 모듈 사용

React는 컴포넌트 기반이므로 모듈 시스템이 필수입니다.

### 1. 컴포넌트 내보내기/가져오기

```javascript
// Button.js
const Button = ({ text, onClick }) => {
  return (
    <button onClick={onClick}>
      {text}
    </button>
  );
};

export default Button;

// App.js
import React from 'react';
import Button from './Button';

function App() {
  const handleClick = () => {
    alert('클릭됨!');
  };

  return (
    <div>
      <h1>내 앱</h1>
      <Button text="클릭하세요" onClick={handleClick} />
    </div>
  );
}

export default App;
```

### 2. 여러 컴포넌트 내보내기

```javascript
// components/index.js
export { default as Button } from './Button';
export { default as Input } from './Input';
export { default as Card } from './Card';

// App.js
import { Button, Input, Card } from './components';
```

### 3. 유틸리티 함수

```javascript
// utils/validators.js
export const isEmail = (email) => {
  return /^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(email);
};

export const isPhoneNumber = (phone) => {
  return /^010-\d{4}-\d{4}$/.test(phone);
};

export const isEmpty = (value) => {
  return value === null || value === undefined || value === '';
};

// components/LoginForm.js
import { isEmail, isEmpty } from '../utils/validators';

function LoginForm() {
  const handleSubmit = (e) => {
    e.preventDefault();
    const email = e.target.email.value;

    if (isEmpty(email)) {
      alert('이메일을 입력하세요');
      return;
    }

    if (!isEmail(email)) {
      alert('올바른 이메일 형식이 아닙니다');
      return;
    }

    // 로그인 처리...
  };

  return <form onSubmit={handleSubmit}>...</form>;
}
```

## 실전 프로젝트 구조

```
my-app/
├── src/
│   ├── components/          # 재사용 가능한 컴포넌트
│   │   ├── Button.js
│   │   ├── Input.js
│   │   └── Card.js
│   ├── pages/              # 페이지 컴포넌트
│   │   ├── Home.js
│   │   ├── About.js
│   │   └── Contact.js
│   ├── utils/              # 유틸리티 함수
│   │   ├── validators.js
│   │   ├── formatters.js
│   │   └── helpers.js
│   ├── api/                # API 호출
│   │   ├── users.js
│   │   ├── products.js
│   │   └── index.js
│   ├── hooks/              # Custom Hooks
│   │   ├── useAuth.js
│   │   └── useFetch.js
│   └── App.js              # 메인 앱
└── index.js                # 진입점
```

### API 모듈 예제

```javascript
// api/client.js
const BASE_URL = 'https://api.example.com';

export const apiClient = {
  get: async (endpoint) => {
    const response = await fetch(`${BASE_URL}${endpoint}`);
    if (!response.ok) throw new Error('API 오류');
    return response.json();
  },

  post: async (endpoint, data) => {
    const response = await fetch(`${BASE_URL}${endpoint}`, {
      method: 'POST',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify(data)
    });
    if (!response.ok) throw new Error('API 오류');
    return response.json();
  }
};

// api/users.js
import { apiClient } from './client';

export const getUsers = () => apiClient.get('/users');
export const getUser = (id) => apiClient.get(`/users/${id}`);
export const createUser = (data) => apiClient.post('/users', data);
export const updateUser = (id, data) => apiClient.post(`/users/${id}`, data);

// api/index.js (통합)
export * from './users';
export * from './products';

// 사용하는 곳
import { getUsers, createUser } from './api';
```

## 동적 Import

필요할 때만 모듈을 불러올 수 있습니다 (코드 스플리팅).

```javascript
// 일반 import - 앱 시작 시 로드됨
import HeavyComponent from './HeavyComponent';

// 동적 import - 필요할 때만 로드됨
const loadHeavyComponent = async () => {
  const module = await import('./HeavyComponent');
  const HeavyComponent = module.default;
  // HeavyComponent 사용...
};

// 버튼 클릭 시에만 로드
button.addEventListener('click', loadHeavyComponent);
```

React에서는 `React.lazy`를 사용:

```javascript
import React, { Suspense, lazy } from 'react';

// 지연 로딩
const HeavyComponent = lazy(() => import('./HeavyComponent'));

function App() {
  return (
    <Suspense fallback={<div>로딩 중...</div>}>
      <HeavyComponent />
    </Suspense>
  );
}
```

## 주의사항

### 1. 순환 참조 피하기

```javascript
// 나쁜 예
// a.js
import { b } from './b.js';
export const a = () => b();

// b.js
import { a } from './a.js';
export const b = () => a(); // 순환 참조!

// 좋은 예 - 공통 모듈로 분리
// common.js
export const shared = () => console.log('공통 함수');

// a.js
import { shared } from './common.js';

// b.js
import { shared } from './common.js';
```

### 2. Default vs Named Export

```javascript
// Default Export
// - 파일당 하나만
// - 이름 자유롭게 변경 가능
export default MyComponent;
import Whatever from './MyComponent'; // OK

// Named Export
// - 여러 개 가능
// - 이름 그대로 사용 (또는 as로 변경)
export const MyComponent = ...;
export const AnotherComponent = ...;
import { MyComponent } from './components'; // 이름 일치해야 함
```

### 3. 파일 확장자

```javascript
// 브라우저에서는 확장자 필수
import { greet } from './utils.js'; // .js 필요

// Webpack/Vite 같은 번들러 사용 시 생략 가능
import { greet } from './utils'; // OK

// React에서는 보통 생략
import Button from './Button';
```

## 연습 문제

다음 jQuery 프로젝트를 모듈 시스템으로 변환해보세요:

```javascript
// 현재 구조: 모든 코드가 한 파일에
var API_URL = 'https://api.example.com';

var formatPrice = function(price) {
  return price.toLocaleString() + '원';
};

var getProducts = function() {
  return $.ajax({
    url: API_URL + '/products',
    method: 'GET'
  });
};

var renderProducts = function(products) {
  var html = '';
  products.forEach(function(product) {
    html += '<div class="product">' +
            '  <h3>' + product.name + '</h3>' +
            '  <p>' + formatPrice(product.price) + '</p>' +
            '</div>';
  });
  $('#products').html(html);
};

$(document).ready(function() {
  getProducts().done(renderProducts);
});
```

### 정답

```javascript
// constants.js
export const API_URL = 'https://api.example.com';

// utils/formatters.js
export const formatPrice = (price) => {
  return price.toLocaleString() + '원';
};

// api/products.js
import { API_URL } from '../constants';

export const getProducts = async () => {
  const response = await fetch(`${API_URL}/products`);
  return response.json();
};

// components/ProductList.js
import { formatPrice } from '../utils/formatters';

export const renderProducts = (products) => {
  const html = products.map(product => `
    <div class="product">
      <h3>${product.name}</h3>
      <p>${formatPrice(product.price)}</p>
    </div>
  `).join('');

  document.querySelector('#products').innerHTML = html;
};

// app.js
import { getProducts } from './api/products';
import { renderProducts } from './components/ProductList';

const init = async () => {
  try {
    const products = await getProducts();
    renderProducts(products);
  } catch (error) {
    console.error('에러 발생:', error);
  }
};

// DOM이 준비되면 실행
document.addEventListener('DOMContentLoaded', init);
```

## 요약

| 구분 | 기존 방식 (jQuery) | 모듈 시스템 |
|------|------------------|------------|
| 파일 로드 | `<script>` 태그 | `import` |
| 코드 공유 | 전역 변수 | `export` |
| 의존성 관리 | 수동 (순서 중요) | 자동 |
| 이름 충돌 | ⚠️ 위험 | ✅ 안전 |
| 재사용성 | 낮음 | ✅ 높음 |

**핵심 원칙:**
1. 파일을 기능별로 분리 (모듈화)
2. `export`로 공유, `import`로 사용
3. Default export는 파일당 하나, Named export는 여러 개
4. React에서는 컴포넌트마다 파일 분리가 기본
5. 명확한 폴더 구조 유지

**다음 단계:**
- npm으로 외부 패키지 설치 및 import
- Webpack/Vite 같은 번들러 이해
- React 프로젝트 구조 배우기
