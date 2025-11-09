# 틱택토 게임 보드 설정

## 학습 목표
- React로 UI 컴포넌트 구조를 설계하는 방법 이해
- props를 통해 데이터를 전달하는 방법 학습
- 이벤트 핸들러를 연결하는 방법 실습

## jQuery 시대의 틱택토

jQuery로 틱택토를 만든다면 이렇게 했을 것입니다:

```html
<div id="board">
  <div class="square" data-index="0"></div>
  <div class="square" data-index="1"></div>
  <div class="square" data-index="2"></div>
  <div class="square" data-index="3"></div>
  <div class="square" data-index="4"></div>
  <div class="square" data-index="5"></div>
  <div class="square" data-index="6"></div>
  <div class="square" data-index="7"></div>
  <div class="square" data-index="8"></div>
</div>
```

```javascript
// jQuery 방식
let currentPlayer = 'X';
let board = ['', '', '', '', '', '', '', '', ''];

$('.square').click(function() {
  const index = $(this).data('index');

  if (board[index] === '') {
    board[index] = currentPlayer;
    $(this).text(currentPlayer);
    currentPlayer = currentPlayer === 'X' ? 'O' : 'X';
  }
});
```

**문제점:**
- HTML과 JavaScript가 분리되어 있어 관리가 어려움
- 보드 상태를 직접 DOM에서 관리
- 재사용 가능한 컴포넌트가 아님
- 보드 상태와 UI가 동기화되지 않을 수 있음

## React 방식: 컴포넌트로 생각하기

React에서는 UI를 컴포넌트 단위로 나눕니다.

### 1단계: Square 컴포넌트 만들기

가장 작은 단위인 칸(Square)부터 시작합니다.

```jsx
function Square({ value, onSquareClick }) {
  return (
    <button className="square" onClick={onSquareClick}>
      {value}
    </button>
  );
}
```

**핵심 개념:**
- `value`: 부모로부터 받은 값 (X, O, 또는 빈 값)
- `onSquareClick`: 부모로부터 받은 클릭 핸들러
- Square는 자신의 상태를 갖지 않음 (Controlled Component)

### 2단계: Board 컴포넌트 만들기

9개의 Square를 관리하는 보드를 만듭니다.

```jsx
function Board() {
  const [squares, setSquares] = useState(Array(9).fill(null));
  const [xIsNext, setXIsNext] = useState(true);

  function handleClick(i) {
    // 이미 값이 있으면 무시
    if (squares[i]) {
      return;
    }

    // 새로운 배열을 만들어 불변성 유지
    const nextSquares = squares.slice();

    if (xIsNext) {
      nextSquares[i] = 'X';
    } else {
      nextSquares[i] = 'O';
    }

    setSquares(nextSquares);
    setXIsNext(!xIsNext);
  }

  return (
    <>
      <div className="board-row">
        <Square value={squares[0]} onSquareClick={() => handleClick(0)} />
        <Square value={squares[1]} onSquareClick={() => handleClick(1)} />
        <Square value={squares[2]} onSquareClick={() => handleClick(2)} />
      </div>
      <div className="board-row">
        <Square value={squares[3]} onSquareClick={() => handleClick(3)} />
        <Square value={squares[4]} onSquareClick={() => handleClick(4)} />
        <Square value={squares[5]} onSquareClick={() => handleClick(5)} />
      </div>
      <div className="board-row">
        <Square value={squares[6]} onSquareClick={() => handleClick(6)} />
        <Square value={squares[7]} onSquareClick={() => handleClick(7)} />
        <Square value={squares[8]} onSquareClick={() => handleClick(8)} />
      </div>
    </>
  );
}
```

**핵심 개념:**
- `squares`: 9개 칸의 상태를 배열로 관리
- `xIsNext`: 다음 차례가 X인지 O인지 추적
- `handleClick`: 클릭 시 상태 업데이트
- `slice()`: 배열을 복사하여 불변성 유지

**문제점:**
- 9개의 Square를 수동으로 작성해야 함
- 코드 중복이 많음
- 보드 크기를 바꾸려면 모든 코드를 수정해야 함

### 3단계: 반복문으로 Square 재사용하기

React의 강력한 점은 JavaScript의 모든 기능을 사용할 수 있다는 것입니다. `map()`을 사용하여 Square를 동적으로 생성할 수 있습니다.

#### 방법 1: board-row를 유지하면서 map() 사용

```jsx
function Board() {
  const [squares, setSquares] = useState(Array(9).fill(null));
  const [xIsNext, setXIsNext] = useState(true);

  function handleClick(i) {
    if (squares[i]) return;

    const nextSquares = squares.slice();
    nextSquares[i] = xIsNext ? 'X' : 'O';
    setSquares(nextSquares);
    setXIsNext(!xIsNext);
  }

  // 한 행을 렌더링하는 헬퍼 함수
  const renderRow = (rowIndex) => {
    return (
      <div key={rowIndex} className="board-row">
        {[0, 1, 2].map(col => {
          const index = rowIndex * 3 + col;
          return (
            <Square
              key={index}
              value={squares[index]}
              onSquareClick={() => handleClick(index)}
            />
          );
        })}
      </div>
    );
  };

  return (
    <>
      <div className="status">
        Next player: {xIsNext ? 'X' : 'O'}
      </div>
      {/* 0, 1, 2 행을 반복 */}
      {[0, 1, 2].map(row => renderRow(row))}
    </>
  );
}
```

**장점:**
- 코드 중복 제거
- 보드 크기를 쉽게 변경 가능
- 가독성 향상

#### 방법 2: CSS Grid를 사용한 더 간결한 방법

```jsx
function Board() {
  const [squares, setSquares] = useState(Array(9).fill(null));
  const [xIsNext, setXIsNext] = useState(true);

  function handleClick(i) {
    if (squares[i]) return;

    const nextSquares = squares.slice();
    nextSquares[i] = xIsNext ? 'X' : 'O';
    setSquares(nextSquares);
    setXIsNext(!xIsNext);
  }

  return (
    <>
      <div className="status">
        Next player: {xIsNext ? 'X' : 'O'}
      </div>
      <div className="board-grid">
        {squares.map((value, index) => (
          <Square
            key={index}
            value={value}
            onSquareClick={() => handleClick(index)}
          />
        ))}
      </div>
    </>
  );
}
```

**장점:**
- 훨씬 더 간결한 코드
- board-row div 불필요
- CSS Grid로 레이아웃 처리

**주의사항:**
- `key` prop을 반드시 제공해야 함
- 리스트의 각 항목은 고유한 key가 필요
- 여기서는 index를 key로 사용 (항목의 순서가 바뀌지 않으므로 안전)

### 4단계: 스타일링

#### Flexbox 방식 (board-row 사용)

```css
.square {
  background: #fff;
  border: 1px solid #999;
  font-size: 24px;
  font-weight: bold;
  line-height: 34px;
  height: 34px;
  margin-right: -1px;
  margin-top: -1px;
  padding: 0;
  text-align: center;
  width: 34px;
  cursor: pointer;
}

.board-row {
  display: flex;
}

.board-row:after {
  clear: both;
  content: "";
  display: table;
}
```

#### CSS Grid 방식 (board-grid 사용)

```css
.square {
  background: #fff;
  border: 1px solid #999;
  font-size: 24px;
  font-weight: bold;
  line-height: 34px;
  height: 60px;
  width: 60px;
  padding: 0;
  text-align: center;
  cursor: pointer;
}

.board-grid {
  display: grid;
  grid-template-columns: repeat(3, 60px);
  grid-template-rows: repeat(3, 60px);
  gap: 0;
  width: fit-content;
}

/* Grid에서는 border를 겹치게 하기 위한 다른 방식 */
.board-grid .square {
  margin-right: -1px;
  margin-bottom: -1px;
}
```

**Grid의 장점:**
- 레이아웃을 CSS로 완전히 제어
- 더 유연한 그리드 시스템
- 반응형 디자인에 유리
- 보드 크기를 쉽게 조정 가능 (예: 4x4, 5x5)

## 실전 예제: 완전한 틱택토 보드

### 버전 1: Flexbox + board-row (수동 나열)

```jsx
import { useState } from 'react';
import './App.css';

function Square({ value, onSquareClick }) {
  return (
    <button className="square" onClick={onSquareClick}>
      {value}
    </button>
  );
}

function Board() {
  const [squares, setSquares] = useState(Array(9).fill(null));
  const [xIsNext, setXIsNext] = useState(true);

  function handleClick(i) {
    if (squares[i]) {
      return;
    }

    const nextSquares = squares.slice();
    nextSquares[i] = xIsNext ? 'X' : 'O';
    setSquares(nextSquares);
    setXIsNext(!xIsNext);
  }

  const status = 'Next player: ' + (xIsNext ? 'X' : 'O');

  return (
    <>
      <div className="status">{status}</div>
      <div className="board-row">
        <Square value={squares[0]} onSquareClick={() => handleClick(0)} />
        <Square value={squares[1]} onSquareClick={() => handleClick(1)} />
        <Square value={squares[2]} onSquareClick={() => handleClick(2)} />
      </div>
      <div className="board-row">
        <Square value={squares[3]} onSquareClick={() => handleClick(3)} />
        <Square value={squares[4]} onSquareClick={() => handleClick(4)} />
        <Square value={squares[5]} onSquareClick={() => handleClick(5)} />
      </div>
      <div className="board-row">
        <Square value={squares[6]} onSquareClick={() => handleClick(6)} />
        <Square value={squares[7]} onSquareClick={() => handleClick(7)} />
        <Square value={squares[8]} onSquareClick={() => handleClick(8)} />
      </div>
    </>
  );
}

export default function App() {
  return <Board />;
}
```

### 버전 2: CSS Grid + map() (권장)

```jsx
import { useState } from 'react';
import './App.css';

function Square({ value, onSquareClick }) {
  return (
    <button className="square" onClick={onSquareClick}>
      {value}
    </button>
  );
}

function Board() {
  const [squares, setSquares] = useState(Array(9).fill(null));
  const [xIsNext, setXIsNext] = useState(true);

  function handleClick(i) {
    if (squares[i]) {
      return;
    }

    const nextSquares = squares.slice();
    nextSquares[i] = xIsNext ? 'X' : 'O';
    setSquares(nextSquares);
    setXIsNext(!xIsNext);
  }

  const status = 'Next player: ' + (xIsNext ? 'X' : 'O');

  return (
    <>
      <div className="status">{status}</div>
      <div className="board-grid">
        {squares.map((value, index) => (
          <Square
            key={index}
            value={value}
            onSquareClick={() => handleClick(index)}
          />
        ))}
      </div>
    </>
  );
}

export default function App() {
  return <Board />;
}
```

**버전 2가 더 나은 이유:**
- 코드가 훨씬 간결함 (45줄 → 35줄)
- 중복 코드 제거
- 확장하기 쉬움 (4x4, 5x5 보드 등)
- React의 선언형 방식을 더 잘 활용

## 컴포넌트 구조 이해하기

```
App
└── Board
    ├── Square (index 0)
    ├── Square (index 1)
    ├── Square (index 2)
    ├── Square (index 3)
    ├── Square (index 4)
    ├── Square (index 5)
    ├── Square (index 6)
    ├── Square (index 7)
    └── Square (index 8)
```

**데이터 흐름:**
1. Board가 `squares` 상태 관리
2. 각 Square에게 `value`와 `onSquareClick` props 전달
3. Square 클릭 시 `onSquareClick` 호출
4. Board의 `handleClick`이 실행되어 상태 업데이트
5. 상태 변경으로 Board와 모든 Square 리렌더링

## 연습 문제

### 문제 1: 호버 효과 추가
Square에 마우스를 올렸을 때 배경색이 변하도록 CSS를 추가하세요.

<details>
<summary>답안 보기</summary>

```css
.square:hover {
  background-color: #f0f0f0;
}

.square:hover:disabled {
  background-color: #fff;
  cursor: not-allowed;
}
```
</details>

### 문제 2: 이미 채워진 칸 비활성화
이미 X나 O가 있는 칸은 클릭할 수 없도록 Square 컴포넌트를 수정하세요.

<details>
<summary>답안 보기</summary>

```jsx
function Square({ value, onSquareClick }) {
  return (
    <button
      className="square"
      onClick={onSquareClick}
      disabled={value !== null}
    >
      {value}
    </button>
  );
}
```
</details>

### 문제 3: 반복문으로 Square 렌더링
9개의 Square를 수동으로 나열하는 대신, `map()`을 사용하여 렌더링하세요.

<details>
<summary>힌트</summary>

- 0-8까지의 배열을 만들고 `map()` 사용
- 3개씩 그룹화하여 행 만들기 (Flexbox 방식)
- 또는 CSS Grid 사용 (더 간단)
- `key` prop 잊지 말기
</details>

<details>
<summary>답안 1: Flexbox 방식 (board-row 유지)</summary>

```jsx
function Board() {
  const [squares, setSquares] = useState(Array(9).fill(null));
  const [xIsNext, setXIsNext] = useState(true);

  function handleClick(i) {
    if (squares[i]) return;

    const nextSquares = squares.slice();
    nextSquares[i] = xIsNext ? 'X' : 'O';
    setSquares(nextSquares);
    setXIsNext(!xIsNext);
  }

  const renderSquare = (i) => (
    <Square
      key={i}
      value={squares[i]}
      onSquareClick={() => handleClick(i)}
    />
  );

  const renderRow = (rowIndex) => (
    <div key={rowIndex} className="board-row">
      {[0, 1, 2].map(col => renderSquare(rowIndex * 3 + col))}
    </div>
  );

  return (
    <>
      <div className="status">
        Next player: {xIsNext ? 'X' : 'O'}
      </div>
      {[0, 1, 2].map(row => renderRow(row))}
    </>
  );
}
```
</details>

<details>
<summary>답안 2: CSS Grid 방식 (권장)</summary>

```jsx
function Board() {
  const [squares, setSquares] = useState(Array(9).fill(null));
  const [xIsNext, setXIsNext] = useState(true);

  function handleClick(i) {
    if (squares[i]) return;

    const nextSquares = squares.slice();
    nextSquares[i] = xIsNext ? 'X' : 'O';
    setSquares(nextSquares);
    setXIsNext(!xIsNext);
  }

  return (
    <>
      <div className="status">
        Next player: {xIsNext ? 'X' : 'O'}
      </div>
      <div className="board-grid">
        {squares.map((value, index) => (
          <Square
            key={index}
            value={value}
            onSquareClick={() => handleClick(index)}
          />
        ))}
      </div>
    </>
  );
}
```

**CSS:**
```css
.board-grid {
  display: grid;
  grid-template-columns: repeat(3, 60px);
  grid-template-rows: repeat(3, 60px);
}
```
</details>

## jQuery vs React 비교

| 측면 | jQuery | React |
|------|--------|-------|
| **UI 구성** | HTML 직접 작성 | 컴포넌트 조합 |
| **상태 관리** | 전역 변수 또는 data 속성 | useState Hook |
| **이벤트 처리** | `.click()` 등으로 직접 연결 | props로 핸들러 전달 |
| **UI 업데이트** | `.text()`, `.html()` 등으로 직접 조작 | 상태 변경 시 자동 리렌더링 |
| **재사용성** | 낮음 (코드 복사 필요) | 높음 (컴포넌트 재사용) |
| **데이터 흐름** | 양방향 (어디서든 수정 가능) | 단방향 (위에서 아래로) |

## 핵심 요약

1. **컴포넌트 분리**: UI를 작은 재사용 가능한 조각으로 나눔
2. **Props**: 부모에서 자식으로 데이터 전달
3. **State**: 컴포넌트의 변하는 데이터 관리
4. **불변성**: 상태를 직접 수정하지 않고 새 객체/배열 생성
5. **단방향 데이터 흐름**: 데이터는 위에서 아래로만 흐름

## 다음 단계

다음 문서에서는:
- 승자 판정 로직 추가
- State를 상위 컴포넌트로 끌어올리기
- 불변성을 유지하는 이유 심화 학습

이어서 `02-game-logic.md`를 학습하세요!
