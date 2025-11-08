# 컴포넌트 (Components)

## jQuery 시대의 코드 재사용

jQuery에서는 함수로 HTML을 생성하고 재사용했습니다.

```javascript
// 버튼 생성 함수
function createButton(text, className) {
  return '<button class="btn ' + className + '">' + text + '</button>';
}

// 사용
$('#app').append(createButton('저장', 'btn-primary'));
$('#app').append(createButton('취소', 'btn-secondary'));

// 사용자 카드 생성 함수
function createUserCard(user) {
  var html = '<div class="user-card">' +
             '  <img src="' + user.avatar + '" />' +
             '  <h3>' + user.name + '</h3>' +
             '  <p>' + user.email + '</p>' +
             '</div>';
  return html;
}

// 사용
var users = [/* 사용자 배열 */];
users.forEach(function(user) {
  $('#users').append(createUserCard(user));
});
```

**문제점:**
- 함수는 HTML 문자열만 반환
- 상태(state)를 관리하기 어려움
- 라이프사이클(생성, 업데이트, 제거) 관리 불가
- 재사용 가능하지만 구조화되지 않음

## 컴포넌트란?

컴포넌트는 **UI를 독립적이고 재사용 가능한 조각으로 나눈 것**입니다.

레고 블록처럼 작은 조각들을 조합하여 복잡한 UI를 만듭니다.

```
앱 전체
  ├── 헤더
  │   ├── 로고
  │   └── 네비게이션
  ├── 메인 콘텐츠
  │   ├── 사이드바
  │   └── 게시글 목록
  │       ├── 게시글 카드
  │       ├── 게시글 카드
  │       └── 게시글 카드
  └── 푸터
```

각 부분이 하나의 컴포넌트입니다!

## 함수 컴포넌트

React에서는 함수가 컴포넌트입니다.

### 가장 간단한 컴포넌트

```javascript
// 함수 선언식
function Welcome() {
  return <h1>안녕하세요!</h1>;
}

// 화살표 함수
const Welcome = () => {
  return <h1>안녕하세요!</h1>;
};

// 한 줄이면 return과 중괄호 생략 가능
const Welcome = () => <h1>안녕하세요!</h1>;
```

**규칙:**
1. 함수 이름은 **대문자로 시작** (Pascal Case)
2. JSX를 반환해야 함
3. 하나의 최상위 요소를 반환

### 컴포넌트 사용하기

```javascript
// 컴포넌트 정의
const Greeting = () => {
  return <h1>안녕하세요!</h1>;
};

// 컴포넌트 사용 (HTML 태그처럼)
const App = () => {
  return (
    <div>
      <Greeting />
      <Greeting />
      <Greeting />
    </div>
  );
};

// 결과:
// <div>
//   <h1>안녕하세요!</h1>
//   <h1>안녕하세요!</h1>
//   <h1>안녕하세요!</h1>
// </div>
```

## jQuery vs React 컴포넌트 비교

### 버튼 컴포넌트

```javascript
// jQuery 방식
function createButton(text, type, onClick) {
  var className = 'btn btn-' + type;
  var $button = $('<button>')
    .addClass(className)
    .text(text)
    .click(onClick);
  return $button;
}

// 사용
var saveButton = createButton('저장', 'primary', function() {
  alert('저장!');
});
var cancelButton = createButton('취소', 'secondary', function() {
  alert('취소!');
});

$('#app').append(saveButton).append(cancelButton);

// React 방식
const Button = ({ text, type, onClick }) => {
  return (
    <button className={`btn btn-${type}`} onClick={onClick}>
      {text}
    </button>
  );
};

// 사용
const App = () => {
  const handleSave = () => alert('저장!');
  const handleCancel = () => alert('취소!');

  return (
    <div>
      <Button text="저장" type="primary" onClick={handleSave} />
      <Button text="취소" type="secondary" onClick={handleCancel} />
    </div>
  );
};
```

### 사용자 카드 컴포넌트

```javascript
// jQuery 방식
function createUserCard(user) {
  var $card = $('<div>').addClass('user-card');

  var $avatar = $('<img>')
    .attr('src', user.avatar)
    .attr('alt', user.name);

  var $name = $('<h3>').text(user.name);
  var $email = $('<p>').text(user.email);

  var $button = $('<button>')
    .addClass('btn-message')
    .text('메시지 보내기')
    .click(function() {
      alert(user.name + '에게 메시지 보내기');
    });

  $card.append($avatar, $name, $email, $button);
  return $card;
}

// React 방식
const UserCard = ({ user }) => {
  const handleMessage = () => {
    alert(`${user.name}에게 메시지 보내기`);
  };

  return (
    <div className="user-card">
      <img src={user.avatar} alt={user.name} />
      <h3>{user.name}</h3>
      <p>{user.email}</p>
      <button className="btn-message" onClick={handleMessage}>
        메시지 보내기
      </button>
    </div>
  );
};

// 사용
const App = () => {
  const users = [
    { id: 1, name: '홍길동', email: 'hong@example.com', avatar: 'avatar1.jpg' },
    { id: 2, name: '김철수', email: 'kim@example.com', avatar: 'avatar2.jpg' }
  ];

  return (
    <div>
      {users.map(user => (
        <UserCard key={user.id} user={user} />
      ))}
    </div>
  );
};
```

## 컴포넌트 합성 (Composition)

컴포넌트 안에 다른 컴포넌트를 포함할 수 있습니다.

```javascript
// 작은 컴포넌트들
const Avatar = ({ src, alt }) => {
  return <img src={src} alt={alt} className="avatar" />;
};

const UserName = ({ name }) => {
  return <h3 className="user-name">{name}</h3>;
};

const UserEmail = ({ email }) => {
  return <p className="user-email">{email}</p>;
};

const MessageButton = ({ userName, onClick }) => {
  return (
    <button className="btn-message" onClick={onClick}>
      {userName}에게 메시지 보내기
    </button>
  );
};

// 큰 컴포넌트로 합성
const UserCard = ({ user }) => {
  const handleMessage = () => {
    alert(`${user.name}에게 메시지 보내기`);
  };

  return (
    <div className="user-card">
      <Avatar src={user.avatar} alt={user.name} />
      <UserName name={user.name} />
      <UserEmail email={user.email} />
      <MessageButton userName={user.name} onClick={handleMessage} />
    </div>
  );
};
```

**장점:**
- 각 컴포넌트가 하나의 역할만 수행
- 테스트하기 쉬움
- 재사용하기 쉬움
- 코드 이해가 쉬움

## 컴포넌트 분리 기준

### 언제 컴포넌트로 분리할까?

1. **재사용되는 경우**
   ```javascript
   // Button을 여러 곳에서 사용
   const Button = ({ children, onClick }) => (
     <button className="btn" onClick={onClick}>
       {children}
     </button>
   );
   ```

2. **독립적인 기능을 가진 경우**
   ```javascript
   const SearchBar = () => {
     const [query, setQuery] = useState('');
     // 검색 로직...
     return <input value={query} onChange={...} />;
   };
   ```

3. **UI가 복잡한 경우**
   ```javascript
   // 복잡한 폼을 여러 컴포넌트로 분리
   const UserForm = () => (
     <form>
       <PersonalInfo />
       <AddressInfo />
       <ContactInfo />
     </form>
   );
   ```

### 너무 작게 나누지 않기

```javascript
// ❌ 과도하게 분리 (비추천)
const Heading = ({ text }) => <h1>{text}</h1>;
const Paragraph = ({ text }) => <p>{text}</p>;

const Article = () => (
  <article>
    <Heading text="제목" />
    <Paragraph text="내용" />
  </article>
);

// ✅ 적절한 분리 (추천)
const Article = () => (
  <article>
    <h1>제목</h1>
    <p>내용</p>
  </article>
);
```

## 컴포넌트 파일 구조

### 파일 분리

각 컴포넌트를 별도 파일로 분리합니다.

```
src/
├── components/
│   ├── Button.js
│   ├── UserCard.js
│   └── SearchBar.js
├── App.js
└── index.js
```

**Button.js:**
```javascript
const Button = ({ text, type, onClick }) => {
  return (
    <button className={`btn btn-${type}`} onClick={onClick}>
      {text}
    </button>
  );
};

export default Button;
```

**App.js:**
```javascript
import Button from './components/Button';

const App = () => {
  return (
    <div>
      <Button text="클릭" type="primary" onClick={() => alert('클릭!')} />
    </div>
  );
};

export default App;
```

### 폴더 구조 (중규모 프로젝트)

```
src/
├── components/
│   ├── common/          # 공통 컴포넌트
│   │   ├── Button.js
│   │   ├── Input.js
│   │   └── Card.js
│   ├── user/            # 사용자 관련
│   │   ├── UserCard.js
│   │   ├── UserList.js
│   │   └── UserProfile.js
│   └── layout/          # 레이아웃
│       ├── Header.js
│       ├── Footer.js
│       └── Sidebar.js
├── App.js
└── index.js
```

## 실전 예제

### 제품 목록 앱

```javascript
// ProductCard.js
const ProductCard = ({ product }) => {
  const { name, price, image, inStock } = product;

  return (
    <div className={`product-card ${!inStock ? 'out-of-stock' : ''}`}>
      <img src={image} alt={name} />
      <h3>{name}</h3>
      <p className="price">{price.toLocaleString()}원</p>
      {inStock ? (
        <button className="btn-buy">구매하기</button>
      ) : (
        <p className="out-of-stock-text">품절</p>
      )}
    </div>
  );
};

// ProductList.js
const ProductList = ({ products }) => {
  return (
    <div className="product-list">
      {products.map(product => (
        <ProductCard key={product.id} product={product} />
      ))}
    </div>
  );
};

// App.js
const App = () => {
  const products = [
    { id: 1, name: '노트북', price: 1200000, image: 'laptop.jpg', inStock: true },
    { id: 2, name: '마우스', price: 35000, image: 'mouse.jpg', inStock: false },
    { id: 3, name: '키보드', price: 80000, image: 'keyboard.jpg', inStock: true }
  ];

  return (
    <div className="app">
      <h1>상품 목록</h1>
      <ProductList products={products} />
    </div>
  );
};
```

### 블로그 게시글

```javascript
// PostHeader.js
const PostHeader = ({ title, author, date }) => {
  return (
    <header className="post-header">
      <h1>{title}</h1>
      <div className="post-meta">
        <span className="author">{author}</span>
        <span className="date">{date}</span>
      </div>
    </header>
  );
};

// PostContent.js
const PostContent = ({ content }) => {
  return (
    <div className="post-content">
      <p>{content}</p>
    </div>
  );
};

// PostFooter.js
const PostFooter = ({ likes, comments }) => {
  return (
    <footer className="post-footer">
      <button className="btn-like">좋아요 {likes}</button>
      <button className="btn-comment">댓글 {comments}</button>
      <button className="btn-share">공유</button>
    </footer>
  );
};

// Post.js - 컴포넌트 합성
const Post = ({ post }) => {
  return (
    <article className="post">
      <PostHeader
        title={post.title}
        author={post.author}
        date={post.date}
      />
      <PostContent content={post.content} />
      <PostFooter likes={post.likes} comments={post.comments} />
    </article>
  );
};

// App.js
const App = () => {
  const post = {
    id: 1,
    title: 'React 시작하기',
    author: '홍길동',
    date: '2024-01-01',
    content: 'React는 UI를 만들기 위한 JavaScript 라이브러리입니다...',
    likes: 42,
    comments: 15
  };

  return (
    <div className="app">
      <Post post={post} />
    </div>
  );
};
```

## children prop

컴포넌트 태그 사이의 내용을 받을 수 있습니다.

```javascript
// Container 컴포넌트
const Container = ({ children }) => {
  return (
    <div className="container">
      {children}
    </div>
  );
};

// 사용
const App = () => {
  return (
    <Container>
      <h1>제목</h1>
      <p>내용입니다.</p>
      <button>버튼</button>
    </Container>
  );
};

// 결과:
// <div className="container">
//   <h1>제목</h1>
//   <p>내용입니다.</p>
//   <button>버튼</button>
// </div>
```

**실전 활용:**

```javascript
const Card = ({ children, title }) => {
  return (
    <div className="card">
      {title && <h2 className="card-title">{title}</h2>}
      <div className="card-body">
        {children}
      </div>
    </div>
  );
};

const App = () => {
  return (
    <div>
      <Card title="사용자 정보">
        <p>이름: 홍길동</p>
        <p>이메일: hong@example.com</p>
      </Card>

      <Card title="알림">
        <p>새 메시지가 3개 있습니다.</p>
        <button>확인</button>
      </Card>
    </div>
  );
};
```

## 컴포넌트 네이밍 규칙

```javascript
// ✅ 좋은 이름 (명확하고 의미있음)
const UserProfile = () => { /* ... */ };
const ProductCard = () => { /* ... */ };
const SearchBar = () => { /* ... */ };
const NavigationMenu = () => { /* ... */ };

// ❌ 나쁜 이름 (불명확)
const Component1 = () => { /* ... */ };
const Thing = () => { /* ... */ };
const MyComponent = () => { /* ... */ };

// 파일명도 컴포넌트 이름과 동일하게
// UserProfile.js
// ProductCard.js
// SearchBar.js
```

## 자주 묻는 질문

### Q: 함수 컴포넌트와 클래스 컴포넌트 중 뭘 써야 하나요?

A: **함수 컴포넌트**를 사용하세요.
- 최신 React는 함수 컴포넌트 + Hooks 권장
- 더 간결하고 이해하기 쉬움
- 클래스 컴포넌트는 레거시 코드에서만 볼 수 있음

### Q: 한 파일에 여러 컴포넌트를 만들어도 되나요?

A: 작은 컴포넌트는 가능하지만, 기본적으로는 한 파일에 하나가 원칙입니다.

```javascript
// ✅ 작은 컴포넌트는 같은 파일에
const UserCard = ({ user }) => {
  return (
    <div className="user-card">
      <Avatar src={user.avatar} />
      <UserInfo name={user.name} email={user.email} />
    </div>
  );
};

const Avatar = ({ src }) => <img src={src} className="avatar" />;
const UserInfo = ({ name, email }) => (
  <div>
    <h3>{name}</h3>
    <p>{email}</p>
  </div>
);

export default UserCard;

// ❌ 큰 컴포넌트는 파일 분리
// UserCard.js, Avatar.js, UserInfo.js로 분리
```

### Q: 컴포넌트 이름을 소문자로 시작하면 안 되나요?

A: React는 대문자로 시작하는 이름을 컴포넌트로 인식합니다.

```javascript
// ❌ 작동 안 함
const myButton = () => <button>클릭</button>;
<myButton />  // HTML <mybutton> 태그로 인식됨

// ✅ 작동함
const MyButton = () => <button>클릭</button>;
<MyButton />  // React 컴포넌트로 인식됨
```

### Q: 어떻게 컴포넌트를 작게 유지할 수 있나요?

A: 단일 책임 원칙을 따르세요.
- 한 컴포넌트는 한 가지 일만
- 100줄 이상이면 분리 고려
- 재사용 가능한 부분은 별도 컴포넌트로

## 연습 문제

다음 요구사항에 맞는 컴포넌트를 만들어보세요:

**요구사항:**
1. TodoItem 컴포넌트: 할 일 항목 표시
   - 할 일 텍스트
   - 완료 여부 체크박스
   - 삭제 버튼

2. TodoList 컴포넌트: 할 일 목록 표시
   - TodoItem 여러 개 렌더링

3. App 컴포넌트: 전체 앱
   - 제목
   - TodoList

### 정답

```javascript
// TodoItem.js
const TodoItem = ({ todo }) => {
  return (
    <div className="todo-item">
      <input
        type="checkbox"
        checked={todo.done}
        onChange={() => {/* 나중에 구현 */}}
      />
      <span className={todo.done ? 'done' : ''}>
        {todo.text}
      </span>
      <button className="btn-delete">삭제</button>
    </div>
  );
};

export default TodoItem;

// TodoList.js
import TodoItem from './TodoItem';

const TodoList = ({ todos }) => {
  return (
    <div className="todo-list">
      {todos.map(todo => (
        <TodoItem key={todo.id} todo={todo} />
      ))}
    </div>
  );
};

export default TodoList;

// App.js
import TodoList from './components/TodoList';

const App = () => {
  const todos = [
    { id: 1, text: 'React 공부하기', done: false },
    { id: 2, text: '운동하기', done: true },
    { id: 3, text: '장보기', done: false }
  ];

  return (
    <div className="app">
      <h1>할 일 목록</h1>
      <TodoList todos={todos} />
    </div>
  );
};

export default App;
```

## 요약

| 구분 | jQuery 함수 | React 컴포넌트 |
|------|------------|---------------|
| 정의 | 함수 | 함수 (대문자 시작) |
| 반환 | HTML 문자열/jQuery 객체 | JSX |
| 재사용 | 함수 호출 | `<Component />` |
| 상태 관리 | 어려움 | Hooks로 쉬움 |
| 합성 | 어려움 | 매우 쉬움 |
| 파일 구조 | 자유로움 | 파일 분리 권장 |

**핵심 원칙:**
1. 컴포넌트는 대문자로 시작
2. 하나의 역할만 수행 (단일 책임)
3. 재사용 가능하게 설계
4. Props로 데이터 전달
5. 작은 컴포넌트를 조합하여 큰 컴포넌트 생성

**다음 단계:**
컴포넌트를 이해했다면 Props와 State를 배울 준비가 되었습니다!
