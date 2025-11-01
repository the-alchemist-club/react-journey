# 스프레드 연산자 (Spread Operator)

## jQuery 시대의 배열/객체 복사

예전에는 배열이나 객체를 복사하거나 합칠 때 반복문이나 jQuery 메서드를 사용했습니다.

```javascript
// 배열 복사
var arr1 = [1, 2, 3];
var arr2 = arr1.slice(); // 또는 [].concat(arr1)

// 배열 합치기
var arr1 = [1, 2, 3];
var arr2 = [4, 5, 6];
var combined = arr1.concat(arr2);

// 객체 복사
var obj1 = { a: 1, b: 2 };
var obj2 = $.extend({}, obj1); // jQuery 사용
```

## 스프레드 연산자란?

`...` (점 세 개)를 사용하여 배열이나 객체를 펼치는 문법입니다.

```javascript
const arr = [1, 2, 3];
console.log(...arr); // 1 2 3 (배열을 펼침)

const obj = { a: 1, b: 2 };
const newObj = { ...obj }; // 객체를 복사
```

## 배열에서의 스프레드 연산자

### 1. 배열 복사

```javascript
// 기존 방식
var original = [1, 2, 3];
var copy = original.slice();

// 스프레드 연산자
const original = [1, 2, 3];
const copy = [...original];

copy.push(4);
console.log(original); // [1, 2, 3] (원본은 변경 안 됨)
console.log(copy);     // [1, 2, 3, 4]
```

### 2. 배열 합치기

```javascript
// 기존 방식
var arr1 = [1, 2, 3];
var arr2 = [4, 5, 6];
var combined = arr1.concat(arr2);

// 스프레드 연산자
const arr1 = [1, 2, 3];
const arr2 = [4, 5, 6];
const combined = [...arr1, ...arr2];

console.log(combined); // [1, 2, 3, 4, 5, 6]

// 중간에 요소 추가도 가능
const result = [...arr1, 'middle', ...arr2];
console.log(result); // [1, 2, 3, 'middle', 4, 5, 6]
```

### 3. 배열에 요소 추가

```javascript
const numbers = [2, 3, 4];

// 앞에 추가
const withFirst = [1, ...numbers];
console.log(withFirst); // [1, 2, 3, 4]

// 뒤에 추가
const withLast = [...numbers, 5];
console.log(withLast); // [2, 3, 4, 5]

// 양쪽에 추가
const withBoth = [0, 1, ...numbers, 5, 6];
console.log(withBoth); // [0, 1, 2, 3, 4, 5, 6]
```

### 4. 배열 요소를 함수 인자로 전달

```javascript
const numbers = [1, 5, 3, 9, 2];

// 기존 방식
var max = Math.max.apply(null, numbers);

// 스프레드 연산자
const max = Math.max(...numbers);
console.log(max); // 9

const min = Math.min(...numbers);
console.log(min); // 1
```

## 객체에서의 스프레드 연산자

### 1. 객체 복사

```javascript
// 기존 방식 (jQuery)
var user = { name: "홍길동", age: 25 };
var copy = $.extend({}, user);

// 스프레드 연산자
const user = { name: "홍길동", age: 25 };
const copy = { ...user };

copy.age = 26;
console.log(user.age); // 25 (원본은 변경 안 됨)
console.log(copy.age); // 26
```

### 2. 객체 합치기

```javascript
const defaults = {
  theme: 'light',
  language: 'ko',
  notifications: true
};

const userSettings = {
  theme: 'dark',
  fontSize: 16
};

const settings = { ...defaults, ...userSettings };

console.log(settings);
// {
//   theme: 'dark',        // userSettings가 덮어씀
//   language: 'ko',
//   notifications: true,
//   fontSize: 16          // 새로 추가됨
// }
```

### 3. 객체 속성 업데이트

```javascript
const user = {
  id: 1,
  name: "홍길동",
  email: "hong@example.com",
  age: 25
};

// 특정 속성만 변경
const updatedUser = {
  ...user,
  age: 26,
  city: "서울" // 새 속성 추가
};

console.log(updatedUser);
// { id: 1, name: "홍길동", email: "hong@example.com", age: 26, city: "서울" }
```

### 4. 중첩된 객체 (주의!)

```javascript
const user = {
  name: "홍길동",
  address: {
    city: "서울",
    zipCode: "12345"
  }
};

// 얕은 복사 (shallow copy)
const copy = { ...user };

copy.address.city = "부산";

console.log(user.address.city);  // "부산" (원본도 변경됨!)
console.log(copy.address.city);  // "부산"

// 깊은 복사를 위해서는 중첩된 객체도 스프레드
const deepCopy = {
  ...user,
  address: { ...user.address }
};

deepCopy.address.city = "대구";
console.log(user.address.city);     // "부산" (원본 유지)
console.log(deepCopy.address.city); // "대구"
```

## jQuery vs 스프레드 연산자

### 배열 조작

```javascript
// jQuery 스타일
var items = ['사과', '바나나'];

// 새 아이템 추가
var newItems = items.slice();
newItems.push('오렌지');

// 배열 합치기
var moreItems = ['포도', '수박'];
var allItems = items.concat(moreItems);

// 현대 JavaScript 스타일
const items = ['사과', '바나나'];

// 새 아이템 추가 (불변성 유지)
const newItems = [...items, '오렌지'];

// 배열 합치기
const moreItems = ['포도', '수박'];
const allItems = [...items, ...moreItems];
```

### 객체 설정 병합

```javascript
// jQuery 스타일
var defaultOptions = {
  width: 300,
  height: 200,
  animation: true
};

var userOptions = {
  width: 500
};

var options = $.extend({}, defaultOptions, userOptions);

// 현대 JavaScript 스타일
const defaultOptions = {
  width: 300,
  height: 200,
  animation: true
};

const userOptions = {
  width: 500
};

const options = { ...defaultOptions, ...userOptions };
```

## React에서의 활용

React에서는 스프레드 연산자가 매우 중요합니다! 불변성을 유지하면서 상태를 업데이트할 때 필수적으로 사용됩니다.

### 1. Props 전달

```javascript
const user = {
  name: "홍길동",
  age: 25,
  email: "hong@example.com"
};

// Props를 하나씩 전달
<UserCard name={user.name} age={user.age} email={user.email} />

// 스프레드로 한 번에 전달 (추천)
<UserCard {...user} />
```

### 2. State 업데이트

```javascript
function TodoApp() {
  const [todos, setTodos] = React.useState([
    { id: 1, text: '공부하기', done: false },
    { id: 2, text: '운동하기', done: false }
  ]);

  const addTodo = (text) => {
    const newTodo = {
      id: Date.now(),
      text: text,
      done: false
    };

    // 기존 배열에 새 할 일 추가 (불변성 유지)
    setTodos([...todos, newTodo]);
  };

  const toggleTodo = (id) => {
    // 특정 항목만 업데이트
    setTodos(todos.map(todo =>
      todo.id === id
        ? { ...todo, done: !todo.done } // 해당 todo만 복사 후 done 변경
        : todo
    ));
  };

  return (
    // JSX...
  );
}
```

### 3. 객체 State 업데이트

```javascript
function ProfileEditor() {
  const [user, setUser] = React.useState({
    name: "홍길동",
    age: 25,
    address: {
      city: "서울",
      zipCode: "12345"
    }
  });

  const updateName = (newName) => {
    setUser({
      ...user,
      name: newName
    });
  };

  const updateCity = (newCity) => {
    setUser({
      ...user,
      address: {
        ...user.address,
        city: newCity
      }
    });
  };

  return (
    // JSX...
  );
}
```

## 나머지 매개변수 (Rest Parameters)

스프레드와 비슷하게 `...`를 사용하지만, 반대 역할을 합니다.

```javascript
// 가변 인자 함수
function sum(...numbers) {
  return numbers.reduce((total, num) => total + num, 0);
}

console.log(sum(1, 2, 3));        // 6
console.log(sum(1, 2, 3, 4, 5));  // 15

// 첫 번째 인자와 나머지 분리
function greet(greeting, ...names) {
  return `${greeting}, ${names.join(' and ')}!`;
}

console.log(greet('Hello', 'Alice'));              // Hello, Alice!
console.log(greet('Hello', 'Alice', 'Bob'));       // Hello, Alice and Bob!
console.log(greet('Hello', 'Alice', 'Bob', 'Charlie')); // Hello, Alice and Bob and Charlie!
```

## 실전 예제

### 1. 장바구니 관리

```javascript
let cart = [
  { id: 1, name: "키보드", price: 80000, quantity: 1 },
  { id: 2, name: "마우스", price: 35000, quantity: 2 }
];

// 상품 추가
const newProduct = { id: 3, name: "모니터", price: 450000, quantity: 1 };
cart = [...cart, newProduct];

// 수량 변경
const updateQuantity = (id, quantity) => {
  cart = cart.map(item =>
    item.id === id
      ? { ...item, quantity }
      : item
  );
};

// 상품 제거
const removeProduct = (id) => {
  cart = cart.filter(item => item.id !== id);
};
```

### 2. 필터 적용

```javascript
const products = [
  { id: 1, name: "노트북", category: "전자", price: 1200000 },
  { id: 2, name: "키보드", category: "전자", price: 80000 },
  { id: 3, name: "책상", category: "가구", price: 150000 }
];

let filters = {
  category: null,
  minPrice: 0,
  maxPrice: Infinity
};

// 필터 업데이트
const updateFilter = (newFilters) => {
  filters = { ...filters, ...newFilters };
};

updateFilter({ category: "전자" });
updateFilter({ minPrice: 50000, maxPrice: 100000 });

console.log(filters);
// { category: "전자", minPrice: 50000, maxPrice: 100000 }
```

### 3. 폼 데이터 관리

```javascript
const formData = {
  name: "",
  email: "",
  phone: "",
  agreeToTerms: false
};

// 입력 필드 변경 핸들러
const handleChange = (field, value) => {
  return {
    ...formData,
    [field]: value
  };
};

// 사용 예
const newFormData1 = handleChange('name', '홍길동');
const newFormData2 = handleChange('email', 'hong@example.com');
```

## 연습 문제

다음 jQuery 코드를 스프레드 연산자를 사용하여 다시 작성해보세요:

```javascript
// jQuery 스타일
var todos = [
  { id: 1, text: '공부하기', done: false },
  { id: 2, text: '운동하기', done: true }
];

// 새 할 일 추가
var newTodo = { id: 3, text: '책 읽기', done: false };
var updatedTodos = todos.slice();
updatedTodos.push(newTodo);

// 특정 할 일의 done 상태 토글
var toggledTodos = todos.map(function(todo) {
  if (todo.id === 2) {
    return $.extend({}, todo, { done: !todo.done });
  }
  return todo;
});

// 완료되지 않은 할 일만 필터링
var activeTodos = todos.filter(function(todo) {
  return !todo.done;
});
```

### 정답

```javascript
const todos = [
  { id: 1, text: '공부하기', done: false },
  { id: 2, text: '운동하기', done: true }
];

// 새 할 일 추가
const newTodo = { id: 3, text: '책 읽기', done: false };
const updatedTodos = [...todos, newTodo];

// 특정 할 일의 done 상태 토글
const toggledTodos = todos.map(todo =>
  todo.id === 2
    ? { ...todo, done: !todo.done }
    : todo
);

// 완료되지 않은 할 일만 필터링
const activeTodos = todos.filter(todo => !todo.done);
```

## 주의사항

### 1. 얕은 복사 (Shallow Copy)

```javascript
// 1단계 객체는 안전
const obj = { a: 1, b: 2 };
const copy = { ...obj };
copy.a = 999;
console.log(obj.a); // 1 (원본 유지)

// 중첩된 객체는 주의!
const nested = {
  a: 1,
  b: { c: 2 }
};

const copy = { ...nested };
copy.b.c = 999;
console.log(nested.b.c); // 999 (원본도 변경됨!)

// 해결: 중첩된 부분도 복사
const deepCopy = {
  ...nested,
  b: { ...nested.b }
};
```

### 2. 성능 고려

```javascript
// 많은 요소를 복사할 때는 성능 고려
const hugeArray = new Array(1000000).fill(0);

// 매번 복사하면 느림
for (let i = 0; i < 100; i++) {
  const newArray = [...hugeArray, i]; // 비효율적
}

// 필요할 때만 복사
const newArray = hugeArray.slice();
for (let i = 0; i < 100; i++) {
  newArray.push(i); // 효율적
}
```

## 요약

| 구분 | 기존 방식 | 스프레드 연산자 |
|------|----------|----------------|
| 배열 복사 | `arr.slice()` | `[...arr]` |
| 배열 합치기 | `arr1.concat(arr2)` | `[...arr1, ...arr2]` |
| 객체 복사 | `$.extend({}, obj)` | `{...obj}` |
| 객체 병합 | `$.extend({}, obj1, obj2)` | `{...obj1, ...obj2}` |
| 가독성 | 보통 | ✅ 높음 |

**핵심 원칙:**
1. 배열/객체를 복사할 때는 스프레드 연산자 사용
2. React에서는 불변성 유지를 위해 필수
3. 얕은 복사라는 점을 항상 기억
4. 중첩된 객체는 각 단계마다 스프레드 필요
