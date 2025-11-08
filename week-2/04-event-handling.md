# 이벤트 핸들링 (Event Handling)

## jQuery 시대의 이벤트 처리

jQuery에서는 이벤트 리스너를 별도로 등록했습니다.

```javascript
// HTML
<button id="save-button">저장</button>
<input id="name-input" type="text" />

// jQuery로 이벤트 등록
$(document).ready(function() {
  // 클릭 이벤트
  $('#save-button').click(function() {
    alert('저장되었습니다!');
  });

  // 입력 이벤트
  $('#name-input').on('input', function() {
    var value = $(this).val();
    console.log('입력값:', value);
  });

  // 이벤트 위임
  $(document).on('click', '.delete-button', function() {
    $(this).closest('.item').remove();
  });
});
```

**특징:**
- HTML과 JavaScript가 분리됨
- 선택자로 요소를 찾아서 이벤트 등록
- 나중에 추가된 요소는 이벤트 위임 필요
- `this`가 이벤트 대상 요소를 가리킴

## React의 이벤트 처리

React에서는 JSX에서 직접 이벤트 핸들러를 지정합니다.

```javascript
const App = () => {
  const handleClick = () => {
    alert('저장되었습니다!');
  };

  const handleInput = (e) => {
    const value = e.target.value;
    console.log('입력값:', value);
  };

  return (
    <div>
      <button onClick={handleClick}>저장</button>
      <input type="text" onInput={handleInput} />
    </div>
  );
};
```

**특징:**
- HTML과 이벤트 핸들러가 함께 있음
- 선택자 없이 직접 지정
- 이벤트 위임 불필요 (React가 자동 처리)
- 화살표 함수를 사용하면 `this` 바인딩 문제 없음

## 기본 이벤트 핸들러

### 클릭 이벤트

```javascript
// jQuery
$('#button').click(function() {
  alert('클릭!');
});

// React
const Button = () => {
  const handleClick = () => {
    alert('클릭!');
  };

  return <button onClick={handleClick}>클릭</button>;
};

// 또는 인라인
const Button = () => {
  return (
    <button onClick={() => alert('클릭!')}>
      클릭
    </button>
  );
};
```

### 입력 이벤트

```javascript
// jQuery
$('#input').on('input', function() {
  var value = $(this).val();
  $('#output').text(value);
});

// React
const Input = () => {
  const [text, setText] = useState('');

  const handleChange = (e) => {
    setText(e.target.value);
  };

  return (
    <div>
      <input type="text" value={text} onChange={handleChange} />
      <p>입력값: {text}</p>
    </div>
  );
};
```

### 폼 제출 이벤트

```javascript
// jQuery
$('#form').submit(function(e) {
  e.preventDefault();
  var name = $('#name').val();
  var email = $('#email').val();
  console.log(name, email);
});

// React
const Form = () => {
  const [name, setName] = useState('');
  const [email, setEmail] = useState('');

  const handleSubmit = (e) => {
    e.preventDefault();
    console.log(name, email);
  };

  return (
    <form onSubmit={handleSubmit}>
      <input
        type="text"
        value={name}
        onChange={(e) => setName(e.target.value)}
      />
      <input
        type="email"
        value={email}
        onChange={(e) => setEmail(e.target.value)}
      />
      <button type="submit">제출</button>
    </form>
  );
};
```

## 이벤트 핸들러 작성 방법

### 1. 함수 정의 후 전달 (추천)

```javascript
const Button = () => {
  const handleClick = () => {
    console.log('클릭됨!');
  };

  return <button onClick={handleClick}>클릭</button>;
};
```

### 2. 인라인 화살표 함수

```javascript
const Button = () => {
  return (
    <button onClick={() => console.log('클릭됨!')}>
      클릭
    </button>
  );
};
```

### 3. 인자가 필요한 경우

```javascript
const List = () => {
  const items = ['사과', '바나나', '오렌지'];

  const handleClick = (item) => {
    console.log(`${item} 클릭됨!`);
  };

  return (
    <ul>
      {items.map((item, index) => (
        <li key={index} onClick={() => handleClick(item)}>
          {item}
        </li>
      ))}
    </ul>
  );
};
```

## 이벤트 객체 (Event Object)

React의 이벤트 객체는 브라우저의 네이티브 이벤트를 감싼 합성 이벤트(SyntheticEvent)입니다.

```javascript
const Input = () => {
  const handleEvent = (e) => {
    console.log('이벤트 타입:', e.type);
    console.log('대상 요소:', e.target);
    console.log('현재 요소:', e.currentTarget);
    console.log('입력값:', e.target.value);

    // 기본 동작 방지
    e.preventDefault();

    // 이벤트 전파 중단
    e.stopPropagation();
  };

  return (
    <input
      type="text"
      onChange={handleEvent}
      onClick={handleEvent}
    />
  );
};
```

### 자주 사용하는 이벤트 속성

```javascript
const handleChange = (e) => {
  // 이벤트가 발생한 요소
  const target = e.target;

  // 입력값
  const value = target.value;

  // 요소 이름
  const name = target.name;

  // 체크박스/라디오 체크 여부
  const checked = target.checked;

  // 키보드 이벤트
  const key = e.key;
  const keyCode = e.keyCode;

  // 마우스 이벤트
  const clientX = e.clientX;
  const clientY = e.clientY;
};
```

## 주요 이벤트 타입

### 마우스 이벤트

```javascript
const MouseEvents = () => {
  return (
    <div
      onClick={() => console.log('클릭')}
      onDoubleClick={() => console.log('더블 클릭')}
      onMouseEnter={() => console.log('마우스 진입')}
      onMouseLeave={() => console.log('마우스 이탈')}
      onMouseMove={() => console.log('마우스 이동')}
      onMouseDown={() => console.log('마우스 버튼 눌림')}
      onMouseUp={() => console.log('마우스 버튼 뗌')}
    >
      마우스를 올려보세요
    </div>
  );
};
```

### 키보드 이벤트

```javascript
const KeyboardEvents = () => {
  const handleKeyPress = (e) => {
    console.log('눌린 키:', e.key);

    if (e.key === 'Enter') {
      console.log('엔터 키!');
    }

    if (e.ctrlKey && e.key === 's') {
      e.preventDefault();
      console.log('Ctrl+S 저장!');
    }
  };

  return (
    <input
      type="text"
      onKeyDown={handleKeyPress}
      onKeyUp={(e) => console.log('키 뗌:', e.key)}
      onKeyPress={(e) => console.log('키 입력:', e.key)}
    />
  );
};
```

### 폼 이벤트

```javascript
const FormEvents = () => {
  const [value, setValue] = useState('');

  return (
    <form
      onSubmit={(e) => {
        e.preventDefault();
        console.log('폼 제출');
      }}
    >
      <input
        type="text"
        value={value}
        onChange={(e) => setValue(e.target.value)}
        onFocus={() => console.log('포커스 획득')}
        onBlur={() => console.log('포커스 잃음')}
      />

      <select
        onChange={(e) => console.log('선택:', e.target.value)}
      >
        <option value="1">옵션 1</option>
        <option value="2">옵션 2</option>
      </select>

      <button type="submit">제출</button>
    </form>
  );
};
```

## jQuery vs React 실전 비교

### 할 일 목록

```javascript
// jQuery 방식
var todos = [];

$('#add-button').click(function() {
  var text = $('#todo-input').val();
  if (!text) return;

  todos.push({ id: Date.now(), text: text, done: false });
  renderTodos();
  $('#todo-input').val('');
});

$(document).on('click', '.todo-toggle', function() {
  var id = $(this).data('id');
  var todo = todos.find(t => t.id === id);
  todo.done = !todo.done;
  renderTodos();
});

$(document).on('click', '.todo-delete', function() {
  var id = $(this).data('id');
  todos = todos.filter(t => t.id !== id);
  renderTodos();
});

function renderTodos() {
  var html = todos.map(function(todo) {
    return '<div class="todo">' +
           '  <input type="checkbox" class="todo-toggle" ' +
           '    data-id="' + todo.id + '" ' +
           (todo.done ? 'checked' : '') + '>' +
           '  <span>' + todo.text + '</span>' +
           '  <button class="todo-delete" data-id="' + todo.id + '">삭제</button>' +
           '</div>';
  }).join('');
  $('#todos').html(html);
}

// React 방식
const TodoApp = () => {
  const [todos, setTodos] = useState([]);
  const [inputText, setInputText] = useState('');

  const handleAdd = () => {
    if (!inputText) return;

    setTodos([
      ...todos,
      { id: Date.now(), text: inputText, done: false }
    ]);
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

  return (
    <div>
      <input
        type="text"
        value={inputText}
        onChange={(e) => setInputText(e.target.value)}
        onKeyPress={handleKeyPress}
      />
      <button onClick={handleAdd}>추가</button>

      <div>
        {todos.map(todo => (
          <div key={todo.id} className="todo">
            <input
              type="checkbox"
              checked={todo.done}
              onChange={() => handleToggle(todo.id)}
            />
            <span>{todo.text}</span>
            <button onClick={() => handleDelete(todo.id)}>
              삭제
            </button>
          </div>
        ))}
      </div>
    </div>
  );
};
```

### 탭 UI

```javascript
// jQuery 방식
$('.tab-button').click(function() {
  var index = $(this).index();

  $('.tab-button').removeClass('active');
  $(this).addClass('active');

  $('.tab-content').removeClass('active');
  $('.tab-content').eq(index).addClass('active');
});

// React 방식
const Tabs = () => {
  const [activeTab, setActiveTab] = useState(0);

  const tabs = [
    { title: '탭 1', content: '첫 번째 탭 내용' },
    { title: '탭 2', content: '두 번째 탭 내용' },
    { title: '탭 3', content: '세 번째 탭 내용' }
  ];

  return (
    <div>
      <div className="tab-buttons">
        {tabs.map((tab, index) => (
          <button
            key={index}
            className={activeTab === index ? 'active' : ''}
            onClick={() => setActiveTab(index)}
          >
            {tab.title}
          </button>
        ))}
      </div>

      <div className="tab-content">
        {tabs[activeTab].content}
      </div>
    </div>
  );
};
```

## 이벤트 핸들러 최적화

### 1. useCallback으로 함수 메모이제이션

```javascript
import { useState, useCallback } from 'react';

const List = () => {
  const [items, setItems] = useState([1, 2, 3]);

  // 매번 새로운 함수가 생성되는 것을 방지
  const handleClick = useCallback((id) => {
    console.log('클릭:', id);
  }, []);

  return (
    <ul>
      {items.map(item => (
        <li key={item} onClick={() => handleClick(item)}>
          {item}
        </li>
      ))}
    </ul>
  );
};
```

### 2. 이벤트 위임 패턴

```javascript
const List = () => {
  const items = ['사과', '바나나', '오렌지'];

  const handleClick = (e) => {
    // 클릭된 요소가 li인 경우만
    if (e.target.tagName === 'LI') {
      console.log('클릭:', e.target.textContent);
    }
  };

  return (
    <ul onClick={handleClick}>
      {items.map((item, index) => (
        <li key={index}>{item}</li>
      ))}
    </ul>
  );
};
```

## 실전 예제

### 검색 기능이 있는 목록

```javascript
const SearchableList = () => {
  const [items] = useState([
    '사과', '바나나', '오렌지', '포도', '딸기', '수박'
  ]);
  const [searchTerm, setSearchTerm] = useState('');

  const filteredItems = items.filter(item =>
    item.toLowerCase().includes(searchTerm.toLowerCase())
  );

  const handleSearchChange = (e) => {
    setSearchTerm(e.target.value);
  };

  const handleClear = () => {
    setSearchTerm('');
  };

  return (
    <div>
      <div>
        <input
          type="text"
          value={searchTerm}
          onChange={handleSearchChange}
          placeholder="과일 검색..."
        />
        <button onClick={handleClear}>지우기</button>
      </div>

      <ul>
        {filteredItems.length > 0 ? (
          filteredItems.map((item, index) => (
            <li key={index}>{item}</li>
          ))
        ) : (
          <li>검색 결과가 없습니다.</li>
        )}
      </ul>
    </div>
  );
};
```

### 모달 (Modal)

```javascript
const Modal = ({ isOpen, onClose, children }) => {
  if (!isOpen) return null;

  const handleBackdropClick = (e) => {
    // 배경 클릭 시에만 닫기
    if (e.target === e.currentTarget) {
      onClose();
    }
  };

  const handleEscapeKey = (e) => {
    if (e.key === 'Escape') {
      onClose();
    }
  };

  return (
    <div
      className="modal-backdrop"
      onClick={handleBackdropClick}
      onKeyDown={handleEscapeKey}
    >
      <div className="modal-content">
        <button className="close-button" onClick={onClose}>
          ×
        </button>
        {children}
      </div>
    </div>
  );
};

// 사용
const App = () => {
  const [isModalOpen, setIsModalOpen] = useState(false);

  return (
    <div>
      <button onClick={() => setIsModalOpen(true)}>
        모달 열기
      </button>

      <Modal
        isOpen={isModalOpen}
        onClose={() => setIsModalOpen(false)}
      >
        <h2>모달 제목</h2>
        <p>모달 내용입니다.</p>
      </Modal>
    </div>
  );
};
```

## 자주 묻는 질문

### Q: onClick={handleClick} vs onClick={handleClick()}의 차이는?

A: 매우 중요한 차이가 있습니다!

```javascript
// ✅ 올바름 - 함수 참조 전달
<button onClick={handleClick}>클릭</button>

// ❌ 잘못됨 - 함수를 즉시 실행
<button onClick={handleClick()}>클릭</button>
// 렌더링 시 즉시 실행되고, 클릭해도 작동 안 함

// ✅ 인자를 전달하려면 화살표 함수 사용
<button onClick={() => handleClick(123)}>클릭</button>
```

### Q: e.preventDefault()는 언제 사용하나요?

A: 브라우저의 기본 동작을 막을 때 사용합니다.

```javascript
// 폼 제출 시 페이지 새로고침 방지
const handleSubmit = (e) => {
  e.preventDefault();
  // 폼 처리 로직
};

// 링크 클릭 시 페이지 이동 방지
const handleLinkClick = (e) => {
  e.preventDefault();
  // 커스텀 동작
};
```

### Q: e.stopPropagation()은 언제 사용하나요?

A: 이벤트 버블링을 막을 때 사용합니다.

```javascript
const Card = () => {
  const handleCardClick = () => {
    console.log('카드 클릭');
  };

  const handleButtonClick = (e) => {
    e.stopPropagation();  // 카드 클릭 이벤트 발생 안 함
    console.log('버튼 클릭');
  };

  return (
    <div onClick={handleCardClick}>
      <h3>카드 제목</h3>
      <button onClick={handleButtonClick}>버튼</button>
    </div>
  );
};
```

### Q: this는 어떻게 처리하나요?

A: 화살표 함수를 사용하면 `this` 바인딩 문제가 없습니다.

```javascript
// ✅ 화살표 함수 (추천)
const Component = () => {
  const handleClick = () => {
    console.log('클릭');
  };

  return <button onClick={handleClick}>클릭</button>;
};

// 함수 선언식 사용 시 bind 필요 (비추천)
class Component extends React.Component {
  constructor() {
    super();
    this.handleClick = this.handleClick.bind(this);
  }

  handleClick() {
    console.log('클릭');
  }

  render() {
    return <button onClick={this.handleClick}>클릭</button>;
  }
}
```

## 연습 문제

카운터와 입력 필드가 있는 컴포넌트를 만들어보세요:

**요구사항:**
1. 카운터 (증가, 감소, 리셋 버튼)
2. 텍스트 입력 필드
3. 엔터 키 누르면 입력값을 알림으로 표시
4. 클리어 버튼

### 정답

```javascript
const App = () => {
  const [count, setCount] = useState(0);
  const [text, setText] = useState('');

  const handleIncrement = () => {
    setCount(count + 1);
  };

  const handleDecrement = () => {
    setCount(count - 1);
  };

  const handleReset = () => {
    setCount(0);
  };

  const handleTextChange = (e) => {
    setText(e.target.value);
  };

  const handleKeyPress = (e) => {
    if (e.key === 'Enter' && text) {
      alert(`입력: ${text}`);
    }
  };

  const handleClear = () => {
    setText('');
  };

  return (
    <div>
      <h2>카운터</h2>
      <p>현재 값: {count}</p>
      <button onClick={handleIncrement}>증가</button>
      <button onClick={handleDecrement}>감소</button>
      <button onClick={handleReset}>리셋</button>

      <h2>텍스트 입력</h2>
      <input
        type="text"
        value={text}
        onChange={handleTextChange}
        onKeyPress={handleKeyPress}
        placeholder="엔터를 눌러보세요"
      />
      <button onClick={handleClear}>클리어</button>
      <p>입력값: {text}</p>
    </div>
  );
};
```

## 요약

| 구분 | jQuery | React |
|------|--------|-------|
| 이벤트 등록 | `.on()`, `.click()` 등 | JSX에 직접 지정 |
| 이벤트 이름 | 소문자 | camelCase |
| 핸들러 전달 | 함수 직접 | 함수 참조 |
| 이벤트 위임 | 수동 | 자동 |
| this 바인딩 | 이벤트 대상 | 화살표 함수 사용 |

**핵심 원칙:**
1. 이벤트 핸들러는 JSX에서 직접 지정
2. camelCase 사용 (onClick, onChange 등)
3. 함수 참조 전달 (실행하지 않음)
4. 화살표 함수로 `this` 문제 해결
5. `e.preventDefault()`로 기본 동작 방지
6. `e.stopPropagation()`으로 이벤트 버블링 중단

**다음 단계:**
이벤트 핸들링을 이해했다면 React의 기본 개념을 모두 익힌 것입니다!
