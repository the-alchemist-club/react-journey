# Week 0: JavaScript 모던 문법 익히기

React를 배우기 전에 반드시 알아야 할 JavaScript 최신 문법을 학습합니다.

## 학습 목표

jQuery에서 사용하던 오래된 JavaScript 문법에서 벗어나, 현대적인 JavaScript(ES6+) 문법을 익힙니다. 이는 React 코드를 이해하고 작성하는 데 필수적입니다.

## 왜 이것부터 배워야 할까요?

React 공식 문서나 튜토리얼을 보면 다음과 같은 코드를 자주 만나게 됩니다:

```javascript
const MyComponent = ({ name, age }) => {
  const [count, setCount] = useState(0);

  return (
    <div>
      <h1>{`안녕하세요, ${name}님!`}</h1>
      <p>나이: {age}세</p>
      <button onClick={() => setCount(count + 1)}>
        클릭 수: {count}
      </button>
    </div>
  );
};

export default MyComponent;
```

이 코드를 이해하려면:
- `const`가 무엇인지
- `() =>` 화살표 함수가 무엇인지
- `{ name, age }` 구조 분해 할당이 무엇인지
- `` `안녕하세요, ${name}님` `` 템플릿 리터럴이 무엇인지
- `export default`가 무엇인지

알아야 합니다. 이번 주차에서 모두 배웁니다!

## 학습 내용

### 1. [const와 let](./01-const-let.md)
- `var`의 문제점
- `const`와 `let`의 차이
- 블록 스코프
- 언제 `const`를 쓰고 언제 `let`을 쓸까?

**예상 소요 시간:** 30분

### 2. [화살표 함수 (Arrow Functions)](./02-arrow-functions.md)
- 기본 문법
- `this`의 동작 차이
- 언제 사용하고 언제 피해야 할까?
- jQuery 콜백을 화살표 함수로 변환하기

**예상 소요 시간:** 1시간

### 3. [템플릿 리터럴 (Template Literals)](./03-template-literals.md)
- 백틱(`` ` ``) 사용법
- 문자열 보간 `${}`
- 여러 줄 문자열
- HTML 생성 시 활용

**예상 소요 시간:** 30분

### 4. [구조 분해 할당 (Destructuring)](./04-destructuring.md)
- 객체 구조 분해
- 배열 구조 분해
- 함수 매개변수에서 활용
- React Props에서의 사용

**예상 소요 시간:** 1시간

### 5. [스프레드 연산자 (Spread Operator)](./05-spread-operator.md)
- `...` 연산자 사용법
- 배열/객체 복사와 합치기
- React State 업데이트에서 필수!
- 나머지 매개변수 (Rest Parameters)

**예상 소요 시간:** 1시간

### 6. [모듈 시스템 (ES6 Modules)](./06-modules.md)
- `import`와 `export`
- Named vs Default export
- 파일 구조 설계
- React 컴포넌트 모듈화

**예상 소요 시간:** 1시간

## 학습 순서

1. **순서대로 읽기**: 위에서 아래로 순서대로 읽는 것을 추천합니다
2. **직접 타이핑**: 예제 코드를 복사-붙여넣기하지 말고 직접 타이핑하세요
3. **연습 문제 풀기**: 각 문서 끝에 있는 연습 문제를 반드시 풀어보세요
4. **비교하며 익히기**: jQuery 코드와 비교하면서 차이점을 이해하세요

## 실습 환경 설정

### 방법 1: 브라우저 콘솔 (가장 쉬움)

1. Chrome 브라우저 열기
2. `F12` 또는 `Cmd+Option+I` (Mac) 눌러서 개발자 도구 열기
3. Console 탭에서 코드 입력하고 실행

```javascript
// 콘솔에서 바로 테스트
const greet = (name) => `안녕하세요, ${name}님!`;
console.log(greet('홍길동'));
```

### 방법 2: CodeSandbox (추천)

1. [CodeSandbox](https://codesandbox.io/) 접속
2. "Create Sandbox" → "Vanilla" 선택
3. `index.js` 파일에서 코드 작성

### 방법 3: VS Code + Node.js (로컬 환경)

1. [Node.js](https://nodejs.org/) 설치
2. [VS Code](https://code.visualstudio.com/) 설치
3. 폴더 생성 후 `.js` 파일 만들기
4. 터미널에서 `node 파일명.js` 실행

```bash
# 예제
node test.js
```

## 학습 체크리스트

각 주제를 학습한 후 체크해보세요:

- [ ] const와 let의 차이를 이해하고 적절히 사용할 수 있다
- [ ] 화살표 함수의 문법을 이해하고 작성할 수 있다
- [ ] 화살표 함수와 일반 함수의 this 차이를 이해한다
- [ ] 템플릿 리터럴로 문자열을 만들 수 있다
- [ ] 객체와 배열을 구조 분해 할당할 수 있다
- [ ] 스프레드 연산자로 배열/객체를 복사하고 합칠 수 있다
- [ ] import/export로 모듈을 만들고 사용할 수 있다

## jQuery vs 현대 JavaScript 요약

| 작업 | jQuery | 현대 JavaScript |
|------|--------|----------------|
| 변수 선언 | `var name = "홍길동"` | `const name = "홍길동"` |
| 함수 | `function() { }` | `() => { }` |
| 문자열 연결 | `"안녕, " + name` | `` `안녕, ${name}` `` |
| 객체 속성 추출 | `var name = user.name` | `const { name } = user` |
| 배열 복사 | `arr.slice()` | `[...arr]` |
| 객체 복사 | `$.extend({}, obj)` | `{ ...obj }` |
| 파일 분리 | `<script>` 태그 | `import/export` |

## 다음 단계

이번 주차를 완료하면:
- React 코드를 읽고 이해할 수 있습니다
- 현대적인 JavaScript로 코드를 작성할 수 있습니다
- React 컴포넌트를 만들 준비가 됩니다

## 추가 자료

### 공식 문서
- [MDN JavaScript Guide](https://developer.mozilla.org/ko/docs/Web/JavaScript/Guide)
- [ES6 Features](http://es6-features.org/)

### 온라인 강의
- [모던 JavaScript 튜토리얼](https://ko.javascript.info/)
- [생활코딩 - JavaScript](https://opentutorials.org/course/743)

### 연습 사이트
- [JavaScript.info Tasks](https://javascript.info/)
- [Exercism - JavaScript Track](https://exercism.org/tracks/javascript)

## 팁

1. **한 번에 모두 이해하려고 하지 마세요**
   - 처음에는 어색할 수 있습니다
   - 사용하다 보면 자연스러워집니다

2. **jQuery와 비교하며 배우세요**
   - 이미 아는 것과 연결하면 이해가 빠릅니다
   - 각 문서에 비교 예제가 있습니다

3. **직접 코드를 작성하세요**
   - 읽기만 하면 금방 잊어버립니다
   - 손으로 타이핑하면서 익히세요

4. **에러를 두려워하지 마세요**
   - 에러 메시지를 읽고 이해하려고 노력하세요
   - 에러는 최고의 선생님입니다

## 질문이 있나요?

- 이해가 안 되는 부분이 있다면 다시 읽어보세요
- 구글에서 검색해보세요 (예: "javascript const vs let")
- MDN 문서를 참고하세요
- 나중에 React를 배우면서 자연스럽게 이해될 수도 있습니다

---

**준비되셨나요?** [01-const-let.md](./01-const-let.md)부터 시작하세요!
