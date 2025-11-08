# Props와 State

## jQuery 시대의 데이터 관리

jQuery에서는 변수와 DOM에 데이터를 저장했습니다.

```javascript
// 전역 변수로 관리
var userName = "홍길동";
var userAge = 25;
var isLoggedIn = false;

// DOM에 데이터 저장
$('#user-name').data('userId', 123);
$('#user-name').text(userName);

// 데이터 변경 시 수동으로 DOM 업데이트
function updateUserName(newName) {
  userName = newName;
  $('#user-name').text(newName);  // 직접 업데이트 필요
}

// 함수 매개변수로 데이터 전달
function createUserCard(name, age, email) {
  var html = '<div>' +
             '  <h3>' + name + '</h3>' +
             '  <p>' + age + '</p>' +
             '  <p>' + email + '</p>' +
             '</div>';
  return html;
}
```

**문제점:**
- 데이터와 UI가 동기화되지 않음
- 데이터가 어디에 있는지 파악하기 어려움
- 데이터 변경 시 직접 DOM을 업데이트해야 함
- 전역 변수 사용으로 충돌 위험

## React의 데이터 관리: Props와 State

React는 두 가지 방식으로 데이터를 관리합니다:

1. **Props (Properties)**: 부모 → 자식으로 전달되는 데이터 (읽기 전용)
2. **State**: 컴포넌트 내부에서 관리하는 데이터 (변경 가능)

```
┌─────────────────┐
│  Parent 부모    │
│  state: name    │  ← State (변경 가능)
└────────┬────────┘
         │ props
         ↓
┌─────────────────┐
│  Child 자식     │
│  받은 name 사용 │  ← Props (읽기 전용)
└─────────────────┘
```

## Props (Properties)

Props는 **컴포넌트에 전달하는 인자**입니다. 함수의 매개변수와 비슷합니다.

### 기본 사용법

```javascript
// 1. Props 전달
const App = () => {
  return (
    <Greeting name="홍길동" age={25} />
  );
};

// 2. Props 받기
const Greeting = (props) => {
  return (
    <div>
      <h1>안녕하세요, {props.name}님!</h1>
      <p>나이: {props.age}세</p>
    </div>
  );
};

// 3. 구조 분해 할당 (추천)
const Greeting = ({ name, age }) => {
  return (
    <div>
      <h1>안녕하세요, {name}님!</h1>
      <p>나이: {age}세</p>
    </div>
  );
};
```

### 다양한 Props 타입

```javascript
const Profile = ({
  name,        // 문자열
  age,         // 숫자
  isStudent,   // 불리언
  hobbies,     // 배열
  address,     // 객체
  onSave       // 함수
}) => {
  return (
    <div>
      <h2>{name}</h2>
      <p>나이: {age}</p>
      <p>학생: {isStudent ? '예' : '아니오'}</p>
      <ul>
        {hobbies.map((hobby, index) => (
          <li key={index}>{hobby}</li>
        ))}
      </ul>
      <p>주소: {address.city}</p>
      <button onClick={onSave}>저장</button>
    </div>
  );
};

// 사용
const App = () => {
  const handleSave = () => alert('저장!');

  return (
    <Profile
      name="홍길동"
      age={25}
      isStudent={true}
      hobbies={['독서', '운동', '요리']}
      address={{ city: '서울', zipCode: '12345' }}
      onSave={handleSave}
    />
  );
};
```

### Props의 특징

1. **읽기 전용 (Read-only)**

```javascript
const Greeting = ({ name }) => {
  // ❌ Props는 변경할 수 없음
  // name = "김철수";  // 에러!

  // ✅ Props는 읽기만 가능
  return <h1>{name}</h1>;
};
```

2. **단방향 데이터 흐름 (One-way Data Flow)**

```javascript
// 부모 → 자식으로만 전달 가능
const Parent = () => {
  const data = "데이터";

  return (
    <Child data={data} />  // ✅ 부모 → 자식
  );
};

// 자식은 부모의 데이터를 직접 변경할 수 없음
// 함수를 Props로 받아서 간접적으로 변경 요청
```

3. **기본값 설정 가능**

```javascript
const Greeting = ({ name = "손님", age = 0 }) => {
  return (
    <div>
      <h1>안녕하세요, {name}님!</h1>
      <p>나이: {age}세</p>
    </div>
  );
};

// Props 없이 사용
<Greeting />
// 결과: 안녕하세요, 손님님! 나이: 0세

// Props와 함께 사용
<Greeting name="홍길동" age={25} />
// 결과: 안녕하세요, 홍길동님! 나이: 25세
```

## State

State는 **컴포넌트의 상태를 나타내는 데이터**입니다. 변경 가능하며, 변경 시 자동으로 UI가 업데이트됩니다.

### useState Hook

```javascript
import { useState } from 'react';

const Counter = () => {
  // [상태 값, 상태 변경 함수] = useState(초기값)
  const [count, setCount] = useState(0);

  const increment = () => {
    setCount(count + 1);  // 상태 변경
  };

  return (
    <div>
      <p>카운트: {count}</p>
      <button onClick={increment}>증가</button>
    </div>
  );
};
```

**중요:**
- `count`는 직접 변경하지 않음
- `setCount()` 함수로만 변경
- 상태가 변경되면 자동으로 리렌더링

### jQuery vs React State

```javascript
// jQuery 방식
var count = 0;

$('#increment').click(function() {
  count++;
  $('#count').text(count);  // 수동 업데이트
});

// React 방식
const Counter = () => {
  const [count, setCount] = useState(0);

  return (
    <div>
      <p>카운트: {count}</p>
      <button onClick={() => setCount(count + 1)}>
        증가
      </button>
    </div>
  );
};
// 상태 변경 시 자동으로 UI 업데이트!
```

### 다양한 State 예제

#### 1. 문자열 State

```javascript
const NameInput = () => {
  const [name, setName] = useState('');

  return (
    <div>
      <input
        type="text"
        value={name}
        onChange={(e) => setName(e.target.value)}
      />
      <p>입력한 이름: {name}</p>
    </div>
  );
};
```

#### 2. 불리언 State

```javascript
const Toggle = () => {
  const [isOn, setIsOn] = useState(false);

  return (
    <div>
      <p>상태: {isOn ? 'ON' : 'OFF'}</p>
      <button onClick={() => setIsOn(!isOn)}>
        토글
      </button>
    </div>
  );
};
```

#### 3. 배열 State

```javascript
const TodoList = () => {
  const [todos, setTodos] = useState([
    { id: 1, text: '공부하기', done: false },
    { id: 2, text: '운동하기', done: false }
  ]);

  const addTodo = (text) => {
    const newTodo = {
      id: Date.now(),
      text: text,
      done: false
    };
    setTodos([...todos, newTodo]);  // 배열에 추가
  };

  const removeTodo = (id) => {
    setTodos(todos.filter(todo => todo.id !== id));  // 배열에서 제거
  };

  const toggleTodo = (id) => {
    setTodos(
      todos.map(todo =>
        todo.id === id
          ? { ...todo, done: !todo.done }  // 특정 항목 수정
          : todo
      )
    );
  };

  return (
    <div>
      {todos.map(todo => (
        <div key={todo.id}>
          <input
            type="checkbox"
            checked={todo.done}
            onChange={() => toggleTodo(todo.id)}
          />
          <span>{todo.text}</span>
          <button onClick={() => removeTodo(todo.id)}>삭제</button>
        </div>
      ))}
    </div>
  );
};
```

#### 4. 객체 State

```javascript
const UserForm = () => {
  const [user, setUser] = useState({
    name: '',
    email: '',
    age: 0
  });

  const updateField = (field, value) => {
    setUser({
      ...user,      // 기존 값 유지
      [field]: value  // 특정 필드만 변경
    });
  };

  return (
    <div>
      <input
        type="text"
        value={user.name}
        onChange={(e) => updateField('name', e.target.value)}
        placeholder="이름"
      />
      <input
        type="email"
        value={user.email}
        onChange={(e) => updateField('email', e.target.value)}
        placeholder="이메일"
      />
      <input
        type="number"
        value={user.age}
        onChange={(e) => updateField('age', Number(e.target.value))}
        placeholder="나이"
      />
      <p>
        이름: {user.name}, 이메일: {user.email}, 나이: {user.age}
      </p>
    </div>
  );
};
```

### State 업데이트 주의사항

#### 1. State는 불변성을 지켜야 함

```javascript
const TodoList = () => {
  const [todos, setTodos] = useState([/* ... */]);

  // ❌ 잘못된 방법 - 직접 변경
  const addTodoWrong = (text) => {
    todos.push({ text });  // 원본 배열 직접 수정 (안 됨!)
    setTodos(todos);
  };

  // ✅ 올바른 방법 - 새 배열 생성
  const addTodoRight = (text) => {
    setTodos([...todos, { text }]);  // 새 배열 생성
  };

  // ❌ 잘못된 방법 - 객체 직접 변경
  const updateUserWrong = () => {
    user.name = "새 이름";  // 직접 변경 (안 됨!)
    setUser(user);
  };

  // ✅ 올바른 방법 - 새 객체 생성
  const updateUserRight = () => {
    setUser({ ...user, name: "새 이름" });  // 새 객체 생성
  };
};
```

#### 2. 이전 State 값 사용하기

```javascript
const Counter = () => {
  const [count, setCount] = useState(0);

  // ❌ 문제 발생 가능
  const increment = () => {
    setCount(count + 1);
    setCount(count + 1);  // 2가 아니라 1 증가
  };

  // ✅ 안전한 방법
  const incrementSafe = () => {
    setCount(prevCount => prevCount + 1);
    setCount(prevCount => prevCount + 1);  // 정확히 2 증가
  };
};
```

## Props와 State 함께 사용하기

부모 컴포넌트의 State를 자식 컴포넌트의 Props로 전달합니다.

```javascript
// 자식 컴포넌트 - Props로 받음
const TodoItem = ({ todo, onToggle, onDelete }) => {
  return (
    <div>
      <input
        type="checkbox"
        checked={todo.done}
        onChange={() => onToggle(todo.id)}
      />
      <span>{todo.text}</span>
      <button onClick={() => onDelete(todo.id)}>삭제</button>
    </div>
  );
};

// 부모 컴포넌트 - State 관리
const TodoApp = () => {
  const [todos, setTodos] = useState([
    { id: 1, text: '공부하기', done: false },
    { id: 2, text: '운동하기', done: false }
  ]);

  const toggleTodo = (id) => {
    setTodos(
      todos.map(todo =>
        todo.id === id ? { ...todo, done: !todo.done } : todo
      )
    );
  };

  const deleteTodo = (id) => {
    setTodos(todos.filter(todo => todo.id !== id));
  };

  return (
    <div>
      <h1>할 일 목록</h1>
      {todos.map(todo => (
        <TodoItem
          key={todo.id}
          todo={todo}
          onToggle={toggleTodo}
          onDelete={deleteTodo}
        />
      ))}
    </div>
  );
};
```

**데이터 흐름:**
```
TodoApp (State)
  ├── todos (State)
  ├── toggleTodo (함수)
  └── deleteTodo (함수)
       ↓ Props로 전달
TodoItem
  ├── todo (Props)
  ├── onToggle (Props)
  └── onDelete (Props)
```

## 실전 예제: 쇼핑 카트

```javascript
// ProductCard.js
const ProductCard = ({ product, onAddToCart }) => {
  return (
    <div className="product-card">
      <img src={product.image} alt={product.name} />
      <h3>{product.name}</h3>
      <p>{product.price.toLocaleString()}원</p>
      <button onClick={() => onAddToCart(product)}>
        장바구니 담기
      </button>
    </div>
  );
};

// Cart.js
const Cart = ({ items, onRemove }) => {
  const total = items.reduce((sum, item) => sum + item.price, 0);

  return (
    <div className="cart">
      <h2>장바구니</h2>
      {items.length === 0 ? (
        <p>장바구니가 비어있습니다</p>
      ) : (
        <>
          {items.map((item, index) => (
            <div key={index} className="cart-item">
              <span>{item.name}</span>
              <span>{item.price.toLocaleString()}원</span>
              <button onClick={() => onRemove(index)}>삭제</button>
            </div>
          ))}
          <div className="total">
            <strong>합계: {total.toLocaleString()}원</strong>
          </div>
        </>
      )}
    </div>
  );
};

// App.js
const App = () => {
  const [products] = useState([
    { id: 1, name: '노트북', price: 1200000, image: 'laptop.jpg' },
    { id: 2, name: '마우스', price: 35000, image: 'mouse.jpg' },
    { id: 3, name: '키보드', price: 80000, image: 'keyboard.jpg' }
  ]);

  const [cartItems, setCartItems] = useState([]);

  const addToCart = (product) => {
    setCartItems([...cartItems, product]);
    alert(`${product.name}을(를) 장바구니에 담았습니다!`);
  };

  const removeFromCart = (index) => {
    setCartItems(cartItems.filter((_, i) => i !== index));
  };

  return (
    <div className="app">
      <h1>쇼핑몰</h1>
      <div className="products">
        {products.map(product => (
          <ProductCard
            key={product.id}
            product={product}
            onAddToCart={addToCart}
          />
        ))}
      </div>
      <Cart items={cartItems} onRemove={removeFromCart} />
    </div>
  );
};
```

## 자주 묻는 질문

### Q: Props와 State의 차이는 뭔가요?

| 구분 | Props | State |
|------|-------|-------|
| 정의 | 부모가 전달한 데이터 | 컴포넌트 내부 데이터 |
| 변경 | ❌ 불가능 (읽기 전용) | ✅ 가능 (setState 사용) |
| 사용처 | 모든 컴포넌트 | 해당 컴포넌트 내부 |
| 목적 | 데이터 전달 | 상태 관리 |

### Q: 언제 Props를 쓰고 언제 State를 쓰나요?

A: 간단한 기준:
- **Props**: 부모에서 받은 데이터, 변경하지 않을 데이터
- **State**: 컴포넌트 내에서 변경되는 데이터

```javascript
// Props 사용
const UserInfo = ({ name, email }) => {
  // name, email은 변경하지 않음
  return <div>{name}: {email}</div>;
};

// State 사용
const Counter = () => {
  const [count, setCount] = useState(0);
  // count는 변경됨
  return <button onClick={() => setCount(count + 1)}>{count}</button>;
};
```

### Q: State를 직접 변경하면 안 되나요?

A: 절대 안 됩니다! 항상 `setState` 함수를 사용해야 합니다.

```javascript
const [count, setCount] = useState(0);

// ❌ 잘못된 방법
count = count + 1;  // 에러! 리렌더링 안 됨

// ✅ 올바른 방법
setCount(count + 1);  // 리렌더링됨
```

### Q: 여러 개의 State를 사용해도 되나요?

A: 네! 필요한 만큼 사용하세요.

```javascript
const Form = () => {
  const [name, setName] = useState('');
  const [email, setEmail] = useState('');
  const [age, setAge] = useState(0);
  const [agreed, setAgreed] = useState(false);

  // ...
};

// 또는 객체로 관리
const Form = () => {
  const [formData, setFormData] = useState({
    name: '',
    email: '',
    age: 0,
    agreed: false
  });

  // ...
};
```

### Q: Props를 자식에서 변경하려면?

A: 부모로부터 함수를 Props로 받아서 호출합니다.

```javascript
// 부모
const Parent = () => {
  const [count, setCount] = useState(0);

  return (
    <Child count={count} onIncrement={() => setCount(count + 1)} />
  );
};

// 자식
const Child = ({ count, onIncrement }) => {
  return (
    <div>
      <p>카운트: {count}</p>
      <button onClick={onIncrement}>증가</button>
    </div>
  );
};
```

## 연습 문제

사용자 정보를 표시하고 수정할 수 있는 컴포넌트를 만들어보세요:

**요구사항:**
1. 이름, 이메일, 나이를 입력받는 폼
2. 입력한 정보를 실시간으로 표시
3. "저장" 버튼 클릭 시 알림 표시

### 정답

```javascript
const UserProfile = () => {
  const [user, setUser] = useState({
    name: '',
    email: '',
    age: ''
  });

  const handleChange = (field, value) => {
    setUser({
      ...user,
      [field]: value
    });
  };

  const handleSave = () => {
    if (!user.name || !user.email || !user.age) {
      alert('모든 필드를 입력해주세요!');
      return;
    }
    alert(`저장됨!\n이름: ${user.name}\n이메일: ${user.email}\n나이: ${user.age}`);
  };

  return (
    <div>
      <h2>사용자 프로필</h2>

      <div>
        <input
          type="text"
          placeholder="이름"
          value={user.name}
          onChange={(e) => handleChange('name', e.target.value)}
        />
      </div>

      <div>
        <input
          type="email"
          placeholder="이메일"
          value={user.email}
          onChange={(e) => handleChange('email', e.target.value)}
        />
      </div>

      <div>
        <input
          type="number"
          placeholder="나이"
          value={user.age}
          onChange={(e) => handleChange('age', e.target.value)}
        />
      </div>

      <div>
        <h3>입력한 정보:</h3>
        <p>이름: {user.name || '(없음)'}</p>
        <p>이메일: {user.email || '(없음)'}</p>
        <p>나이: {user.age || '(없음)'}</p>
      </div>

      <button onClick={handleSave}>저장</button>
    </div>
  );
};
```

## 요약

| 개념 | 설명 | 변경 가능 | 사용 예 |
|------|------|----------|---------|
| **Props** | 부모 → 자식 데이터 전달 | ❌ 읽기 전용 | 사용자 이름, 설정값 |
| **State** | 컴포넌트 내부 상태 | ✅ setState로 변경 | 카운터, 폼 입력 |

**핵심 원칙:**
1. Props는 읽기 전용, State는 변경 가능
2. State 변경은 반드시 `setState` 함수 사용
3. State 업데이트 시 불변성 유지 (새 객체/배열 생성)
4. 부모의 State → 자식의 Props로 전달
5. 자식이 부모 데이터를 변경하려면 함수를 Props로 받음

**다음 단계:**
Props와 State를 이해했다면 이벤트 핸들링을 배울 준비가 되었습니다!
