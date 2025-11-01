# Node.js 설치

## jQuery 시대에는 필요 없었던 것

jQuery를 사용할 때는 HTML 파일에 `<script>` 태그만 추가하면 됐습니다.

```html
<!DOCTYPE html>
<html>
<head>
  <title>My Site</title>
</head>
<body>
  <h1>Hello World</h1>

  <!-- jQuery CDN -->
  <script src="https://code.jquery.com/jquery-3.6.0.min.js"></script>
  <!-- 내 스크립트 -->
  <script src="app.js"></script>
</body>
</html>
```

브라우저만 있으면 끝! 별도의 설치나 설정이 필요 없었습니다.

## 왜 Node.js가 필요할까?

React를 개발하려면 다음과 같은 것들이 필요합니다:

1. **패키지 관리자** (npm/yarn)
   - React 라이브러리 설치
   - 수백 개의 다른 라이브러리 관리

2. **빌드 도구**
   - JSX를 일반 JavaScript로 변환
   - 최신 문법을 구형 브라우저용으로 변환
   - 파일 번들링 (여러 파일을 하나로 합치기)

3. **개발 서버**
   - 코드를 수정하면 자동으로 새로고침
   - 빠른 개발 환경 제공

**Node.js는 이 모든 것을 가능하게 하는 JavaScript 실행 환경입니다.**

## Node.js란?

> Node.js는 브라우저 밖에서 JavaScript를 실행할 수 있게 해주는 런타임 환경입니다.

**간단히 말하면:**
- 원래 JavaScript는 브라우저에서만 실행됨
- Node.js를 설치하면 컴퓨터에서도 JavaScript 실행 가능
- 이를 통해 개발 도구, 서버 등을 만들 수 있음

**React 개발에서는:**
- Node.js 자체로 코드를 실행하는 것이 아니라
- Node.js에 포함된 npm(패키지 관리자)과 빌드 도구를 사용하기 위함

## 설치 방법

### Windows

#### 방법 1: 공식 사이트에서 다운로드 (추천)

1. [Node.js 공식 사이트](https://nodejs.org/) 접속
2. **LTS (Long Term Support)** 버전 다운로드
   - LTS는 안정적이고 장기 지원되는 버전
   - 초보자에게 권장
3. 다운로드한 `.msi` 파일 실행
4. 설치 마법사 따라가기
   - "Next" 버튼 계속 클릭
   - 기본 설정 그대로 사용
   - "Install" 클릭

#### 설치 확인

명령 프롬프트(cmd) 또는 PowerShell을 열고:

```bash
node --version
# 출력 예: v20.10.0

npm --version
# 출력 예: 10.2.3
```

버전 번호가 나오면 설치 성공!

### macOS

#### 방법 1: 공식 사이트에서 다운로드

1. [Node.js 공식 사이트](https://nodejs.org/) 접속
2. **LTS** 버전 다운로드
3. `.pkg` 파일 실행 후 설치

#### 방법 2: Homebrew 사용 (추천)

Homebrew가 설치되어 있다면:

```bash
# Node.js 설치
brew install node

# 설치 확인
node --version
npm --version
```

### Linux (Ubuntu/Debian)

```bash
# NodeSource 저장소 추가
curl -fsSL https://deb.nodesource.com/setup_lts.x | sudo -E bash -

# Node.js 설치
sudo apt-get install -y nodejs

# 설치 확인
node --version
npm --version
```

## 설치 후 첫 번째 테스트

### 1. JavaScript 파일 실행해보기

`test.js` 파일을 만들고:

```javascript
// test.js
console.log('Hello, Node.js!');

const sum = (a, b) => a + b;
console.log('5 + 3 =', sum(5, 3));

console.log('현재 Node.js 버전:', process.version);
```

터미널에서 실행:

```bash
node test.js
```

출력:
```
Hello, Node.js!
5 + 3 = 8
현재 Node.js 버전: v20.10.0
```

### 2. npm이 잘 작동하는지 확인

```bash
# npm 도움말
npm help

# npm으로 간단한 패키지 정보 보기
npm view react version
# 출력 예: 18.2.0
```

## 버전 관리 도구 (선택사항)

여러 프로젝트를 작업하다 보면 Node.js 버전을 바꿔야 할 때가 있습니다.

### nvm (Node Version Manager)

여러 Node.js 버전을 쉽게 전환할 수 있는 도구입니다.

#### Windows: nvm-windows

1. [nvm-windows 릴리스](https://github.com/coreybutler/nvm-windows/releases) 페이지 접속
2. `nvm-setup.exe` 다운로드 및 설치
3. 사용법:

```bash
# 설치 가능한 버전 목록
nvm list available

# 특정 버전 설치
nvm install 20.10.0

# 설치된 버전 확인
nvm list

# 버전 전환
nvm use 20.10.0
```

#### macOS/Linux: nvm

```bash
# nvm 설치
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.0/install.sh | bash

# 터미널 재시작 후 사용
nvm install --lts
nvm use --lts
```

## 자주 묻는 질문

### Q: LTS와 Current 버전 중 어떤 것을 설치해야 하나요?

A: **LTS (Long Term Support)** 버전을 설치하세요.
- LTS: 안정적이고 장기 지원, 초보자에게 권장
- Current: 최신 기능이 있지만 불안정할 수 있음

### Q: Node.js 버전이 너무 많은데 어떤 버전을 써야 하나요?

A: **짝수 버전의 LTS**를 사용하세요 (예: v18.x, v20.x)
- 홀수 버전은 실험적 기능이 많음
- 짝수 버전이 안정적

### Q: 이미 설치되어 있는데 버전이 낮으면 어떻게 하나요?

A: Node.js를 다시 설치하거나 nvm을 사용하세요.

```bash
# 현재 버전 확인
node --version

# v14 이하면 업데이트 권장
# v16 이상이면 대부분의 React 프로젝트 실행 가능
```

### Q: npm은 뭔가요?

A: Node.js를 설치하면 npm(Node Package Manager)도 자동으로 설치됩니다.
- npm은 JavaScript 패키지(라이브러리)를 설치하고 관리하는 도구
- React를 설치할 때도 npm을 사용
- 다음 학습 자료에서 자세히 배웁니다

### Q: Node.js를 설치하면 브라우저의 JavaScript에 영향을 주나요?

A: 아니요, 전혀 영향을 주지 않습니다.
- Node.js는 컴퓨터에서 실행되는 별도의 환경
- 브라우저의 JavaScript와는 독립적

### Q: React 앱을 배포할 때도 Node.js가 필요한가요?

A: 개발할 때만 필요합니다.
- 개발: Node.js로 빌드 → 결과물 생성
- 배포: 빌드된 HTML/CSS/JS 파일만 서버에 업로드
- 사용자는 브라우저에서 접속 (Node.js 불필요)

## 문제 해결

### Windows: 'node' 명령어를 찾을 수 없습니다

**원인:** 환경 변수 PATH에 Node.js가 추가되지 않음

**해결:**
1. 컴퓨터 재시작
2. 여전히 안 되면 수동으로 PATH 추가:
   - "시스템 환경 변수 편집" 검색
   - "환경 변수" 클릭
   - Path 변수에 `C:\Program Files\nodejs\` 추가

### macOS: Permission denied

**원인:** npm 전역 패키지 설치 권한 문제

**해결:**
```bash
# npm 전역 패키지 디렉토리 변경
mkdir ~/.npm-global
npm config set prefix '~/.npm-global'

# ~/.zshrc 또는 ~/.bash_profile에 추가
echo 'export PATH=~/.npm-global/bin:$PATH' >> ~/.zshrc
source ~/.zshrc
```

### 설치는 됐는데 버전이 안 나와요

```bash
# Node.js 경로 확인 (Windows)
where node

# Node.js 경로 확인 (macOS/Linux)
which node

# 경로가 나오는데도 버전이 안 나오면
# 터미널을 완전히 종료 후 다시 실행
```

## 다음 단계

Node.js 설치를 완료했다면:
1. ✅ `node --version`으로 확인
2. ✅ `npm --version`으로 확인
3. 📖 다음 학습: [npm과 yarn 사용법](./02-npm-yarn.md)

## 참고 자료

- [Node.js 공식 사이트](https://nodejs.org/)
- [Node.js 한국어 가이드](https://nodejs.org/ko/docs/)
- [nvm GitHub](https://github.com/nvm-sh/nvm)
- [nvm-windows GitHub](https://github.com/coreybutler/nvm-windows)

---

**축하합니다!** Node.js 설치를 완료했습니다. 이제 React 개발 환경을 구축할 준비가 되었습니다!
