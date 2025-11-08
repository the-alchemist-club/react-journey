# JSX (JavaScript XML)

## jQuery 시대의 HTML 생성

jQuery를 사용할 때는 문자열로 HTML을 만들었습니다.

```javascript
// HTML 문자열로 만들기
var userName = "홍길동";
var userAge = 25;

var html = '<div class="user-card">' +
           '  <h2>' + userName + '</h2>' +
           '  <p>나이: ' + userAge + '세</p>' +
           '  <button class="btn">프로필 보기</button>' +
           '</div>';

$('#app').html(html);

// 또는 템플릿 리터럴로
const html = `
  <div class="user-card">
    <h2>${userName}</h2>
    <p>나이: ${userAge}세</p>
    <button class="btn">프로필 보기</button>
  </div>
`;
```

**문제점:**
- 문자열이라 문법 검사가 안 됨
- IDE의 자동 완성이 작동하지 않음
- 오타를 찾기 어려움
- JavaScript 로직과 HTML이 섞여서 복잡함

## JSX란?

JSX는 JavaScript 안에서 HTML처럼 보이는 코드를 작성할 수 있게 해주는 문법입니다.

```javascript
// JSX
const userName = "홍길동";
const userAge = 25;

const element = (
  <div className="user-card">
    <h2>{userName}</h2>
    <p>나이: {userAge}세</p>
    <button className="btn">프로필 보기</button>
  </div>
);
```

**특징:**
- HTML처럼 보이지만 JavaScript 표현식
- 빌드 시 일반 JavaScript로 변환됨
- 문법 검사, 자동 완성 지원
- 더 직관적이고 읽기 쉬움

## JSX는 실제로 어떻게 동작할까?

JSX는 컴파일되면 `React.createElement()` 함수 호출로 변환됩니다.

```javascript
// JSX 코드
const element = <h1 className="greeting">안녕하세요!</h1>;

// 변환된 JavaScript 코드
const element = React.createElement(
  'h1',
  { className: 'greeting' },
  '안녕하세요!'
);

// 최종 생성되는 객체
const element = {
  type: 'h1',
  props: {
    className: 'greeting',
    children: '안녕하세요!'
  }
};
```

**중요:** JSX를 사용하려면 빌드 도구(Babel, Webpack 등)가 필요합니다. Create React App을 사용하면 자동으로 설정됩니다.

## JSX 기본 문법

### 1. JavaScript 표현식 삽입

중괄호 `{}`를 사용하여 JavaScript 표현식을 넣을 수 있습니다.

```javascript
const name = "홍길동";
const age = 25;

const element = (
  <div>
    <h1>이름: {name}</h1>
    <p>나이: {age}세</p>
    <p>내년 나이: {age + 1}세</p>
    <p>성인 여부: {age >= 18 ? '성인' : '미성년자'}</p>
  </div>
);
```

### 2. 속성(Attributes) 지정

HTML 속성을 지정하는 방식과 비슷하지만 camelCase를 사용합니다.

```javascript
// HTML 속성
const element = (
  <div>
    <img src="image.jpg" alt="설명" />
    <input type="text" value="초기값" />
    <button onClick={handleClick}>클릭</button>
  </div>
);

// JavaScript 표현식도 가능
const imageUrl = "https://example.com/image.jpg";
const altText = "프로필 사진";

const element = <img src={imageUrl} alt={altText} />;
```

**주의:** HTML과 다른 점
- `class` → `className`
- `for` → `htmlFor`
- `onclick` → `onClick`
- `tabindex` → `tabIndex`

### 3. 자식 요소(Children)

```javascript
// 단일 자식
const element = <div>안녕하세요</div>;

// 여러 자식
const element = (
  <div>
    <h1>제목</h1>
    <p>내용</p>
    <button>버튼</button>
  </div>
);

// 자식이 없는 태그는 자기 자신을 닫음
const element = <img src="image.jpg" />;
const element = <input type="text" />;
```

### 4. 반드시 하나의 부모 요소로 감싸기

```javascript
// 잘못된 예 - 여러 최상위 요소
const element = (
  <h1>제목</h1>
  <p>내용</p>
);

// 올바른 예 - 하나의 부모로 감싸기
const element = (
  <div>
    <h1>제목</h1>
    <p>내용</p>
  </div>
);

// Fragment 사용 (불필요한 div 제거)
const element = (
  <>
    <h1>제목</h1>
    <p>내용</p>
  </>
);

// 또는
const element = (
  <React.Fragment>
    <h1>제목</h1>
    <p>내용</p>
  </React.Fragment>
);
```

### 5. 주석

```javascript
const element = (
  <div>
    {/* 이것은 주석입니다 */}
    <h1>제목</h1>

    {/*
      여러 줄
      주석도 가능
    */}
    <p>내용</p>
  </div>
);

// JSX 밖에서는 일반 JavaScript 주석
// 이것은 주석
/* 이것도 주석 */
```

## jQuery vs JSX 비교

### 동적 콘텐츠 생성

```javascript
// jQuery 방식
var users = [
  { id: 1, name: "홍길동", age: 25 },
  { id: 2, name: "김철수", age: 30 }
];

var html = '';
users.forEach(function(user) {
  html += '<div class="user">' +
          '  <h3>' + user.name + '</h3>' +
          '  <p>' + user.age + '세</p>' +
          '</div>';
});

$('#app').html(html);

// React JSX 방식
const users = [
  { id: 1, name: "홍길동", age: 25 },
  { id: 2, name: "김철수", age: 30 }
];

const element = (
  <div>
    {users.map(user => (
      <div key={user.id} className="user">
        <h3>{user.name}</h3>
        <p>{user.age}세</p>
      </div>
    ))}
  </div>
);
```

### 조건부 렌더링

```javascript
// jQuery 방식
var isLoggedIn = true;
var html = '';

if (isLoggedIn) {
  html = '<button>로그아웃</button>';
} else {
  html = '<button>로그인</button>';
}

$('#app').html(html);

// React JSX 방식 1: 삼항 연산자
const isLoggedIn = true;

const element = (
  <div>
    {isLoggedIn ? (
      <button>로그아웃</button>
    ) : (
      <button>로그인</button>
    )}
  </div>
);

// React JSX 방식 2: && 연산자
const element = (
  <div>
    {isLoggedIn && <button>로그아웃</button>}
    {!isLoggedIn && <button>로그인</button>}
  </div>
);
```

### 이벤트 핸들러

```javascript
// jQuery 방식
var html = '<button class="my-button">클릭하세요</button>';
$('#app').html(html);

$('.my-button').click(function() {
  alert('클릭됨!');
});

// React JSX 방식
const handleClick = () => {
  alert('클릭됨!');
};

const element = (
  <button onClick={handleClick}>
    클릭하세요
  </button>
);
```

## JSX 고급 활용

### 1. 조건부 렌더링 패턴

```javascript
// 1. if-else 문 (JSX 밖에서)
let content;
if (isLoading) {
  content = <div>로딩 중...</div>;
} else if (hasError) {
  content = <div>에러 발생!</div>;
} else {
  content = <div>데이터 표시</div>;
}

const element = <div>{content}</div>;

// 2. 삼항 연산자
const element = (
  <div>
    {isLoading ? (
      <div>로딩 중...</div>
    ) : (
      <div>데이터 표시</div>
    )}
  </div>
);

// 3. && 연산자 (조건이 참일 때만 렌더링)
const element = (
  <div>
    {isLoggedIn && <UserProfile />}
    {!isLoggedIn && <LoginButton />}
    {errorMessage && <div className="error">{errorMessage}</div>}
  </div>
);

// 4. 즉시 실행 함수
const element = (
  <div>
    {(() => {
      if (status === 'loading') return <Spinner />;
      if (status === 'error') return <Error />;
      return <Content />;
    })()}
  </div>
);
```

### 2. 리스트 렌더링

```javascript
const products = [
  { id: 1, name: "노트북", price: 1200000 },
  { id: 2, name: "마우스", price: 35000 },
  { id: 3, name: "키보드", price: 80000 }
];

// map을 사용한 리스트 렌더링
const element = (
  <div className="product-list">
    {products.map(product => (
      <div key={product.id} className="product">
        <h3>{product.name}</h3>
        <p>{product.price.toLocaleString()}원</p>
      </div>
    ))}
  </div>
);

// filter와 map 조합
const element = (
  <div>
    <h2>10만원 이하 상품</h2>
    {products
      .filter(product => product.price <= 100000)
      .map(product => (
        <div key={product.id}>
          {product.name}
        </div>
      ))}
  </div>
);
```

**중요:** 리스트 렌더링 시 각 항목에 고유한 `key` prop을 지정해야 합니다!

### 3. 스타일 지정

```javascript
// 1. 문자열로 className 지정
const element = <div className="container">내용</div>;

// 2. 동적 className
const isActive = true;
const element = (
  <div className={isActive ? 'active' : 'inactive'}>
    내용
  </div>
);

// 3. 여러 클래스
const element = (
  <div className={`card ${isActive ? 'active' : ''} ${isPrimary ? 'primary' : ''}`}>
    내용
  </div>
);

// 4. 인라인 스타일 (객체 사용)
const style = {
  color: 'red',
  backgroundColor: 'yellow',
  fontSize: '20px',  // 숫자면 px 자동 추가
  padding: '10px'
};

const element = <div style={style}>내용</div>;

// 5. 직접 객체 전달
const element = (
  <div style={{ color: 'red', fontSize: '20px' }}>
    내용
  </div>
);
```

### 4. 변수와 함수 활용

```javascript
// 변수에 JSX 저장
const greeting = <h1>안녕하세요!</h1>;
const message = <p>환영합니다.</p>;

const element = (
  <div>
    {greeting}
    {message}
  </div>
);

// 함수에서 JSX 반환
const getGreeting = (name) => {
  if (!name) {
    return <h1>안녕하세요!</h1>;
  }
  return <h1>안녕하세요, {name}님!</h1>;
};

const element = (
  <div>
    {getGreeting('홍길동')}
  </div>
);
```

## 실전 예제

### 사용자 카드 컴포넌트

```javascript
// jQuery 방식
function createUserCard(user) {
  var statusClass = user.isActive ? 'active' : 'inactive';
  var statusText = user.isActive ? '활동 중' : '오프라인';

  var html = '<div class="user-card ' + statusClass + '">' +
             '  <img src="' + user.avatar + '" alt="' + user.name + '" />' +
             '  <h3>' + user.name + '</h3>' +
             '  <p>' + user.email + '</p>' +
             '  <span class="status">' + statusText + '</span>' +
             '  <button class="btn-message" data-id="' + user.id + '">메시지 보내기</button>' +
             '</div>';

  return html;
}

var users = [/* 사용자 배열 */];
var html = users.map(createUserCard).join('');
$('#app').html(html);

// React JSX 방식
const UserCard = ({ user }) => {
  const statusClass = user.isActive ? 'active' : 'inactive';
  const statusText = user.isActive ? '활동 중' : '오프라인';

  return (
    <div className={`user-card ${statusClass}`}>
      <img src={user.avatar} alt={user.name} />
      <h3>{user.name}</h3>
      <p>{user.email}</p>
      <span className="status">{statusText}</span>
      <button className="btn-message" data-id={user.id}>
        메시지 보내기
      </button>
    </div>
  );
};

const App = () => {
  const users = [/* 사용자 배열 */];

  return (
    <div id="app">
      {users.map(user => (
        <UserCard key={user.id} user={user} />
      ))}
    </div>
  );
};
```

## 주의사항

### 1. JSX에서 금지된 것들

```javascript
// ❌ for 대신 htmlFor 사용
<label for="name">이름</label>  // 잘못됨
<label htmlFor="name">이름</label>  // 올바름

// ❌ class 대신 className 사용
<div class="container"></div>  // 잘못됨
<div className="container"></div>  // 올바름

// ❌ 문(statement)은 사용 불가, 표현식만 가능
<div>
  {if (condition) { return "text"; }}  // 잘못됨
  {condition ? "text" : ""}  // 올바름
</div>
```

### 2. Boolean 속성

```javascript
// HTML에서는
<input type="checkbox" checked>
<button disabled>버튼</button>

// JSX에서는
<input type="checkbox" checked={true} />
<input type="checkbox" checked />  // true와 같음

<button disabled={false}>버튼</button>  // disabled 아님
<button disabled={true}>버튼</button>
<button disabled>버튼</button>  // true와 같음
```

### 3. 특수 문자 이스케이프

```javascript
// HTML 엔티티는 작동 안 함
<div>&copy; 2024</div>  // © 2024로 표시 안 됨

// 유니코드 사용
<div>{'\u00A9'} 2024</div>  // © 2024

// 또는 직접 입력
<div>© 2024</div>
```

## 자주 묻는 질문

### Q: JSX는 필수인가요?

A: 아니요, 하지만 **강력히 권장**됩니다.
```javascript
// JSX 없이 React 사용
const element = React.createElement('h1', null, '안녕하세요');

// JSX 사용 (훨씬 읽기 쉬움)
const element = <h1>안녕하세요</h1>;
```

### Q: JSX에서 if문을 사용할 수 없나요?

A: JSX 안에서는 표현식만 가능하고 문(statement)은 불가능합니다.
```javascript
// ❌ 안 됨
<div>
  {if (condition) { return <div>Yes</div>; }}
</div>

// ✅ 삼항 연산자 사용
<div>
  {condition ? <div>Yes</div> : null}
</div>

// ✅ JSX 밖에서 if 사용
let content;
if (condition) {
  content = <div>Yes</div>;
}
return <div>{content}</div>;
```

### Q: key prop은 왜 필요한가요?

A: React가 어떤 항목이 변경/추가/삭제되었는지 식별하기 위해 필요합니다.
```javascript
// ❌ key 없음 (경고 발생)
{items.map(item => <div>{item.name}</div>)}

// ✅ 고유한 key 제공
{items.map(item => <div key={item.id}>{item.name}</div>)}

// ❌ 인덱스를 key로 사용 (항목 순서가 바뀔 수 있으면 피해야 함)
{items.map((item, index) => <div key={index}>{item.name}</div>)}
```

### Q: className을 동적으로 지정하려면?

A: 템플릿 리터럴이나 라이브러리를 사용합니다.
```javascript
// 템플릿 리터럴
<div className={`card ${isActive ? 'active' : ''}`}>

// classnames 라이브러리 (npm install classnames)
import classNames from 'classnames';

<div className={classNames('card', { active: isActive, disabled: isDisabled })}>
```

## 연습 문제

다음 jQuery 코드를 JSX로 변환해보세요:

```javascript
var products = [
  { id: 1, name: "노트북", price: 1200000, inStock: true },
  { id: 2, name: "마우스", price: 35000, inStock: false },
  { id: 3, name: "키보드", price: 80000, inStock: true }
];

var html = '<div class="products">';
products.forEach(function(product) {
  var stockClass = product.inStock ? 'in-stock' : 'out-of-stock';
  var stockText = product.inStock ? '재고 있음' : '품절';

  html += '<div class="product ' + stockClass + '">' +
          '  <h3>' + product.name + '</h3>' +
          '  <p class="price">' + product.price.toLocaleString() + '원</p>' +
          '  <span class="stock">' + stockText + '</span>';

  if (product.inStock) {
    html += '  <button class="btn-buy">구매하기</button>';
  }

  html += '</div>';
});
html += '</div>';

$('#app').html(html);
```

### 정답

```javascript
const products = [
  { id: 1, name: "노트북", price: 1200000, inStock: true },
  { id: 2, name: "마우스", price: 35000, inStock: false },
  { id: 3, name: "키보드", price: 80000, inStock: true }
];

const App = () => {
  return (
    <div className="products">
      {products.map(product => {
        const stockClass = product.inStock ? 'in-stock' : 'out-of-stock';
        const stockText = product.inStock ? '재고 있음' : '품절';

        return (
          <div key={product.id} className={`product ${stockClass}`}>
            <h3>{product.name}</h3>
            <p className="price">{product.price.toLocaleString()}원</p>
            <span className="stock">{stockText}</span>
            {product.inStock && (
              <button className="btn-buy">구매하기</button>
            )}
          </div>
        );
      })}
    </div>
  );
};
```

## 요약

| 구분 | jQuery (문자열) | JSX |
|------|----------------|-----|
| 문법 | HTML 문자열 | JavaScript + HTML 구문 |
| 타입 | 문자열 | JavaScript 표현식 |
| 문법 검사 | ❌ 없음 | ✅ 있음 |
| 자동 완성 | ❌ 제한적 | ✅ 완벽 지원 |
| 변수 삽입 | `+` 또는 `` `${}` `` | `{}` |
| 클래스 속성 | `class` | `className` |
| 이벤트 | 문자열 후 별도 바인딩 | 직접 지정 |

**핵심 원칙:**
1. JSX는 HTML이 아니라 JavaScript 표현식
2. `{}` 안에는 JavaScript 표현식만 가능
3. 하나의 부모 요소로 감싸기
4. `className`, `htmlFor` 등 camelCase 사용
5. 리스트는 `key` prop 필수

**다음 단계:**
JSX를 이해했다면 컴포넌트를 배울 준비가 되었습니다!
