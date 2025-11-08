# 틱택토 시간 여행 기능

## 학습 목표
- 게임 히스토리를 배열로 관리하는 방법 학습
- 과거의 특정 시점으로 되돌아가는 기능 구현
- 리스트 렌더링과 key prop의 중요성 이해
- React의 선언형 프로그래밍 사고방식 체득

## jQuery 시대의 히스토리 관리

jQuery로 히스토리를 구현한다면 매우 복잡했을 것입니다:

```javascript
let history = [];
let currentStep = 0;

function saveMove(board) {
  // 현재 위치 이후의 히스토리 제거
  history = history.slice(0, currentStep + 1);
  // 새로운 상태 저장
  history.push(board.slice());
  currentStep = history.length - 1;
}

function jumpTo(step) {
  currentStep = step;
  const board = history[step];

  // DOM을 직접 업데이트
  $('.square').each(function(i) {
    $(this).text(board[i] || '');
  });

  // 플레이어 표시 업데이트
  const xIsNext = (step % 2) === 0;
  $('#status').text('Next player: ' + (xIsNext ? 'X' : 'O'));
}

// 히스토리 목록 렌더링
function renderHistory() {
  const $list = $('#history');
  $list.empty();

  history.forEach((_, move) => {
    const desc = move ? 'Go to move #' + move : 'Go to game start';
    const $button = $('<button>').text(desc).click(() => jumpTo(move));
    $list.append($('<li>').append($button));
  });
}
```

**문제점:**
- 히스토리와 DOM을 수동으로 동기화해야 함
- 상태 관리가 여러 곳에 분산됨
- 버그가 발생하기 쉽고 유지보수 어려움

## React 방식: 상태로 시간 여행

React에서는 히스토리를 상태로 관리하고, UI는 자동으로 업데이트됩니다.

### 1단계: 히스토리 상태 추가

```jsx
function Game() {
  // 모든 게임 상태를 배열로 저장
  const [history, setHistory] = useState([Array(9).fill(null)]);
  const [currentMove, setCurrentMove] = useState(0);

  // 현재 보드 상태는 히스토리에서 가져옴
  const currentSquares = history[currentMove];

  // 현재 플레이어는 이동 횟수로 계산
  const xIsNext = currentMove % 2 === 0;

  // ...
}
```

**핵심 개념:**
- `history`: 모든 게임 상태를 저장하는 배열
- `currentMove`: 현재 보고 있는 이동 (0부터 시작)
- `currentSquares`: 현재 보드 상태
- `xIsNext`: 홀수/짝수로 플레이어 판단

### 2단계: 이동 시 히스토리 업데이트

```jsx
function handlePlay(nextSquares) {
  // 현재 위치 이후의 미래 히스토리 제거
  const nextHistory = [...history.slice(0, currentMove + 1), nextSquares];

  // 히스토리 업데이트
  setHistory(nextHistory);

  // 최신 이동으로 이동
  setCurrentMove(nextHistory.length - 1);
}
```

**시나리오 예시:**
```javascript
// 초기: [null×9]
// 1수: [null×9], [X,null×8]
// 2수: [null×9], [X,null×8], [X,O,null×7]
// 3수: [null×9], [X,null×8], [X,O,null×7], [X,O,X,null×6]

// 2수로 돌아가서 다른 수를 두면?
// currentMove = 1
// history.slice(0, 2) → [null×9], [X,null×8]
// 새로운 수 추가 → [null×9], [X,null×8], [X,null,null,O,null×5]
// 3수 히스토리는 사라짐 (대체됨)
```

### 3단계: 히스토리 목록 렌더링

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

  function jumpTo(nextMove) {
    setCurrentMove(nextMove);
  }

  // 히스토리 목록 생성
  const moves = history.map((squares, move) => {
    let description;

    if (move > 0) {
      description = 'Go to move #' + move;
    } else {
      description = 'Go to game start';
    }

    return (
      <li key={move}>
        <button onClick={() => jumpTo(move)}>{description}</button>
      </li>
    );
  });

  return (
    <div className="game">
      <div className="game-board">
        <Board xIsNext={xIsNext} squares={currentSquares} onPlay={handlePlay} />
      </div>
      <div className="game-info">
        <ol>{moves}</ol>
      </div>
    </div>
  );
}
```

### 4단계: 현재 이동 강조하기

```jsx
const moves = history.map((squares, move) => {
  let description;

  if (move > 0) {
    description = 'Go to move #' + move;
  } else {
    description = 'Go to game start';
  }

  return (
    <li key={move}>
      <button onClick={() => jumpTo(move)}>
        {description}
      </button>
      {move === currentMove && ' ← You are here'}
    </li>
  );
});
```

## key prop의 중요성

React가 리스트를 효율적으로 업데이트하려면 각 항목에 고유한 `key`가 필요합니다.

### 나쁜 예: key 없이 또는 index를 key로

```jsx
// ❌ key 없음 - 경고 발생
moves.map((squares, move) => (
  <li>
    <button onClick={() => jumpTo(move)}>Move #{move}</button>
  </li>
));

// ⚠️ index를 key로 사용 - 항목 순서가 바뀌면 문제
moves.map((squares, move) => (
  <li key={move}>
    <button onClick={() => jumpTo(move)}>Move #{move}</button>
  </li>
));
```

### 좋은 예: 고유한 key 사용

```jsx
// ✅ 이동 번호를 key로 사용 (우리 경우는 안전)
const moves = history.map((squares, move) => (
  <li key={move}>
    <button onClick={() => jumpTo(move)}>Move #{move}</button>
  </li>
));

// ✅ 고유 ID 사용 (더 복잡한 경우)
const moves = history.map((item) => (
  <li key={item.id}>
    <button onClick={() => jumpTo(item.move)}>Move #{item.move}</button>
  </li>
));
```

**key가 중요한 이유:**
1. **성능**: React가 어떤 항목이 변경되었는지 빠르게 파악
2. **정확성**: 컴포넌트 상태가 올바른 항목과 연결됨
3. **예측 가능성**: 리스트 재정렬 시에도 올바르게 동작

### key 선택 가이드

```jsx
// ✅ 데이터베이스 ID
<li key={user.id}>{user.name}</li>

// ✅ 고유한 식별자
<li key={post.slug}>{post.title}</li>

// ✅ 생성 시간 (중복되지 않는 경우)
<li key={Date.now()}>{item.name}</li>

// ⚠️ 배열 인덱스 (정렬/삭제가 없을 때만)
<li key={index}>{item}</li>

// ❌ 랜덤 값 (매번 새로운 key 생성으로 성능 저하)
<li key={Math.random()}>{item}</li>
```

## 실전 예제: 완전한 시간 여행 게임

```jsx
import { useState } from 'react';
import './App.css';

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

function Board({ xIsNext, squares, onPlay }) {
  function handleClick(i) {
    if (calculateWinner(squares) || squares[i]) {
      return;
    }

    const nextSquares = squares.slice();
    nextSquares[i] = xIsNext ? 'X' : 'O';
    onPlay(nextSquares);
  }

  const winnerInfo = calculateWinner(squares);
  const winner = winnerInfo?.winner;
  const winningLine = winnerInfo?.line;

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

  function jumpTo(nextMove) {
    setCurrentMove(nextMove);
  }

  const moves = history.map((squares, move) => {
    const description = move > 0
      ? `Go to move #${move}`
      : 'Go to game start';

    return (
      <li key={move}>
        <button onClick={() => jumpTo(move)}>
          {description}
        </button>
        {move === currentMove && ' ← 현재'}
      </li>
    );
  });

  return (
    <div className="game">
      <div className="game-board">
        <Board xIsNext={xIsNext} squares={currentSquares} onPlay={handlePlay} />
      </div>
      <div className="game-info">
        <h3>게임 히스토리</h3>
        <ol>{moves}</ol>
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

### 스타일링

```css
.game {
  display: flex;
  flex-direction: row;
  gap: 20px;
  padding: 20px;
}

.game-info {
  background-color: #f8f8f8;
  padding: 15px;
  border-radius: 5px;
  min-width: 200px;
}

.game-info h3 {
  margin-top: 0;
  font-size: 16px;
  color: #333;
}

.game-info ol {
  padding-left: 20px;
}

.game-info li {
  margin-bottom: 8px;
}

.game-info button {
  background-color: #fff;
  border: 1px solid #ddd;
  padding: 5px 10px;
  border-radius: 3px;
  cursor: pointer;
  font-size: 14px;
  transition: all 0.2s;
}

.game-info button:hover {
  background-color: #e8e8e8;
  border-color: #999;
}

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

## 고급 기능 추가하기

### 1. 이동 좌표 표시

```jsx
function Game() {
  const [history, setHistory] = useState([
    { squares: Array(9).fill(null), lastMove: null }
  ]);
  const [currentMove, setCurrentMove] = useState(0);

  function handlePlay(nextSquares, position) {
    const nextHistory = [
      ...history.slice(0, currentMove + 1),
      { squares: nextSquares, lastMove: position }
    ];
    setHistory(nextHistory);
    setCurrentMove(nextHistory.length - 1);
  }

  const moves = history.map((step, move) => {
    const { lastMove } = step;
    let description;

    if (move > 0 && lastMove !== null) {
      const row = Math.floor(lastMove / 3) + 1;
      const col = (lastMove % 3) + 1;
      description = `Move #${move} (${row}, ${col})`;
    } else {
      description = 'Game start';
    }

    return (
      <li key={move}>
        <button onClick={() => jumpTo(move)}>
          {description}
        </button>
        {move === currentMove && ' ← 현재'}
      </li>
    );
  });

  // ...
}
```

### 2. 오름차순/내림차순 토글

```jsx
function Game() {
  const [history, setHistory] = useState([Array(9).fill(null)]);
  const [currentMove, setCurrentMove] = useState(0);
  const [isAscending, setIsAscending] = useState(true);

  // ...

  let moves = history.map((squares, move) => {
    // ... 버튼 생성
  });

  // 내림차순으로 정렬
  if (!isAscending) {
    moves = moves.reverse();
  }

  return (
    <div className="game">
      <div className="game-board">
        <Board {...boardProps} />
      </div>
      <div className="game-info">
        <button onClick={() => setIsAscending(!isAscending)}>
          {isAscending ? '최신순으로 보기' : '오래된 순으로 보기'}
        </button>
        <ol>{moves}</ol>
      </div>
    </div>
  );
}
```

### 3. 현재 이동 버튼 스타일링

```jsx
const moves = history.map((squares, move) => {
  const isCurrent = move === currentMove;
  const description = move > 0 ? `Move #${move}` : 'Game start';

  return (
    <li key={move}>
      {isCurrent ? (
        <strong>{description} ← 현재</strong>
      ) : (
        <button onClick={() => jumpTo(move)}>
          {description}
        </button>
      )}
    </li>
  );
});
```

## 연습 문제

### 문제 1: 이동 좌표 표시
각 이동 버튼에 "(행, 열)" 형식으로 좌표를 표시하세요.

<details>
<summary>힌트</summary>

- 히스토리에 `lastMove` 인덱스 저장
- `Math.floor(index / 3)`로 행 계산
- `index % 3`로 열 계산
</details>

### 문제 2: 히스토리 정렬 토글
"오름차순/내림차순" 버튼을 추가하여 히스토리 목록 순서를 바꾸세요.

<details>
<summary>답안 보기</summary>

```jsx
function Game() {
  const [isAscending, setIsAscending] = useState(true);

  let moves = history.map((squares, move) => {
    // ... 생성
  });

  if (!isAscending) {
    moves = moves.reverse();
  }

  return (
    <>
      <button onClick={() => setIsAscending(!isAscending)}>
        정렬: {isAscending ? '오름차순' : '내림차순'}
      </button>
      <ol>{moves}</ol>
    </>
  );
}
```
</details>

### 문제 3: 승자 없을 때만 이동 가능
게임이 끝나면 새로운 수를 둘 수 없도록 하세요. (이미 구현되어 있는지 확인)

<details>
<summary>확인 방법</summary>

```jsx
function handleClick(i) {
  // 승자가 있거나 칸이 채워져 있으면 무시
  if (calculateWinner(squares) || squares[i]) {
    return;
  }
  // ...
}
```
</details>

### 문제 4: 리셋 버튼
게임을 초기 상태로 되돌리는 "새 게임" 버튼을 추가하세요.

<details>
<summary>답안 보기</summary>

```jsx
function Game() {
  // ...

  function handleReset() {
    setHistory([Array(9).fill(null)]);
    setCurrentMove(0);
  }

  return (
    <div className="game">
      <div className="game-board">
        <Board {...} />
        <button onClick={handleReset} style={{ marginTop: '10px' }}>
          새 게임 시작
        </button>
      </div>
      <div className="game-info">
        <ol>{moves}</ol>
      </div>
    </div>
  );
}
```
</details>

## jQuery vs React 비교

| 측면 | jQuery | React |
|------|--------|-------|
| **히스토리 관리** | 수동 배열 관리 + DOM 동기화 | 상태로 자동 관리 |
| **시간 여행** | DOM을 직접 조작하여 되돌림 | 상태만 변경하면 자동 렌더링 |
| **리스트 렌더링** | 수동으로 HTML 생성/추가 | `map()`으로 선언적 렌더링 |
| **성능 최적화** | 수동으로 최소 변경 계산 | React가 자동으로 최적화 (key 사용) |
| **코드 복잡도** | 높음 (상태-DOM 동기화) | 낮음 (상태만 관리) |

## 핵심 요약

1. **히스토리는 배열**: 모든 게임 상태를 배열로 저장하여 시간 여행 가능
2. **불변성 유지**: `slice()`로 히스토리를 잘라내고 새 항목 추가
3. **계산된 값**: `currentMove`로 현재 상태를 결정 (파생 상태)
4. **리스트 렌더링**: `map()`으로 버튼 목록 생성
5. **key prop**: 각 리스트 항목에 고유한 key 제공
6. **선언형 UI**: 상태만 변경하면 UI는 자동으로 업데이트

## React의 핵심 철학

이 틱택토 튜토리얼을 통해 React의 핵심을 경험했습니다:

### 1. 컴포넌트 기반
UI를 재사용 가능한 조각으로 나눔
```
Game → Board → Square
```

### 2. 단방향 데이터 흐름
데이터는 위에서 아래로, 이벤트는 아래에서 위로
```
부모 (props) → 자식
자식 (callback) → 부모
```

### 3. 불변성
상태를 직접 수정하지 않고 새 객체 생성
```javascript
const next = squares.slice(); // 복사
next[i] = 'X';               // 수정
setSquares(next);            // 업데이트
```

### 4. 선언형 프로그래밍
"어떻게"가 아닌 "무엇을" 보여줄지 선언
```jsx
// 명령형 (jQuery)
if (winner) {
  $('#status').text('Winner: ' + winner);
}

// 선언형 (React)
<div>{winner ? `Winner: ${winner}` : `Next: ${xIsNext ? 'X' : 'O'}`}</div>
```

## 다음 단계

축하합니다! React 공식 튜토리얼을 완료했습니다.

이제 다음을 학습할 준비가 되었습니다:
- **실전 프로젝트**: To-Do 리스트 만들기 (Week 4)
- **Hooks 심화**: useEffect, useRef, Custom Hooks
- **상태 관리**: Context API, 상태 관리 라이브러리
- **라우팅**: React Router로 다중 페이지 앱 만들기

계속해서 실전 프로젝트를 통해 React 실력을 키워나가세요!
