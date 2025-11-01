# 구조 분해 할당 (Destructuring)

## jQuery 시대의 데이터 추출

예전에는 객체나 배열에서 값을 꺼낼 때 하나씩 접근했습니다.

```javascript
var user = {
  name: "홍길동",
  age: 25,
  email: "hong@example.com"
};

var name = user.name;
var age = user.age;
var email = user.email;

console.log(name, age, email);
```

```javascript
var colors = ["red", "green", "blue"];

var first = colors[0];
var second = colors[1];
var third = colors[2];
```

이 방법은 반복적이고 코드가 길어집니다.

## 구조 분해 할당이란?

객체나 배열에서 값을 추출하여 변수에 한 번에 할당하는 문법입니다.

```javascript
// 객체 분해
const user = {
  name: "홍길동",
  age: 25,
  email: "hong@example.com"
};

const { name, age, email } = user;
console.log(name, age, email);
// 출력: 홍길동 25 hong@example.com

// 배열 분해
const colors = ["red", "green", "blue"];
const [first, second, third] = colors;
console.log(first, second, third);
// 출력: red green blue
```

## 객체 구조 분해

### 1. 기본 사용법

```javascript
const user = {
  name: "김철수",
  age: 30,
  city: "서울"
};

// 기존 방식
// const name = user.name;
// const age = user.age;

// 구조 분해
const { name, age, city } = user;

console.log(name); // 김철수
console.log(age);  // 30
console.log(city); // 서울
```

### 2. 필요한 속성만 추출

```javascript
const user = {
  id: 1,
  name: "홍길동",
  age: 25,
  email: "hong@example.com",
  phone: "010-1234-5678"
};

// name과 email만 필요할 때
const { name, email } = user;
console.log(name, email);
// 나머지 속성(id, age, phone)은 무시됨
```

### 3. 다른 이름으로 할당

```javascript
const user = {
  name: "홍길동",
  age: 25
};

// name을 userName으로 변경
const { name: userName, age: userAge } = user;

console.log(userName); // 홍길동
console.log(userAge);  // 25
// console.log(name);  // 에러! name은 정의되지 않음
```

### 4. 기본값 설정

```javascript
const user = {
  name: "홍길동",
  age: 25
  // country 속성이 없음
};

const { name, age, country = "한국" } = user;

console.log(name);    // 홍길동
console.log(age);     // 25
console.log(country); // 한국 (기본값)
```

### 5. 중첩된 객체

```javascript
const user = {
  name: "홍길동",
  address: {
    city: "서울",
    zipCode: "12345"
  }
};

// 중첩된 객체에서 추출
const {
  name,
  address: { city, zipCode }
} = user;

console.log(name);    // 홍길동
console.log(city);    // 서울
console.log(zipCode); // 12345
// console.log(address); // 에러! address는 할당되지 않음
```

## 배열 구조 분해

### 1. 기본 사용법

```javascript
const colors = ["red", "green", "blue"];

const [first, second, third] = colors;

console.log(first);  // red
console.log(second); // green
console.log(third);  // blue
```

### 2. 일부 요소만 추출

```javascript
const numbers = [1, 2, 3, 4, 5];

// 첫 두 개만 필요
const [first, second] = numbers;
console.log(first, second); // 1 2

// 중간 건너뛰기
const [a, , c] = numbers;
console.log(a, c); // 1 3
```

### 3. 나머지 요소 수집

```javascript
const numbers = [1, 2, 3, 4, 5];

const [first, second, ...rest] = numbers;

console.log(first);  // 1
console.log(second); // 2
console.log(rest);   // [3, 4, 5]
```

### 4. 기본값 설정

```javascript
const colors = ["red"];

const [first, second = "green", third = "blue"] = colors;

console.log(first);  // red
console.log(second); // green (기본값)
console.log(third);  // blue (기본값)
```

### 5. 값 교환

```javascript
let a = 1;
let b = 2;

// 기존 방식
// let temp = a;
// a = b;
// b = temp;

// 구조 분해로 간단하게
[a, b] = [b, a];

console.log(a); // 2
console.log(b); // 1
```

## jQuery vs 구조 분해 할당

### Ajax 응답 처리

```javascript
// jQuery 스타일
$.ajax({
  url: '/api/user/123',
  success: function(response) {
    var name = response.data.name;
    var email = response.data.email;
    var city = response.data.address.city;

    $('#name').text(name);
    $('#email').text(email);
    $('#city').text(city);
  }
});

// 현대 JavaScript 스타일
fetch('/api/user/123')
  .then(res => res.json())
  .then(response => {
    const {
      data: {
        name,
        email,
        address: { city }
      }
    } = response;

    document.querySelector('#name').textContent = name;
    document.querySelector('#email').textContent = email;
    document.querySelector('#city').textContent = city;
  });
```

### 여러 요소 처리

```javascript
// jQuery 스타일
$('.user-card').each(function() {
  var $card = $(this);
  var name = $card.data('name');
  var age = $card.data('age');
  var city = $card.data('city');

  $card.find('.info').text(name + ', ' + age + '세, ' + city);
});

// 현대 JavaScript 스타일
document.querySelectorAll('.user-card').forEach(card => {
  const { name, age, city } = card.dataset;

  card.querySelector('.info').textContent = `${name}, ${age}세, ${city}`;
});
```

## 함수 매개변수에서의 활용

### 1. 객체 매개변수 분해

```javascript
// 기존 방식
function createUser(options) {
  var name = options.name;
  var age = options.age;
  var role = options.role || 'user';

  return {
    name: name,
    age: age,
    role: role
  };
}

// 구조 분해 사용
function createUser({ name, age, role = 'user' }) {
  return { name, age, role };
}

const user = createUser({ name: "홍길동", age: 25 });
console.log(user);
// { name: "홍길동", age: 25, role: "user" }
```

### 2. 배열 매개변수 분해

```javascript
function getCoordinates() {
  return [37.5665, 126.9780]; // 서울의 위도, 경도
}

const [latitude, longitude] = getCoordinates();
console.log(`위도: ${latitude}, 경도: ${longitude}`);
```

### 3. 여러 값 반환하기

```javascript
function getMinMax(numbers) {
  return {
    min: Math.min(...numbers),
    max: Math.max(...numbers)
  };
}

const { min, max } = getMinMax([3, 1, 4, 1, 5, 9]);
console.log(`최소: ${min}, 최대: ${max}`);
// 최소: 1, 최대: 9
```

## React에서의 활용

React에서는 구조 분해 할당이 필수적입니다!

### 1. Props 분해

```javascript
// 기존 방식
function UserCard(props) {
  return (
    <div>
      <h2>{props.name}</h2>
      <p>{props.email}</p>
      <p>{props.age}세</p>
    </div>
  );
}

// 구조 분해 사용 (추천)
function UserCard({ name, email, age }) {
  return (
    <div>
      <h2>{name}</h2>
      <p>{email}</p>
      <p>{age}세</p>
    </div>
  );
}
```

### 2. State와 Hooks

```javascript
// useState 훅
function Counter() {
  // useState는 배열을 반환 -> 배열 분해
  const [count, setCount] = React.useState(0);

  return (
    <div>
      <p>카운트: {count}</p>
      <button onClick={() => setCount(count + 1)}>증가</button>
    </div>
  );
}
```

### 3. 이벤트 객체

```javascript
function Form() {
  const handleSubmit = (event) => {
    // 이벤트 객체에서 필요한 것만 추출
    const { target, preventDefault } = event;

    preventDefault();

    const formData = new FormData(target);
    console.log(formData);
  };

  return <form onSubmit={handleSubmit}>...</form>;
}
```

## 실전 예제

### 1. API 데이터 처리

```javascript
const fetchUserData = async (userId) => {
  const response = await fetch(`/api/users/${userId}`);
  const {
    data: {
      profile: { name, avatar },
      stats: { posts, followers }
    }
  } = await response.json();

  return { name, avatar, posts, followers };
};
```

### 2. 설정 객체 병합

```javascript
const defaultConfig = {
  theme: 'light',
  language: 'ko',
  notifications: true
};

const userConfig = {
  theme: 'dark',
  language: 'en'
};

const { theme, language, notifications } = {
  ...defaultConfig,
  ...userConfig
};

console.log({ theme, language, notifications });
// { theme: 'dark', language: 'en', notifications: true }
```

### 3. 배열 메서드와 조합

```javascript
const users = [
  { id: 1, name: "홍길동", role: "admin" },
  { id: 2, name: "김철수", role: "user" },
  { id: 3, name: "이영희", role: "user" }
];

// map과 구조 분해
const userNames = users.map(({ name }) => name);
console.log(userNames);
// ["홍길동", "김철수", "이영희"]

// filter와 구조 분해
const admins = users.filter(({ role }) => role === 'admin');
console.log(admins);
// [{ id: 1, name: "홍길동", role: "admin" }]

// forEach와 구조 분해
users.forEach(({ id, name, role }) => {
  console.log(`ID: ${id}, 이름: ${name}, 역할: ${role}`);
});
```

## 연습 문제

다음 jQuery 코드를 구조 분해 할당을 사용하여 다시 작성해보세요:

```javascript
$.ajax({
  url: '/api/products',
  success: function(response) {
    var products = response.data.items;
    var total = response.data.total;
    var page = response.pagination.currentPage;
    var pageSize = response.pagination.pageSize;

    products.forEach(function(product) {
      var id = product.id;
      var name = product.name;
      var price = product.price;
      var stock = product.inventory.stock;

      var html = '<div class="product">' +
                 '  <h3>' + name + '</h3>' +
                 '  <p>가격: ' + price + '</p>' +
                 '  <p>재고: ' + stock + '</p>' +
                 '</div>';

      $('#products').append(html);
    });

    $('#pagination').text('페이지 ' + page + ' / ' + Math.ceil(total / pageSize));
  }
});
```

### 정답

```javascript
fetch('/api/products')
  .then(res => res.json())
  .then(response => {
    const {
      data: { items: products, total },
      pagination: { currentPage: page, pageSize }
    } = response;

    const html = products.map(({
      id,
      name,
      price,
      inventory: { stock }
    }) => `
      <div class="product">
        <h3>${name}</h3>
        <p>가격: ${price}</p>
        <p>재고: ${stock}</p>
      </div>
    `).join('');

    document.querySelector('#products').innerHTML = html;

    const totalPages = Math.ceil(total / pageSize);
    document.querySelector('#pagination').textContent = `페이지 ${page} / ${totalPages}`;
  });
```

## 주의사항

### 1. undefined 처리

```javascript
const user = null;

// 에러 발생!
// const { name } = user; // Cannot destructure property 'name' of 'null'

// 안전하게 처리
const { name } = user || {};
console.log(name); // undefined
```

### 2. 너무 깊은 중첩 피하기

```javascript
// 나쁜 예 - 가독성이 떨어짐
const {
  data: {
    user: {
      profile: {
        address: {
          city
        }
      }
    }
  }
} = response;

// 좋은 예 - 단계적으로 분해
const { data } = response;
const { user } = data;
const { profile } = user;
const { address } = profile;
const { city } = address;
```

## 요약

| 구분 | 기존 방식 | 구조 분해 할당 |
|------|----------|--------------|
| 객체 | `const a = obj.a` | `const { a } = obj` |
| 배열 | `const a = arr[0]` | `const [a] = arr` |
| 기본값 | `const a = obj.a \|\| 'default'` | `const { a = 'default' } = obj` |
| 이름 변경 | `const newName = obj.a` | `const { a: newName } = obj` |
| 가독성 | 보통 | ✅ 높음 |

**핵심 원칙:**
1. 객체/배열에서 여러 값을 추출할 때는 구조 분해 사용
2. 함수 매개변수도 구조 분해하면 가독성 향상
3. React에서는 거의 필수로 사용
4. 너무 깊은 중첩은 피하기
