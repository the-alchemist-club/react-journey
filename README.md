# React Journey for Designers

jQuery를 사용할 줄 아는 디자이너를 위한 React 학습 커리큘럼입니다.

**jQuery에서 React로 전환하면:**
- 재사용 가능한 컴포넌트로 UI를 구성할 수 있습니다
- 상태 관리가 더 명확하고 예측 가능해집니다
- 복잡한 UI도 유지보수하기 쉬워집니다
- 더 많은 개발자들과 협업할 수 있습니다

## 학습 로드맵

### Phase 1: 기초 다지기 (1-2주)

#### 1.1 JavaScript 모던 문법
jQuery에서 사용하던 JavaScript와 현대 JavaScript의 차이를 이해합니다.

**학습 주제:**
- `const`, `let`의 차이
- 화살표 함수 (Arrow Functions)
- 템플릿 리터럴 (Template Literals)
- 구조 분해 할당 (Destructuring)
- 스프레드 연산자 (Spread Operator)
- 모듈 시스템 (import/export)

**jQuery vs Modern JavaScript 예제:**
```javascript
// jQuery 방식
$('.button').click(function() {
  var text = $(this).text();
  console.log(text);
});

// Modern JavaScript 방식
document.querySelectorAll('.button').forEach(button => {
  button.addEventListener('click', (e) => {
    const text = e.target.textContent;
    console.log(text);
  });
});
```

#### 1.2 개발 환경 설정
- Node.js 설치
- npm/yarn 사용법
- VS Code 설정 및 확장 프로그램

### Phase 2: React 기본 개념 (2-3주)

#### 2.1 React의 핵심 개념 이해
**학습 주제:**
- JSX란 무엇인가?
- 컴포넌트의 개념
- Props와 State
- 이벤트 핸들링

**jQuery와 React 비교:**
```javascript
// jQuery: 버튼 클릭 시 카운터 증가
let count = 0;
$('#increment').click(function() {
  count++;
  $('#counter').text(count);
});

// React: 같은 기능
function Counter() {
  const [count, setCount] = React.useState(0);

  return (
    <div>
      <span id="counter">{count}</span>
      <button onClick={() => setCount(count + 1)}>
        증가
      </button>
    </div>
  );
}
```

#### 2.2 React 공식 튜토리얼: 틱택토 게임 (1주)
**실습 프로젝트:** React 핵심 개념을 실전에 적용하며 배우기

**학습 주제:**
- 게임 보드 UI 구성
- State 끌어올리기 (Lifting State Up)
- 불변성의 중요성 이해
- 게임 히스토리 관리
- 시간 여행 기능 구현

**단계별 학습:**
1. 틱택토 보드와 칸 만들기
2. 클릭으로 게임 진행하기
3. 승자 판정 및 히스토리 추가
4. 이전 수로 되돌아가기 (시간 여행)

이 튜토리얼을 통해 컴포넌트 간 데이터 공유, State 관리, 배열 불변성 등 React의 핵심 개념을 확실히 이해할 수 있습니다.

#### 2.3 실전 프로젝트: To-Do 리스트 (1주)
**실습 프로젝트:** 완전한 기능의 할 일 관리 앱

**학습 주제:**
- CRUD 기능 구현
- 폼 처리 및 유효성 검사
- 조건부 렌더링 활용
- localStorage로 데이터 저장
- 완성도 높은 UI/UX

**기능:**
- 할 일 추가/수정/삭제
- 완료 상태 토글
- 필터링 (전체/진행중/완료)
- 로컬 저장소 연동

### Phase 3: 컴포넌트 설계 (2-3주)

#### 3.1 컴포넌트 분리와 재사용
**학습 주제:**
- 컴포넌트 계층 구조
- Props를 통한 데이터 전달
- 컴포넌트 합성 (Composition)
- 조건부 렌더링
- 리스트 렌더링

**실습 프로젝트:** 제품 카탈로그
- 제품 카드 컴포넌트
- 필터링 기능
- 정렬 기능

#### 3.2 폼과 사용자 입력
**학습 주제:**
- 제어 컴포넌트 (Controlled Components)
- 폼 유효성 검사
- 여러 입력 필드 다루기

**실습 프로젝트:** 회원가입 폼
- 입력 값 검증
- 에러 메시지 표시
- 제출 처리

### Phase 4: React Hooks (2-3주)

#### 4.1 Hooks 마스터하기
**학습 주제:**
- useState: 상태 관리
- useEffect: 사이드 이펙트 처리
- useRef: DOM 접근
- useContext: 전역 상태 공유
- Custom Hooks 만들기

**jQuery AJAX vs React useEffect:**
```javascript
// jQuery
$.ajax({
  url: '/api/users',
  success: function(data) {
    $('#users').html(renderUsers(data));
  }
});

// React
function Users() {
  const [users, setUsers] = useState([]);

  useEffect(() => {
    fetch('/api/users')
      .then(res => res.json())
      .then(data => setUsers(data));
  }, []);

  return (
    <div>
      {users.map(user => <UserCard key={user.id} {...user} />)}
    </div>
  );
}
```

#### 4.2 실습 프로젝트
**날씨 앱 만들기**
- API 데이터 가져오기
- 로딩 상태 처리
- 에러 핸들링
- 도시 검색 기능

### Phase 5: 스타일링 (1-2주)

#### 5.1 React에서의 CSS
**학습 주제:**
- CSS Modules
- Styled Components
- Tailwind CSS와 React
- CSS-in-JS의 장단점

**실습:**
- 기존 프로젝트에 다양한 스타일링 방법 적용
- 반응형 디자인 구현
- 다크 모드 구현

### Phase 6: 상태 관리와 라우팅 (2-3주)

#### 6.1 React Router
**학습 주제:**
- 페이지 라우팅
- 동적 라우트
- 중첩 라우팅
- 네비게이션

#### 6.2 전역 상태 관리
**학습 주제:**
- Context API 심화
- 상태 관리 라이브러리 소개 (Redux, Zustand, Jotai)

**실습 프로젝트:** 블로그 플랫폼
- 다중 페이지
- 글 목록, 상세, 작성, 수정
- 사용자 인증 상태 관리

### Phase 7: 실전 프로젝트 (3-4주)

#### 종합 프로젝트: 포트폴리오 웹사이트 또는 대시보드
다음 요소들을 모두 포함하는 완성도 높은 프로젝트를 만듭니다:
- 컴포넌트 기반 아키텍처
- 라우팅
- API 연동
- 상태 관리
- 반응형 디자인
- 로딩 및 에러 처리
- 폼 유효성 검사

## jQuery vs React: 사고방식의 전환

### jQuery: 명령형 프로그래밍
"어떻게(How)" 구현할지에 집중합니다.
```javascript
// 요소를 찾아서
$('#button').click(function() {
  // DOM을 직접 조작
  $('#message').addClass('visible').text('클릭됨!');
});
```

### React: 선언형 프로그래밍
"무엇을(What)" 보여줄지에 집중합니다.
```javascript
function App() {
  const [clicked, setClicked] = useState(false);

  // 상태에 따라 UI가 자동으로 업데이트
  return (
    <div>
      <button onClick={() => setClicked(true)}>클릭</button>
      {clicked && <p className="visible">클릭됨!</p>}
    </div>
  );
}
```

## 학습 팁

### 1. 손으로 코드를 작성하세요
복사-붙여넣기보다 직접 타이핑하면서 익히세요.

### 2. 작은 프로젝트부터 시작하세요
처음부터 복잡한 앱을 만들려고 하지 마세요.

### 3. 공식 문서를 활용하세요
[React 공식 문서](https://react.dev/)는 최고의 학습 자료입니다.

### 4. 디버깅 도구를 사용하세요
React DevTools를 설치하여 컴포넌트 구조와 상태를 확인하세요.

### 5. 커뮤니티를 활용하세요
- Stack Overflow
- React 한국 사용자 그룹
- Discord/Slack 커뮤니티

## 추천 학습 자료

### 공식 문서
- [React 공식 문서](https://react.dev/)
- [React 한국어 문서](https://ko.react.dev/)

### 온라인 강의
- [코딩애플 - React 기초](https://codingapple.com/)
- [Nomad Coders - React 강의](https://nomadcoders.co/)
- [인프런 - React 강의](https://www.inflearn.com/)

### 연습 플랫폼
- [CodeSandbox](https://codesandbox.io/) - 브라우저에서 React 코드 작성
- [React 공식 튜토리얼](https://react.dev/learn/tutorial-tic-tac-toe) - 틱택토 게임

### YouTube 채널
- 생활코딩
- 드림코딩
- 코딩애플

## 자주 묻는 질문

### Q: jQuery를 완전히 버려야 하나요?
A: 아닙니다. 작은 인터랙션에는 jQuery가 여전히 유용할 수 있습니다. 하지만 복잡한 UI는 React가 더 적합합니다.

### Q: 얼마나 걸리나요?
A: 개인차가 있지만, 하루 2-3시간씩 투자하면 2-3개월 안에 기본적인 React 앱을 만들 수 있습니다.

### Q: JavaScript를 잘 모르는데 괜찮나요?
A: jQuery를 사용하셨다면 JavaScript 기초는 알고 계실 겁니다. 다만 ES6+ 문법을 먼저 익히시는 것을 추천합니다.

### Q: 디자인 백그라운드가 도움이 되나요?
A: 네! 컴포넌트 설계 시 UI/UX 감각이 큰 도움이 됩니다. 개발자들도 디자이너의 시각을 높이 평가합니다.

## 기여하기

이 커리큘럼을 개선하고 싶으시다면 Pull Request를 보내주세요!

## 라이선스

MIT License

---

**좋은 학습 되세요!** 질문이나 피드백이 있다면 Issue를 생성해 주세요.
