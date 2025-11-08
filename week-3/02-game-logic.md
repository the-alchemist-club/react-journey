# 틱택토 게임 로직과 State 끌어올리기

## 학습 목표
- 승자 판정 로직 구현
- State를 상위 컴포넌트로 끌어올리는 방법 이해
- 불변성(Immutability)의 중요성 학습
- 여러 컴포넌트 간 상태 공유 방법 익히기

## jQuery 시대의 승자 판정

jQuery로 승자를 판정한다면 이렇게 했을 것입니다:

```javascript
// 승자 판정 패턴
const winPatterns = [
  [0, 1, 2], [3, 4, 5], [6, 7, 8], // 가로
  [0, 3, 6], [1, 4, 7], [2, 5, 8], // 세로
  [0, 4, 8], [2, 4, 6]             // 대각선
];

function checkWinner() {
  for (let pattern of winPatterns) {
    const [a, b, c] = pattern;
    const squares = $('.square');

    const valueA = squares.eq(a).text();
    const valueB = squares.eq(b).text();
    const valueC = squares.eq(c).text();

    if (valueA && valueA === valueB && valueA === valueC) {
      $('#status').text('Winner: ' + valueA);
      $('.square').off('click'); // 게임 종료
      return;
    }
  }
}

$('.square').click(function() {
  // ... 클릭 처리
  checkWinner();
});
```

**문제점:**
- DOM에서 직접 값을 읽어야 함
- 전역 함수로 승자 판정 로직 관리
- 게임 상태와 UI가 긴밀하게 결합됨

## React 방식: 순수 함수로 로직 분리

React에서는 로직을 순수 함수로 분리하고 상태를 기반으로 UI를 렌더링합니다.

### 1단계: 승자 판정 함수 만들기

```jsx
function calculateWinner(squares) {
  const lines = [
    [0, 1, 2],
    [3, 4, 5],
    [6, 7, 8],
    [0, 3, 6],
    [1, 4, 7],
    [2, 5, 8],
    [0, 4, 8],
    [2, 4, 6]
  ];

  for (let i = 0; i < lines.length; i++) {
    const [a, b, c] = lines[i];

    if (squares[a] && squares[a] === squares[b] && squares[a] === squares[c]) {
      return squares[a]; // 'X' 또는 'O' 반환
    }
  }

  return null; // 승자 없음
}
```

**핵심 특징:**
- **순수 함수**: 같은 입력에 항상 같은 출력
- **부수 효과 없음**: DOM이나 외부 상태를 변경하지 않음
- **테스트 가능**: 쉽게 단위 테스트 작성 가능

### 2단계: Board 컴포넌트에 승자 판정 추가

```jsx
function Board() {
  const [squares, setSquares] = useState(Array(9).fill(null));
  const [xIsNext, setXIsNext] = useState(true);

  function handleClick(i) {
    // 이미 승자가 있거나 칸이 채워져 있으면 무시
    if (calculateWinner(squares) || squares[i]) {
      return;
    }

    const nextSquares = squares.slice();
    nextSquares[i] = xIsNext ? 'X' : 'O';
    setSquares(nextSquares);
    setXIsNext(!xIsNext);
  }

  // 승자 확인
  const winner = calculateWinner(squares);
  let status;

  if (winner) {
    status = 'Winner: ' + winner;
  } else {
    status = 'Next player: ' + (xIsNext ? 'X' : 'O');
  }

  return (
    <>
      <div className="status">{status}</div>
      {/* ... Square 렌더링 */}
    </>
  );
}
```

## State 끌어올리기 (Lifting State Up)

여러 컴포넌트가 같은 상태를 공유해야 할 때, 상태를 공통 부모로 옮깁니다.

### 왜 State를 끌어올려야 하나요?

현재 구조에서는 Board가 모든 게임 상태를 관리합니다. 하지만 나중에 "게임 히스토리" 기능을 추가하려면 Game 컴포넌트가 필요합니다.

**Before (현재 구조):**
```
App
└── Board (상태: squares, xIsNext)
    └── Square
```

**After (State 끌어올린 후):**
```
App
└── Game (상태: history, currentMove)
    └── Board (상태 없음, props만 받음)
        └── Square
```

### 실습: State 끌어올리기

#### 1. Game 컴포넌트 생성

```jsx
function Game() {
  const [history, setHistory] = useState([Array(9).fill(null)]);
  const [currentMove, setCurrentMove] = useState(0);
  const xIsNext = currentMove % 2 === 0;
  const currentSquares = history[currentMove];

  function handlePlay(nextSquares) {
    const nextHistory = [...history.slice(0, currentMove + 1), nextSquares];
    setHistory(nextHistory);
    setCurrentMove(nextHistory.length - 1);
  }

  return (
    <div className="game">
      <div className="game-board">
        <Board
          xIsNext={xIsNext}
          squares={currentSquares}
          onPlay={handlePlay}
        />
      </div>
    </div>
  );
}
```

#### 2. Board를 제어 컴포넌트로 변경

```jsx
function Board({ xIsNext, squares, onPlay }) {
  function handleClick(i) {
    if (calculateWinner(squares) || squares[i]) {
      return;
    }

    const nextSquares = squares.slice();
    nextSquares[i] = xIsNext ? 'X' : 'O';
    onPlay(nextSquares);
  }

  const winner = calculateWinner(squares);
  let status;

  if (winner) {
    status = 'Winner: ' + winner;
  } else {
    status = 'Next player: ' + (xIsNext ? 'X' : 'O');
  }

  return (
    <>
      <div className="status">{status}</div>
      <div className="board-row">
        <Square value={squares[0]} onSquareClick={() => handleClick(0)} />
        <Square value={squares[1]} onSquareClick={() => handleClick(1)} />
        <Square value={squares[2]} onSquareClick={() => handleClick(2)} />
      </div>
      {/* ... 나머지 행 */}
    </>
  );
}
```

**변경 사항:**
- Board는 더 이상 상태를 갖지 않음
- `xIsNext`, `squares`, `onPlay`를 props로 받음
- 상태 업데이트는 `onPlay` 호출로 Game에게 위임

## 불변성(Immutability)의 중요성

### 나쁜 예: 직접 수정 (Mutation)

```jsx
// ❌ 잘못된 방법
function handleClick(i) {
  squares[i] = 'X'; // 직접 수정
  setSquares(squares); // React가 변경을 감지하지 못함!
}
```

### 좋은 예: 새 배열 생성

```jsx
// ✅ 올바른 방법
function handleClick(i) {
  const nextSquares = squares.slice(); // 복사본 생성
  nextSquares[i] = 'X';
  setSquares(nextSquares); // 새 배열로 상태 업데이트
}
```

### 불변성의 장점

#### 1. 변경 감지가 쉬움

```javascript
// 직접 수정: 변경 감지 어려움
const oldArray = [1, 2, 3];
oldArray[0] = 99;
// oldArray === oldArray (여전히 같은 참조)

// 불변 업데이트: 변경 감지 쉬움
const oldArray = [1, 2, 3];
const newArray = [...oldArray];
newArray[0] = 99;
// oldArray !== newArray (다른 참조)
```

#### 2. 시간 여행 가능

```jsx
// 히스토리에 모든 상태를 저장
const history = [
  [null, null, null, null, null, null, null, null, null], // 시작
  ['X', null, null, null, null, null, null, null, null],  // 첫 수
  ['X', 'O', null, null, null, null, null, null, null],   // 두 번째 수
  // ...
];

// 어떤 시점으로도 돌아갈 수 있음
setCurrentMove(1); // 첫 수로 돌아가기
```

#### 3. 성능 최적화

```jsx
// React.memo로 불필요한 리렌더링 방지
const Square = React.memo(function Square({ value, onSquareClick }) {
  return (
    <button className="square" onClick={onSquareClick}>
      {value}
    </button>
  );
});

// 불변성 덕분에 React가 props 변경을 정확히 감지
```

### 불변 업데이트 패턴

#### 배열

```javascript
// 항목 추가
const newArray = [...oldArray, newItem];

// 항목 제거
const newArray = oldArray.filter(item => item.id !== removeId);

// 항목 수정
const newArray = oldArray.map(item =>
  item.id === updateId ? { ...item, value: newValue } : item
);

// 배열 복사
const newArray = [...oldArray];
const newArray = oldArray.slice();
```

#### 객체

```javascript
// 프로퍼티 추가/수정
const newObj = { ...oldObj, newProp: value };

// 중첩 객체 수정
const newObj = {
  ...oldObj,
  nested: {
    ...oldObj.nested,
    value: newValue
  }
};
```

## 실전 예제: 완전한 게임 로직

```jsx
import { useState } from 'react';

function Square({ value, onSquareClick, isWinningSquare }) {
  return (
    <button
      className={`square ${isWinningSquare ? 'winning' : ''}`}
      onClick={onSquareClick}
    >
      {value}
    </button>
  );
}

function Board({ xIsNext, squares, onPlay, winningLine }) {
  function handleClick(i) {
    if (calculateWinner(squares) || squares[i]) {
      return;
    }

    const nextSquares = squares.slice();
    nextSquares[i] = xIsNext ? 'X' : 'O';
    onPlay(nextSquares);
  }

  const winner = calculateWinner(squares);
  let status;

  if (winner) {
    status = '승자: ' + winner;
  } else if (squares.every(square => square !== null)) {
    status = '무승부!';
  } else {
    status = '다음 플레이어: ' + (xIsNext ? 'X' : 'O');
  }

  return (
    <>
      <div className="status">{status}</div>
      {[0, 1, 2].map(row => (
        <div key={row} className="board-row">
          {[0, 1, 2].map(col => {
            const index = row * 3 + col;
            return (
              <Square
                key={index}
                value={squares[index]}
                onSquareClick={() => handleClick(index)}
                isWinningSquare={winningLine?.includes(index)}
              />
            );
          })}
        </div>
      ))}
    </>
  );
}

function Game() {
  const [history, setHistory] = useState([Array(9).fill(null)]);
  const [currentMove, setCurrentMove] = useState(0);
  const xIsNext = currentMove % 2 === 0;
  const currentSquares = history[currentMove];

  function handlePlay(nextSquares) {
    const nextHistory = [...history.slice(0, currentMove + 1), nextSquares];
    setHistory(nextHistory);
    setCurrentMove(nextHistory.length - 1);
  }

  const winnerInfo = calculateWinner(currentSquares);

  return (
    <div className="game">
      <div className="game-board">
        <Board
          xIsNext={xIsNext}
          squares={currentSquares}
          onPlay={handlePlay}
          winningLine={winnerInfo?.line}
        />
      </div>
    </div>
  );
}

function calculateWinner(squares) {
  const lines = [
    [0, 1, 2],
    [3, 4, 5],
    [6, 7, 8],
    [0, 3, 6],
    [1, 4, 7],
    [2, 5, 8],
    [0, 4, 8],
    [2, 4, 6]
  ];

  for (let i = 0; i < lines.length; i++) {
    const [a, b, c] = lines[i];
    if (squares[a] && squares[a] === squares[b] && squares[a] === squares[c]) {
      return { winner: squares[a], line: lines[i] };
    }
  }

  return null;
}

export default Game;
```

### 추가 CSS

```css
.winning {
  background-color: #90EE90 !important;
  animation: pulse 0.5s ease-in-out;
}

@keyframes pulse {
  0%, 100% {
    transform: scale(1);
  }
  50% {
    transform: scale(1.05);
  }
}
```

## 연습 문제

### 문제 1: 무승부 감지
모든 칸이 채워졌지만 승자가 없을 때 "무승부!"를 표시하세요.

<details>
<summary>답안 보기</summary>

```jsx
const winner = calculateWinner(squares);
let status;

if (winner) {
  status = '승자: ' + winner;
} else if (squares.every(square => square !== null)) {
  status = '무승부!';
} else {
  status = '다음 플레이어: ' + (xIsNext ? 'X' : 'O');
}
```
</details>

### 문제 2: 리셋 버튼
게임을 초기 상태로 되돌리는 "다시 시작" 버튼을 추가하세요.

<details>
<summary>답안 보기</summary>

```jsx
function Game() {
  const [history, setHistory] = useState([Array(9).fill(null)]);
  const [currentMove, setCurrentMove] = useState(0);

  function handleReset() {
    setHistory([Array(9).fill(null)]);
    setCurrentMove(0);
  }

  return (
    <div className="game">
      <div className="game-board">
        <Board {...props} />
      </div>
      <button onClick={handleReset}>다시 시작</button>
    </div>
  );
}
```
</details>

### 문제 3: 승리 라인 하이라이트
승리한 3개의 칸을 하이라이트하세요. (calculateWinner가 승리 라인도 반환하도록 수정)

<details>
<summary>힌트</summary>

- calculateWinner가 `{ winner: 'X', line: [0, 1, 2] }` 형태로 반환
- Square 컴포넌트에 `isWinningSquare` prop 전달
- 조건부 className 적용
</details>

## jQuery vs React 비교

| 측면 | jQuery | React |
|------|--------|-------|
| **로직 위치** | 이벤트 핸들러 내부 | 순수 함수로 분리 |
| **상태 공유** | 전역 변수 또는 DOM | State 끌어올리기 |
| **불변성** | 직접 수정 (mutation) | 새 객체/배열 생성 |
| **변경 감지** | 수동으로 확인 | 자동 감지 및 리렌더링 |
| **시간 여행** | 불가능 | 가능 (히스토리 저장) |
| **테스트** | 어려움 (DOM 필요) | 쉬움 (순수 함수) |

## 핵심 요약

1. **순수 함수**: 게임 로직을 순수 함수로 분리하여 테스트하기 쉽게 만듦
2. **State 끌어올리기**: 여러 컴포넌트가 공유하는 상태는 공통 부모로 이동
3. **제어 컴포넌트**: 자식 컴포넌트는 상태를 갖지 않고 props를 통해 제어됨
4. **불변성**: 상태를 직접 수정하지 않고 새 객체/배열을 생성
5. **단방향 데이터 흐름**: 부모 → 자식 (props), 자식 → 부모 (콜백 함수)

## 다음 단계

다음 문서에서는:
- 게임 히스토리 저장
- 시간 여행 기능 구현
- 히스토리 목록 렌더링

이어서 `03-time-travel.md`를 학습하세요!
