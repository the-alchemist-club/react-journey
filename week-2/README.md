# Week 2: React 기본 개념

React의 핵심 개념인 JSX, 컴포넌트, Props, State, 이벤트 핸들링을 학습합니다.

## 학습 목표

jQuery에서 HTML 문자열로 UI를 만들던 방식에서 벗어나, React의 선언적 방식으로 UI를 구축하는 방법을 배웁니다. 이번 주차를 마치면 간단한 인터랙티브 React 앱을 만들 수 있습니다.

## 왜 이런 개념들이 필요할까요?

### jQuery 방식의 한계

```javascript
// jQuery: 명령형 - "어떻게" 할지 지시
var count = 0;

$('#increment').click(function() {
  count++;
  $('#counter').text(count);  // DOM 직접 조작

  if (count > 10) {
    $('#message').show();  // 조건에 따라 직접 조작
  }
});
```

**문제점:**
- 데이터와 UI가 따로 관리됨
- 데이터 변경 시 수동으로 DOM 업데이트 필요
- 복잡해질수록 관리하기 어려움

### React 방식의 장점

```javascript
// React: 선언형 - "무엇을" 보여줄지 선언
const Counter = () => {
  const [count, setCount] = useState(0);

  return (
    <div>
      <p>카운트: {count}</p>
      <button onClick={() => setCount(count + 1)}>
        증가
      </button>
      {count > 10 && <p>10을 넘었습니다!</p>}
    </div>
  );
};
```

**장점:**
- 데이터(State)와 UI가 자동 동기화
- 상태가 바뀌면 UI가 자동 업데이트
- 코드가 더 직관적이고 예측 가능

## 학습 내용

### 1. [JSX (JavaScript XML)](./01-jsx.md)

JavaScript 안에서 HTML처럼 작성하는 문법입니다.

**배우는 것:**
- JSX가 무엇이고 왜 필요한지
- 기본 문법과 규칙
- 조건부 렌더링
- 리스트 렌더링
- 스타일 지정 방법

**핵심 포인트:**
```javascript
// JSX
const element = (
  <div className="user">
    <h1>{name}</h1>
    <p>나이: {age}세</p>
  </div>
);

// jQuery (비교)
var html = '<div class="user">' +
           '  <h1>' + name + '</h1>' +
           '  <p>나이: ' + age + '세</p>' +
           '</div>';
```

**예상 소요 시간:** 1-2시간

### 2. [컴포넌트 (Components)](./02-components.md)

UI를 독립적이고 재사용 가능한 조각으로 나누는 방법입니다.

**배우는 것:**
- 함수 컴포넌트 작성
- 컴포넌트 합성
- 컴포넌트 분리 기준
- 파일 구조
- children prop

**핵심 포인트:**
```javascript
// 작은 컴포넌트
const Button = ({ text, onClick }) => (
  <button onClick={onClick}>{text}</button>
);

// 큰 컴포넌트로 조합
const App = () => (
  <div>
    <Button text="저장" onClick={handleSave} />
    <Button text="취소" onClick={handleCancel} />
  </div>
);
```

**예상 소요 시간:** 1-2시간

### 3. [Props와 State](./03-props-state.md)

React에서 데이터를 다루는 두 가지 핵심 개념입니다.

**배우는 것:**
- Props: 부모 → 자식 데이터 전달
- State: 컴포넌트 내부 상태 관리
- useState Hook 사용법
- State 업데이트와 불변성
- Props와 State 함께 사용하기

**핵심 포인트:**
```javascript
// Props (읽기 전용)
const UserCard = ({ name, email }) => (
  <div>
    <h3>{name}</h3>
    <p>{email}</p>
  </div>
);

// State (변경 가능)
const Counter = () => {
  const [count, setCount] = useState(0);

  return (
    <button onClick={() => setCount(count + 1)}>
      {count}
    </button>
  );
};
```

**예상 소요 시간:** 2-3시간

### 4. [이벤트 핸들링 (Event Handling)](./04-event-handling.md)

사용자 상호작용을 처리하는 방법입니다.

**배우는 것:**
- 이벤트 핸들러 작성
- 이벤트 객체 활용
- 폼 처리
- 다양한 이벤트 타입
- 이벤트 최적화

**핵심 포인트:**
```javascript
// jQuery
$('#button').click(function() {
  alert('클릭!');
});

// React
<button onClick={() => alert('클릭!')}>
  클릭
</button>
```

**예상 소요 시간:** 1-2시간

## 학습 순서

1. **JSX 먼저**: React 코드를 읽고 쓰는 기본
2. **컴포넌트**: UI를 조각내는 방법
3. **Props & State**: 데이터 관리의 핵심
4. **이벤트**: 인터랙션 추가

각 개념이 이전 개념을 기반으로 하므로 순서대로 학습하세요!

## 실습: 첫 React 앱 만들기

모든 개념을 배운 후 간단한 할 일 목록 앱을 만들어봅시다.

### 요구사항

1. 할 일 추가
2. 할 일 완료 체크
3. 할 일 삭제
4. 완료된 항목 개수 표시

### 구현

```javascript
import { useState } from 'react';

// TodoItem 컴포넌트
const TodoItem = ({ todo, onToggle, onDelete }) => {
  return (
    <div className="todo-item">
      <input
        type="checkbox"
        checked={todo.done}
        onChange={() => onToggle(todo.id)}
      />
      <span style={{
        textDecoration: todo.done ? 'line-through' : 'none'
      }}>
        {todo.text}
      </span>
      <button onClick={() => onDelete(todo.id)}>삭제</button>
    </div>
  );
};

// TodoApp 컴포넌트
const TodoApp = () => {
  const [todos, setTodos] = useState([]);
  const [inputText, setInputText] = useState('');

  const handleAdd = () => {
    if (!inputText.trim()) return;

    const newTodo = {
      id: Date.now(),
      text: inputText,
      done: false
    };

    setTodos([...todos, newTodo]);
    setInputText('');
  };

  const handleToggle = (id) => {
    setTodos(
      todos.map(todo =>
        todo.id === id ? { ...todo, done: !todo.done } : todo
      )
    );
  };

  const handleDelete = (id) => {
    setTodos(todos.filter(todo => todo.id !== id));
  };

  const handleKeyPress = (e) => {
    if (e.key === 'Enter') {
      handleAdd();
    }
  };

  const completedCount = todos.filter(todo => todo.done).length;

  return (
    <div className="todo-app">
      <h1>할 일 목록</h1>

      <div className="input-section">
        <input
          type="text"
          value={inputText}
          onChange={(e) => setInputText(e.target.value)}
          onKeyPress={handleKeyPress}
          placeholder="할 일을 입력하세요"
        />
        <button onClick={handleAdd}>추가</button>
      </div>

      <p>완료: {completedCount} / {todos.length}</p>

      <div className="todo-list">
        {todos.length === 0 ? (
          <p>할 일이 없습니다!</p>
        ) : (
          todos.map(todo => (
            <TodoItem
              key={todo.id}
              todo={todo}
              onToggle={handleToggle}
              onDelete={handleDelete}
            />
          ))
        )}
      </div>
    </div>
  );
};

export default TodoApp;
```

## jQuery vs React 비교

### 할 일 목록 구현 비교

| 작업 | jQuery | React |
|------|--------|-------|
| UI 생성 | HTML 문자열 | JSX |
| 데이터 관리 | 전역 변수/DOM | State |
| UI 업데이트 | 수동 (DOM 조작) | 자동 (리렌더링) |
| 이벤트 처리 | 별도 등록 | JSX에 직접 |
| 컴포넌트화 | 함수 (문자열 반환) | 컴포넌트 (JSX 반환) |

### 코드 길이 비교

같은 기능을 구현할 때:
- **jQuery**: 더 많은 코드, 수동 DOM 조작
- **React**: 더 적은 코드, 선언적 UI

### 유지보수성

- **jQuery**: 복잡해질수록 관리 어려움
- **React**: 컴포넌트로 분리하여 관리 쉬움

## 학습 체크리스트

각 주제를 완료했는지 확인하세요:

### JSX
- [ ] JSX 기본 문법 이해
- [ ] `{}` 안에 JavaScript 표현식 사용
- [ ] 조건부 렌더링 (삼항 연산자, && 연산자)
- [ ] 리스트 렌더링 (map, key prop)
- [ ] className, htmlFor 등 차이점 이해
- [ ] 인라인 스타일 적용

### 컴포넌트
- [ ] 함수 컴포넌트 작성
- [ ] Props 전달 및 받기
- [ ] 컴포넌트 합성 이해
- [ ] children prop 활용
- [ ] 컴포넌트 파일 분리
- [ ] 컴포넌트 명명 규칙 (PascalCase)

### Props & State
- [ ] Props vs State 차이 이해
- [ ] useState Hook 사용
- [ ] State 업데이트 (불변성 유지)
- [ ] 배열 State 관리
- [ ] 객체 State 관리
- [ ] 부모-자식 간 데이터 흐름 이해

### 이벤트 핸들링
- [ ] onClick, onChange 등 이벤트 핸들러 작성
- [ ] 이벤트 객체 활용
- [ ] e.preventDefault() 사용
- [ ] 폼 제출 처리
- [ ] 키보드 이벤트 처리
- [ ] 함수 참조 vs 함수 실행 차이 이해

### 실습
- [ ] 할 일 목록 앱 완성
- [ ] 컴포넌트로 분리
- [ ] State로 데이터 관리
- [ ] 이벤트 핸들러 구현

## 문제 해결 가이드

### JSX에서 중괄호가 표시됨

```javascript
// ❌ 잘못됨
<div>{{name}}</div>

// ✅ 올바름
<div>{name}</div>
```

### State가 업데이트 안 됨

```javascript
// ❌ 직접 변경 (안 됨)
items.push(newItem);
setItems(items);

// ✅ 새 배열 생성
setItems([...items, newItem]);
```

### 이벤트 핸들러가 즉시 실행됨

```javascript
// ❌ 즉시 실행
<button onClick={handleClick()}>클릭</button>

// ✅ 함수 참조 전달
<button onClick={handleClick}>클릭</button>

// ✅ 인자 전달 시
<button onClick={() => handleClick(id)}>클릭</button>
```

### key prop 경고

```javascript
// ❌ key 없음
{items.map(item => <div>{item}</div>)}

// ✅ key 추가
{items.map(item => <div key={item.id}>{item}</div>)}
```

## 자주 묻는 질문

### Q: JSX는 필수인가요?

A: 필수는 아니지만 **강력히 권장**됩니다.
- JSX 없이도 `React.createElement()`로 작성 가능
- 하지만 JSX가 훨씬 읽기 쉽고 직관적

### Q: 클래스 컴포넌트는 배워야 하나요?

A: 아니요, **함수 컴포넌트 + Hooks**만 배우세요.
- 클래스 컴포넌트는 레거시
- 최신 React는 함수 컴포넌트 권장

### Q: State를 Props로 전달할 수 있나요?

A: 네! 부모의 State를 자식의 Props로 전달합니다.

```javascript
const Parent = () => {
  const [name, setName] = useState('홍길동');
  return <Child name={name} />;  // State → Props
};

const Child = ({ name }) => {
  return <p>{name}</p>;  // Props로 받음
};
```

### Q: 언제 State를 쓰고 언제 Props를 쓰나요?

A: 간단한 기준:
- **변경되는 데이터** → State
- **부모에서 받은 데이터** → Props
- **자식이 수정하면 안 되는 데이터** → Props

### Q: 너무 많은 개념을 한 번에 배우는 것 같아요

A: 괜찮습니다! 천천히 진행하세요.
- 한 번에 완벽히 이해하려 하지 마세요
- 실습하면서 자연스럽게 익숙해집니다
- 중요한 것은 **코드를 직접 작성**하는 것

## 다음 단계

Week 2를 완료했다면:
- ✅ JSX로 UI 작성 가능
- ✅ 컴포넌트로 UI 분리 가능
- ✅ Props와 State로 데이터 관리 가능
- ✅ 이벤트 핸들러로 인터랙션 추가 가능
- 📖 다음 학습: Week 3 - 컴포넌트 설계

## 추가 자료

### 공식 문서
- [React 공식 문서 - 주요 개념](https://react.dev/learn)
- [React 한국어 문서](https://ko.react.dev/)

### 튜토리얼
- [React 공식 튜토리얼 - 틱택토](https://react.dev/learn/tutorial-tic-tac-toe)
- [생활코딩 - React](https://opentutorials.org/module/4058)

### 연습 프로젝트 아이디어
1. **카운터 앱**: 증가/감소/리셋 버튼
2. **할 일 목록**: 추가/삭제/완료
3. **간단한 계산기**: 기본 연산
4. **사용자 카드 목록**: 검색/필터
5. **탭 UI**: 여러 탭 전환

## 팁

### 1. 실습이 가장 중요합니다

이론만 읽지 말고 직접 코드를 작성하세요:
```javascript
// 읽기만 하지 말고
const Button = ({ text }) => <button>{text}</button>;

// 직접 타이핑하면서
// 변형해보고, 에러도 내보세요!
```

### 2. 작은 것부터 시작

복잡한 앱을 만들려 하지 말고:
1. 버튼 하나 만들기
2. State 추가하기
3. 입력 필드 추가하기
4. 점진적으로 기능 추가

### 3. React DevTools 활용

- Chrome/Firefox 확장 프로그램 설치
- 컴포넌트 구조 확인
- Props와 State 실시간 확인

### 4. 에러 메시지를 읽으세요

React는 친절한 에러 메시지를 제공합니다:
```
Warning: Each child in a list should have a unique "key" prop.
→ 리스트의 각 항목에 key prop 추가하세요!
```

### 5. console.log() 적극 활용

```javascript
const MyComponent = ({ data }) => {
  console.log('받은 데이터:', data);  // 확인용

  const [state, setState] = useState(0);
  console.log('현재 state:', state);  // 확인용

  return <div>...</div>;
};
```

## 축하합니다!

React의 핵심 개념을 모두 학습했습니다! 🎉

이제 여러분은:
- JSX로 UI를 작성할 수 있습니다
- 컴포넌트로 UI를 구조화할 수 있습니다
- Props와 State로 데이터를 관리할 수 있습니다
- 이벤트로 인터랙티브한 앱을 만들 수 있습니다

---

**준비되셨나요?** Week 3에서는 더 복잡한 컴포넌트 설계를 배웁니다!

메인 [README.md](../README.md)로 돌아가서 다음 단계를 확인하세요.
