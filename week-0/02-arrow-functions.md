# 화살표 함수 (Arrow Functions)

## jQuery 시대의 함수

jQuery를 사용할 때는 `function` 키워드로 함수를 작성했습니다.

```javascript
// 일반 함수
function greet(name) {
  return "안녕하세요, " + name + "님!";
}

// 익명 함수
$('.button').click(function() {
  alert('클릭됨!');
});

// 콜백 함수
$.ajax({
  url: '/api/data',
  success: function(data) {
    console.log(data);
  }
});
```

## 화살표 함수란?

화살표 함수는 `=>` 기호를 사용하는 더 간결한 함수 작성 방법입니다.

```javascript
// 기존 함수
function greet(name) {
  return "안녕하세요, " + name + "님!";
}

// 화살표 함수
const greet = (name) => {
  return `안녕하세요, ${name}님!`;
};

// 더 간결하게 (중괄호와 return 생략)
const greet = (name) => `안녕하세요, ${name}님!`;
```

## 기본 문법

### 1. 매개변수가 여러 개일 때

```javascript
// 기존 함수
function add(a, b) {
  return a + b;
}

// 화살표 함수
const add = (a, b) => {
  return a + b;
};

// 간결하게
const add = (a, b) => a + b;
```

### 2. 매개변수가 하나일 때

```javascript
// 괄호 생략 가능
const double = num => num * 2;

// 괄호를 써도 됨
const double = (num) => num * 2;
```

### 3. 매개변수가 없을 때

```javascript
// 빈 괄호 필수
const sayHello = () => "안녕하세요!";

// 여러 줄일 때
const getCurrentTime = () => {
  const now = new Date();
  return now.toLocaleTimeString();
};
```

### 4. 객체를 반환할 때

```javascript
// 객체를 반환하려면 괄호로 감싸기
const createUser = (name, age) => ({
  name: name,
  age: age,
  createdAt: new Date()
});

// 이렇게 쓰면 안 됨! (블록으로 인식됨)
// const createUser = (name, age) => { name: name, age: age };
```

## jQuery 이벤트 핸들러 비교

### jQuery 스타일

```javascript
$('.menu-button').click(function() {
  $('.menu').toggle();
});

$('.item').each(function(index) {
  $(this).text('Item ' + index);
});

$('.form').submit(function(event) {
  event.preventDefault();
  var data = $(this).serialize();
  console.log(data);
});
```

### 화살표 함수 스타일

```javascript
const menuButton = document.querySelector('.menu-button');
const menu = document.querySelector('.menu');

menuButton.addEventListener('click', () => {
  menu.classList.toggle('open');
});

const items = document.querySelectorAll('.item');
items.forEach((item, index) => {
  item.textContent = `Item ${index}`;
});

const form = document.querySelector('.form');
form.addEventListener('submit', (event) => {
  event.preventDefault();
  const data = new FormData(form);
  console.log(data);
});
```

## this의 차이 (중요!)

화살표 함수와 일반 함수의 가장 큰 차이는 `this`의 동작 방식입니다.

### 일반 함수의 this

```javascript
// jQuery에서 흔히 겪는 문제
$('.buttons button').click(function() {
  console.log(this); // 클릭된 버튼 요소

  setTimeout(function() {
    console.log(this); // Window 객체! (원하는 결과가 아님)
    // $(this).addClass('clicked'); // 작동 안 함!
  }, 1000);
});

// 해결 방법: that 변수 사용
$('.buttons button').click(function() {
  const that = this;

  setTimeout(function() {
    $(that).addClass('clicked'); // that 사용
  }, 1000);
});
```

### 화살표 함수의 this

```javascript
// 화살표 함수는 this를 상위 스코프에서 가져옴
const buttons = document.querySelectorAll('.buttons button');

buttons.forEach(button => {
  button.addEventListener('click', () => {
    console.log(this); // 상위 스코프의 this

    setTimeout(() => {
      console.log(this); // 여전히 상위 스코프의 this
      button.classList.add('clicked'); // button 변수 사용
    }, 1000);
  });
});
```

### React에서는 화살표 함수가 유용

```javascript
// 클래스 컴포넌트에서
class MyComponent extends React.Component {
  constructor() {
    super();
    this.state = { count: 0 };
  }

  // 일반 함수: bind 필요
  handleClickOld() {
    this.setState({ count: this.state.count + 1 }); // this가 undefined!
  }

  // 화살표 함수: bind 불필요
  handleClick = () => {
    this.setState({ count: this.state.count + 1 }); // 작동함!
  }

  render() {
    return (
      <button onClick={this.handleClick}>
        클릭: {this.state.count}
      </button>
    );
  }
}
```

## 언제 화살표 함수를 사용할까?

### ✅ 화살표 함수를 사용하면 좋은 경우

```javascript
// 1. 배열 메서드
const numbers = [1, 2, 3, 4, 5];
const doubled = numbers.map(num => num * 2);
const evens = numbers.filter(num => num % 2 === 0);
const sum = numbers.reduce((acc, num) => acc + num, 0);

// 2. 콜백 함수
setTimeout(() => {
  console.log('1초 후 실행');
}, 1000);

fetch('/api/data')
  .then(response => response.json())
  .then(data => console.log(data));

// 3. 짧은 유틸리티 함수
const isAdult = age => age >= 18;
const getFullName = (first, last) => `${first} ${last}`;
```

### ❌ 화살표 함수를 피해야 하는 경우

```javascript
// 1. 객체 메서드 (this 필요)
const user = {
  name: '홍길동',
  // 나쁜 예
  greetBad: () => {
    console.log(`안녕하세요, ${this.name}님`); // this.name은 undefined
  },
  // 좋은 예
  greet() {
    console.log(`안녕하세요, ${this.name}님`); // 정상 작동
  }
};

// 2. 생성자 함수
// const Person = (name) => {
//   this.name = name; // 에러! 화살표 함수는 생성자로 사용 불가
// };

// 좋은 예
function Person(name) {
  this.name = name;
}

// 3. 가변 인자 (arguments 객체)
// const sum = () => {
//   return Array.from(arguments).reduce((a, b) => a + b); // arguments 없음
// };

// 좋은 예
function sum() {
  return Array.from(arguments).reduce((a, b) => a + b);
}
// 또는 나머지 매개변수 사용
const sum = (...nums) => nums.reduce((a, b) => a + b);
```

## 실전 예제

### Ajax 요청 비교

```javascript
// jQuery 스타일
$.ajax({
  url: '/api/users',
  method: 'GET',
  success: function(users) {
    var userList = users.map(function(user) {
      return '<li>' + user.name + '</li>';
    });
    $('#userList').html(userList.join(''));
  },
  error: function(error) {
    console.error('에러:', error);
  }
});

// 현대 JavaScript 스타일 (화살표 함수)
fetch('/api/users')
  .then(response => response.json())
  .then(users => {
    const userList = users.map(user => `<li>${user.name}</li>`);
    document.querySelector('#userList').innerHTML = userList.join('');
  })
  .catch(error => {
    console.error('에러:', error);
  });
```

### 이벤트 위임

```javascript
// jQuery 스타일
$(document).on('click', '.delete-button', function() {
  var itemId = $(this).data('id');
  deleteItem(itemId);
});

// 현대 JavaScript 스타일
document.addEventListener('click', (event) => {
  if (event.target.matches('.delete-button')) {
    const itemId = event.target.dataset.id;
    deleteItem(itemId);
  }
});

const deleteItem = (id) => {
  console.log(`아이템 ${id} 삭제`);
};
```

## 연습 문제

다음 jQuery 코드를 화살표 함수를 사용하여 다시 작성해보세요:

```javascript
// jQuery 코드
var numbers = [1, 2, 3, 4, 5];

var doubled = numbers.map(function(n) {
  return n * 2;
});

var sum = numbers.reduce(function(acc, n) {
  return acc + n;
}, 0);

$('.calculate').click(function() {
  var result = sum / numbers.length;
  $('.result').text('평균: ' + result);
});
```

### 정답

```javascript
const numbers = [1, 2, 3, 4, 5];

const doubled = numbers.map(n => n * 2);

const sum = numbers.reduce((acc, n) => acc + n, 0);

const calculateButton = document.querySelector('.calculate');
const resultElement = document.querySelector('.result');

calculateButton.addEventListener('click', () => {
  const result = sum / numbers.length;
  resultElement.textContent = `평균: ${result}`;
});
```

## 요약

| 구분 | 일반 함수 | 화살표 함수 |
|------|----------|------------|
| 문법 | `function() {}` | `() => {}` |
| this | 호출 방식에 따라 변함 | 상위 스코프의 this |
| arguments | ✅ 있음 | ❌ 없음 (나머지 매개변수 사용) |
| 생성자 | ✅ 가능 | ❌ 불가능 |
| 간결성 | 보통 | ✅ 매우 간결 |

**핵심 원칙:**
1. 콜백과 배열 메서드에는 화살표 함수 사용
2. 객체 메서드는 일반 함수 사용
3. `this`가 중요한 경우 화살표 함수의 동작 이해 필수
4. React에서는 대부분 화살표 함수 사용
