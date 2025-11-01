# Week 1: 개발 환경 설정

React 개발을 시작하기 위한 필수 도구를 설치하고 설정합니다.

## 학습 목표

jQuery 시대에는 브라우저와 텍스트 에디터만 있으면 됐지만, 현대적인 React 개발에는 몇 가지 도구가 더 필요합니다. 이번 주차에서는 React 개발 환경을 완벽하게 구축합니다.

## 왜 이런 도구들이 필요할까요?

### jQuery 시대

```html
<!DOCTYPE html>
<html>
<head>
  <title>My Site</title>
</head>
<body>
  <div id="app"></div>

  <!-- jQuery CDN -->
  <script src="https://code.jquery.com/jquery-3.6.0.min.js"></script>
  <script src="app.js"></script>
</body>
</html>
```

**필요한 것:**
- ✅ 브라우저
- ✅ 텍스트 에디터
- ✅ 끝!

### React 시대

```javascript
import React from 'react';
import ReactDOM from 'react-dom';
import App from './App';

ReactDOM.render(<App />, document.getElementById('root'));
```

**필요한 것:**
- ✅ Node.js (npm/yarn)
- ✅ 빌드 도구 (Webpack, Vite 등)
- ✅ 코드 에디터 (VS Code)
- ✅ 확장 프로그램들

왜 이렇게 복잡해졌을까요?
- **JSX 변환**: `<div>Hello</div>` → `React.createElement('div', null, 'Hello')`
- **모듈 번들링**: 수십 개의 파일 → 하나의 파일
- **최신 문법 변환**: ES6+ → 구형 브라우저용 JavaScript
- **개발 서버**: 코드 수정 시 자동 새로고침

## 학습 내용

### 1. [Node.js 설치](./01-nodejs-install.md)

Node.js는 React 개발의 핵심입니다.

**배우는 것:**
- Node.js가 무엇이고 왜 필요한지
- 운영체제별 설치 방법
- 설치 확인 및 테스트
- 버전 관리 도구 (nvm)

**핵심 명령어:**
```bash
node --version
npm --version
node test.js
```

**예상 소요 시간:** 30분

### 2. [npm과 yarn 사용법](./02-npm-yarn.md)

패키지 관리자로 라이브러리를 쉽게 설치하고 관리합니다.

**배우는 것:**
- npm과 yarn의 개념
- package.json 이해
- 패키지 설치/제거
- npm scripts 활용
- 의존성 관리

**핵심 명령어:**
```bash
npm init -y
npm install react
npm start
npm run build
```

**예상 소요 시간:** 1시간

### 3. [VS Code 설정 및 확장 프로그램](./03-vscode-setup.md)

React 개발에 최적화된 에디터를 설정합니다.

**배우는 것:**
- VS Code 설치 및 기본 사용법
- 필수 확장 프로그램 10가지
- 단축키와 생산성 팁
- 코드 포맷팅 자동화
- Emmet으로 빠른 코딩

**필수 확장 프로그램:**
- ES7 React Snippets
- Prettier
- ESLint
- Auto Rename Tag

**예상 소요 시간:** 1시간

## 학습 순서

1. **Node.js 먼저 설치**: 나머지 도구들의 기반
2. **npm 명령어 익히기**: 패키지 설치 및 관리
3. **VS Code 설정**: 편안한 개발 환경

각 단계를 건너뛰지 말고 순서대로 진행하세요!

## 실습: 첫 React 앱 실행해보기

모든 설정을 완료했다면 간단한 React 앱을 실행해봅시다.

### 1. React 앱 생성

```bash
# Create React App으로 프로젝트 생성
npx create-react-app my-first-app

# 프로젝트 폴더로 이동
cd my-first-app

# VS Code로 열기
code .
```

### 2. 개발 서버 실행

```bash
npm start
```

브라우저가 자동으로 열리고 `http://localhost:3000`에 React 앱이 실행됩니다!

### 3. 코드 수정해보기

`src/App.js` 파일을 열고:

```javascript
function App() {
  return (
    <div className="App">
      <h1>안녕하세요, React!</h1>
      <p>첫 번째 React 앱입니다.</p>
    </div>
  );
}

export default App;
```

파일을 저장하면 브라우저가 자동으로 새로고침됩니다!

## jQuery vs React 개발 환경 비교

| 항목 | jQuery | React |
|------|--------|-------|
| 설치 | CDN 링크 추가 | npm install |
| 파일 구조 | 1~3개 파일 | 수십 개 파일 |
| 빌드 | 불필요 | 필수 (npm run build) |
| 개발 서버 | 파일 직접 열기 | npm start |
| 코드 분할 | 수동 | 자동 (모듈 시스템) |
| 라이브러리 관리 | 수동 (CDN) | 자동 (package.json) |

## 학습 체크리스트

각 단계를 완료했는지 확인하세요:

### Node.js
- [ ] Node.js 설치 완료
- [ ] `node --version` 확인
- [ ] `npm --version` 확인
- [ ] 간단한 JavaScript 파일 실행 테스트

### npm/yarn
- [ ] `npm init -y`로 프로젝트 생성 가능
- [ ] `npm install` 명령어 이해
- [ ] package.json 역할 이해
- [ ] node_modules 폴더 이해
- [ ] npm scripts 사용법 이해

### VS Code
- [ ] VS Code 설치 완료
- [ ] 한국어 언어팩 설치 (선택)
- [ ] ES7 React Snippets 설치
- [ ] Prettier 설치 및 설정
- [ ] ESLint 설치
- [ ] Auto Rename Tag 설치
- [ ] 주요 단축키 3개 이상 익힘
- [ ] Emmet 사용법 이해

### 실습
- [ ] Create React App으로 프로젝트 생성
- [ ] `npm start`로 개발 서버 실행
- [ ] 브라우저에서 앱 확인
- [ ] 코드 수정 후 자동 새로고침 확인

## 문제 해결 가이드

### Node.js 설치 후 명령어가 안 됨

```bash
# 터미널 재시작
# Windows: PowerShell/CMD 완전히 종료 후 재실행
# Mac: Terminal 재시작

# 여전히 안 되면 경로 확인
where node  # Windows
which node  # Mac/Linux
```

### npm install이 느림

```bash
# 캐시 삭제
npm cache clean --force

# 또는 yarn 사용
npm install -g yarn
yarn
```

### VS Code에서 Prettier가 작동 안 함

1. `Ctrl+,` (설정 열기)
2. "Format On Save" 검색 → 체크
3. "Default Formatter" 검색 → Prettier 선택

### Create React App이 안 됨

```bash
# npx가 설치되어 있는지 확인
npm --version

# npx 버전 확인
npx --version

# Node.js 16 이상인지 확인
node --version

# 캐시 삭제 후 재시도
npx clear-npx-cache
npx create-react-app my-app
```

## 자주 묻는 질문

### Q: 설치가 너무 복잡한데 꼭 해야 하나요?

A: 네, 한 번만 설정하면 됩니다!
- jQuery는 간단하지만 큰 프로젝트에는 부적합
- React는 초기 설정이 복잡하지만 개발이 훨씬 편함
- 설정은 프로젝트당 한 번만

### Q: Node.js 버전은 어떤 걸 써야 하나요?

A: LTS (Long Term Support) 버전 추천
- v18.x 또는 v20.x
- 짝수 버전이 안정적

### Q: npm과 yarn 중 뭘 써야 하나요?

A: 초보자는 npm 추천
- Node.js와 함께 설치되어 별도 설정 불필요
- React 공식 문서도 npm 사용

### Q: VS Code 대신 다른 에디터를 써도 되나요?

A: 가능하지만 VS Code 강력 추천
- 무료
- React 확장 프로그램이 가장 많음
- 개발자들이 가장 많이 사용
- 설정이 쉬움

### Q: 이 모든 걸 다 외워야 하나요?

A: 아니요!
- 자주 쓰는 명령어만 외우면 됨 (5~10개)
- 나머지는 필요할 때 찾아보기
- 사용하다 보면 자연스럽게 익숙해짐

## 다음 단계

개발 환경 설정을 완료했다면:
- ✅ Node.js, npm, VS Code 모두 설치
- ✅ Create React App으로 첫 프로젝트 생성
- ✅ 개발 서버 실행 및 코드 수정 테스트
- 📖 다음 학습: Week 2 - React 기본 개념

## 추가 자료

### 공식 문서
- [Node.js 한국어 문서](https://nodejs.org/ko/)
- [npm 공식 문서](https://docs.npmjs.com/)
- [VS Code 한국어 문서](https://code.visualstudio.com/docs)
- [Create React App](https://create-react-app.dev/)

### 유용한 링크
- [VS Code 단축키 치트시트](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-windows.pdf)
- [npm 패키지 검색](https://www.npmjs.com/)
- [VS Code 확장 프로그램](https://marketplace.visualstudio.com/)

### 비디오 튜토리얼
- [생활코딩 - Node.js](https://opentutorials.org/course/3332)
- [코딩애플 - VS Code 사용법](https://codingapple.com/)

## 팁

### 1. 명령어를 외우지 마세요
자주 쓰는 명령어만 외우고, 나머지는 package.json의 scripts를 확인하세요.

```json
{
  "scripts": {
    "start": "react-scripts start",
    "build": "react-scripts build",
    "test": "react-scripts test"
  }
}
```

### 2. VS Code 단축키는 점진적으로
한 번에 모든 단축키를 외우려 하지 말고, 하나씩 익히세요.

**먼저 익힐 단축키:**
- `Ctrl+P`: 파일 열기
- `Ctrl+S`: 저장
- `Ctrl+\``: 터미널

### 3. 에러 메시지를 읽으세요
대부분의 에러 메시지는 해결 방법을 알려줍니다.
- 천천히 읽어보기
- 구글에 검색하기
- Stack Overflow 확인하기

### 4. Git 버전 관리 시작하기
개발 환경이 준비되면 Git도 배워보세요.

```bash
git init
git add .
git commit -m "first commit"
```

## 축하합니다!

React 개발 환경 설정을 완료했습니다! 🎉

이제 본격적으로 React를 배울 준비가 되었습니다.

---

**다음 주차 미리보기:**
Week 2에서는 React의 핵심 개념인 컴포넌트, JSX, Props, State를 배웁니다.

**준비되셨나요?** 메인 [README.md](../README.md)로 돌아가서 다음 단계를 확인하세요!
