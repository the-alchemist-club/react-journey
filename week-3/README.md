# Week 3: React 공식 튜토리얼 - 틱택토 게임

> React의 핵심 개념을 실전으로 배우는 1주

## 이번 주 목표

React 공식 문서의 틱택토 튜토리얼을 통해 다음을 학습합니다:
- ✅ 컴포넌트 기반 UI 구성
- ✅ State 끌어올리기 (Lifting State Up)
- ✅ 불변성의 중요성 체득
- ✅ 시간 여행 기능 구현
- ✅ 리스트 렌더링과 key prop

## 학습 순서

### 1일-2일차: [게임 보드 설정](./01-setup-board.md)
- Square 컴포넌트 만들기
- Board 컴포넌트로 게임판 구성
- 클릭 이벤트로 X와 O 표시
- 컴포넌트 계층 구조 이해

**핵심 개념:**
- 컴포넌트 분리와 재사용
- Props를 통한 데이터 전달
- 제어 컴포넌트 (Controlled Component)

### 3일-4일차: [게임 로직과 State 끌어올리기](./02-game-logic.md)
- 승자 판정 로직 구현
- State를 Game 컴포넌트로 이동
- 불변성을 지키는 상태 업데이트
- 순수 함수로 로직 분리

**핵심 개념:**
- Lifting State Up
- 불변성 (Immutability)
- 순수 함수
- 제어 컴포넌트 패턴

### 5일-7일차: [시간 여행 기능](./03-time-travel.md)
- 게임 히스토리 배열로 관리
- 특정 시점으로 되돌아가기
- 히스토리 목록 렌더링
- key prop의 중요성 이해

**핵심 개념:**
- 히스토리 관리
- 시간 여행 (Time Travel)
- 리스트 렌더링
- key prop

## 완성된 틱택토 게임

이번 주를 마치면 다음 기능을 가진 완전한 틱택토 게임을 만들 수 있습니다:

### 기능 목록
- ✅ X와 O를 번갈아가며 플레이
- ✅ 승자 자동 판정
- ✅ 무승부 감지
- ✅ 게임 히스토리 저장
- ✅ 이전 수로 되돌아가기 (시간 여행)
- ✅ 승리 라인 하이라이트

### 전체 코드

#### App.jsx

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

export default function Game() {
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

  function handleReset() {
    setHistory([Array(9).fill(null)]);
    setCurrentMove(0);
  }

  const moves = history.map((squares, move) => {
    const isCurrent = move === currentMove;
    const description = move > 0 ? `이동 #${move}` : '게임 시작';

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

  return (
    <div className="game">
      <div className="game-board">
        <Board xIsNext={xIsNext} squares={currentSquares} onPlay={handlePlay} />
        <button className="reset-button" onClick={handleReset}>
          새 게임 시작
        </button>
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
```

#### App.css

```css
* {
  box-sizing: border-box;
}

body {
  font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen,
    Ubuntu, Cantarell, 'Open Sans', 'Helvetica Neue', sans-serif;
  margin: 20px;
  background-color: #f0f0f0;
}

.game {
  display: flex;
  flex-direction: row;
  gap: 30px;
  padding: 20px;
  max-width: 800px;
  margin: 0 auto;
}

.game-board {
  background-color: white;
  padding: 20px;
  border-radius: 10px;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
}

.status {
  margin-bottom: 20px;
  font-size: 20px;
  font-weight: bold;
  text-align: center;
  color: #333;
}

.board-row {
  display: flex;
}

.board-row:after {
  clear: both;
  content: "";
  display: table;
}

.square {
  background: #fff;
  border: 2px solid #999;
  font-size: 36px;
  font-weight: bold;
  line-height: 60px;
  height: 60px;
  width: 60px;
  margin-right: -2px;
  margin-top: -2px;
  padding: 0;
  text-align: center;
  cursor: pointer;
  transition: all 0.2s;
}

.square:hover {
  background-color: #f8f8f8;
  border-color: #666;
}

.square:focus {
  outline: none;
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

.reset-button {
  width: 100%;
  margin-top: 20px;
  padding: 10px;
  font-size: 16px;
  background-color: #4CAF50;
  color: white;
  border: none;
  border-radius: 5px;
  cursor: pointer;
  transition: background-color 0.2s;
}

.reset-button:hover {
  background-color: #45a049;
}

.game-info {
  background-color: white;
  padding: 20px;
  border-radius: 10px;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
  min-width: 200px;
}

.game-info h3 {
  margin-top: 0;
  font-size: 18px;
  color: #333;
  margin-bottom: 15px;
}

.game-info ol {
  padding-left: 20px;
  list-style: decimal;
}

.game-info li {
  margin-bottom: 8px;
}

.game-info button {
  background-color: #fff;
  border: 1px solid #ddd;
  padding: 6px 12px;
  border-radius: 4px;
  cursor: pointer;
  font-size: 14px;
  transition: all 0.2s;
}

.game-info button:hover {
  background-color: #e8e8e8;
  border-color: #999;
}

.game-info strong {
  color: #4CAF50;
  font-weight: bold;
}

@media (max-width: 600px) {
  .game {
    flex-direction: column;
  }

  .square {
    height: 50px;
    width: 50px;
    font-size: 28px;
    line-height: 50px;
  }
}
```

## 프로젝트 시작하기

### 1. 새 React 앱 생성

```bash
npm create vite@latest tic-tac-toe -- --template react
cd tic-tac-toe
npm install
```

### 2. 코드 작성

- `src/App.jsx`에 위의 코드 복사
- `src/App.css`에 스타일 복사

### 3. 개발 서버 실행

```bash
npm run dev
```

브라우저에서 `http://localhost:5173` 열기

## 학습 체크리스트

이번 주를 마치면 다음을 체크할 수 있어야 합니다:

### 컴포넌트 이해
- [ ] 컴포넌트를 작은 단위로 분리할 수 있다
- [ ] props를 통해 데이터를 전달할 수 있다
- [ ] 컴포넌트 계층 구조를 설계할 수 있다

### State 관리
- [ ] useState로 상태를 관리할 수 있다
- [ ] State를 적절한 위치에 배치할 수 있다 (Lifting State Up)
- [ ] 불변성을 지키며 상태를 업데이트할 수 있다

### 이벤트 처리
- [ ] 클릭 이벤트를 처리할 수 있다
- [ ] 이벤트 핸들러를 props로 전달할 수 있다
- [ ] 조건에 따라 이벤트를 무시할 수 있다

### 리스트 렌더링
- [ ] `map()`으로 리스트를 렌더링할 수 있다
- [ ] key prop의 중요성을 이해한다
- [ ] 적절한 key 값을 선택할 수 있다

### React 핵심 이해
- [ ] 선언형 프로그래밍을 이해한다
- [ ] 단방향 데이터 흐름을 이해한다
- [ ] 불변성이 왜 중요한지 설명할 수 있다

## 추가 도전 과제

기본 틱택토를 완성했다면 다음 기능을 추가해보세요:

### 난이도: 쉬움
- [ ] 현재 이동 위치 좌표 표시 (행, 열)
- [ ] 히스토리 정렬 순서 토글 (오름차순/내림차순)
- [ ] 게임 완료 시 축하 메시지 추가

### 난이도: 중간
- [ ] 무승부 시 모든 칸 다른 색으로 표시
- [ ] 누가 시작할지 선택하는 옵션 추가
- [ ] 게임 통계 (X 승, O 승, 무승부 수) 추적

### 난이도: 어려움
- [ ] 5x5 또는 더 큰 보드로 확장 (5개 연속으로 승리)
- [ ] AI 상대 구현 (Minimax 알고리즘)
- [ ] 멀티플레이어 모드 (WebSocket 사용)

## jQuery와 비교

### jQuery로 같은 기능을 구현한다면?

```javascript
// 전역 상태
let history = [Array(9).fill(null)];
let currentMove = 0;

// 클릭 이벤트
$('.square').click(function() {
  const index = $(this).data('index');
  const squares = history[currentMove].slice();

  if (calculateWinner(squares) || squares[index]) return;

  squares[index] = currentMove % 2 === 0 ? 'X' : 'O';

  // 히스토리 업데이트
  history = history.slice(0, currentMove + 1);
  history.push(squares);
  currentMove++;

  // DOM 수동 업데이트
  renderBoard(squares);
  renderHistory();
});

function renderBoard(squares) {
  $('.square').each(function(i) {
    $(this).text(squares[i] || '');
  });
}

function renderHistory() {
  const $list = $('#moves');
  $list.empty();

  history.forEach((_, move) => {
    const desc = move ? `Go to move #${move}` : 'Go to game start';
    const $button = $('<button>').text(desc);
    $button.click(() => jumpTo(move));
    $list.append($('<li>').append($button));
  });
}
```

**문제점:**
- 상태와 DOM을 수동으로 동기화
- 코드가 분산되어 있어 유지보수 어려움
- 버그 발생 가능성 높음

**React의 장점:**
- 상태만 관리하면 UI는 자동 업데이트
- 컴포넌트로 관심사 분리
- 버그 발생 가능성 낮음

## 핵심 개념 정리

### 1. 컴포넌트 기반 아키텍처
```
Game (전체 게임 상태 관리)
└── Board (게임 로직)
    └── Square (개별 칸)
```

### 2. 단방향 데이터 흐름
```
부모 --props--> 자식
부모 <-callback- 자식
```

### 3. 불변성
```javascript
// ❌ 잘못
squares[0] = 'X';

// ✅ 올바름
const next = squares.slice();
next[0] = 'X';
```

### 4. 선언형 UI
```jsx
// 상태에 따라 자동으로 렌더링
{winner ? '승자: ' + winner : '다음 플레이어: ' + (xIsNext ? 'X' : 'O')}
```

## 다음 단계

축하합니다! React의 핵심 개념을 익혔습니다.

**다음 주 (Week 4):**
- To-Do 리스트 실전 프로젝트
- CRUD 기능 구현
- 폼 처리 및 유효성 검사
- localStorage 연동

계속해서 실전 프로젝트로 React 실력을 향상시켜 나가세요!

## 참고 자료

- [React 공식 튜토리얼](https://react.dev/learn/tutorial-tic-tac-toe)
- [React 공식 문서 (한국어)](https://ko.react.dev/)
- [Thinking in React](https://react.dev/learn/thinking-in-react)

## 질문과 답변

### Q: 왜 Board가 아닌 Game에서 상태를 관리하나요?
A: 히스토리 기능을 위해서입니다. 여러 컴포넌트가 같은 상태를 공유해야 할 때는 공통 부모로 상태를 옮깁니다.

### Q: key prop은 왜 필요한가요?
A: React가 리스트 항목을 효율적으로 업데이트하려면 각 항목을 구별할 수 있어야 합니다. key가 있으면 어떤 항목이 변경/추가/삭제되었는지 정확히 알 수 있습니다.

### Q: slice()와 spread 연산자(...) 중 무엇을 사용해야 하나요?
A: 둘 다 배열을 복사합니다. 개인 취향이지만 spread 연산자가 더 현대적입니다.
```javascript
const copy1 = arr.slice();
const copy2 = [...arr];
```

### Q: 불변성을 지키지 않으면 어떻게 되나요?
A: React가 변경을 감지하지 못해 리렌더링이 안 될 수 있고, 시간 여행 같은 기능이 불가능해집니다.

---

**좋은 학습 되세요!** 막히는 부분이 있다면 공식 문서를 참고하거나 커뮤니티에 질문하세요.
