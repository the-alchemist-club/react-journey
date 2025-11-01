# VS Code 설정 및 확장 프로그램

## jQuery 시대의 코드 에디터

jQuery를 사용할 때는 메모장, Sublime Text, Atom 등 간단한 에디터를 사용했습니다.

**필요한 기능:**
- 문법 하이라이팅
- 파일 탐색
- 검색/바꾸기

이 정도면 충분했습니다.

## 왜 VS Code가 필요할까?

React 개발은 더 복잡합니다:
- **자동 완성**: 수백 개의 컴포넌트와 함수
- **에러 표시**: 코드 작성 중 실시간 오류 확인
- **디버깅**: 브레이크포인트, 변수 추적
- **Git 통합**: 버전 관리
- **확장 프로그램**: React 개발에 최적화된 도구들

**VS Code는 무료이면서도 강력한 개발 도구입니다!**

## VS Code 설치

### 다운로드 및 설치

1. [VS Code 공식 사이트](https://code.visualstudio.com/) 접속
2. 운영체제에 맞는 버전 다운로드
   - Windows: `.exe` 파일
   - macOS: `.dmg` 파일
   - Linux: `.deb` 또는 `.rpm` 파일
3. 설치 파일 실행

### 첫 실행

1. VS Code 실행
2. 한국어 설정 (선택사항):
   - `Ctrl+Shift+P` (Mac: `Cmd+Shift+P`)
   - "Configure Display Language" 입력
   - "한국어" 선택 후 재시작

## 기본 사용법

### 프로젝트 열기

```bash
# 터미널에서 폴더 열기
code .

# 또는 VS Code에서
# File → Open Folder
```

### 주요 단축키

| 기능 | Windows/Linux | macOS |
|------|---------------|-------|
| 명령 팔레트 | `Ctrl+Shift+P` | `Cmd+Shift+P` |
| 파일 열기 | `Ctrl+P` | `Cmd+P` |
| 파일 저장 | `Ctrl+S` | `Cmd+S` |
| 전체 저장 | `Ctrl+K S` | `Cmd+K S` |
| 검색 | `Ctrl+F` | `Cmd+F` |
| 전체 검색 | `Ctrl+Shift+F` | `Cmd+Shift+F` |
| 터미널 열기 | `Ctrl+\`` | `Cmd+\`` |
| 사이드바 토글 | `Ctrl+B` | `Cmd+B` |
| 새 파일 | `Ctrl+N` | `Cmd+N` |

### 화면 구성

```
┌─────────────────────────────────────────┐
│  Activity Bar │  Side Bar  │  Editor   │
│               │            │           │
│  📁 Explorer  │  파일 목록  │  코드     │
│  🔍 Search    │            │           │
│  🔀 Git       │            │           │
│  🐛 Debug     │            │           │
│  🧩 Extensions│            │           │
└─────────────────────────────────────────┘
│          Terminal (터미널)              │
└─────────────────────────────────────────┘
```

## 필수 확장 프로그램

확장 프로그램 설치 방법:
1. 왼쪽 사이드바에서 확장 아이콘 클릭 (🧩)
2. 검색
3. "Install" 버튼 클릭

### 1. ES7+ React/Redux/React-Native snippets

**용도:** React 코드 스니펫(자동 완성 템플릿)

**설치:**
- 확장 탭에서 "ES7 React" 검색
- dsznajder.es7-react-js-snippets 설치

**사용 예:**
```javascript
// rfc 입력 후 Tab 키
import React from 'react'

function ComponentName() {
  return (
    <div>

    </div>
  )
}

export default ComponentName

// rafce 입력 후 Tab 키
const ComponentName = () => {
  return (
    <div>

    </div>
  )
}

export default ComponentName
```

**주요 스니펫:**
- `rfc`: React Function Component
- `rafce`: Arrow Function Component with Export
- `useState`: useState Hook
- `useEffect`: useEffect Hook

### 2. Prettier - Code formatter

**용도:** 코드 자동 포맷팅 (들여쓰기, 세미콜론, 따옴표 등)

**설치:**
- "Prettier" 검색
- esbenp.prettier-vscode 설치

**설정:**

`settings.json` 파일 수정:
```json
{
  "editor.defaultFormatter": "esbenp.prettier-vscode",
  "editor.formatOnSave": true,
  "[javascript]": {
    "editor.defaultFormatter": "esbenp.prettier-vscode"
  },
  "[javascriptreact]": {
    "editor.defaultFormatter": "esbenp.prettier-vscode"
  }
}
```

프로젝트 루트에 `.prettierrc` 파일 생성:
```json
{
  "semi": true,
  "singleQuote": true,
  "tabWidth": 2,
  "trailingComma": "es5",
  "printWidth": 80
}
```

### 3. ESLint

**용도:** 코드 품질 검사, 오류 감지

**설치:**
- "ESLint" 검색
- dbaeumer.vscode-eslint 설치

**프로젝트에 ESLint 설치:**
```bash
npm install -D eslint
npx eslint --init
```

### 4. Auto Rename Tag

**용도:** HTML/JSX 태그 자동 변경

**설치:**
- "Auto Rename Tag" 검색
- formulahendry.auto-rename-tag 설치

**예:**
```jsx
// <div>를 <section>으로 변경하면
<div>
  내용
</div>

// 자동으로 닫는 태그도 변경됨
<section>
  내용
</section>
```

### 5. Auto Close Tag

**용도:** 태그 자동 닫기

**설치:**
- "Auto Close Tag" 검색
- formulahendry.auto-close-tag 설치

### 6. Bracket Pair Colorizer 2 (또는 내장 기능)

**용도:** 괄호 쌍을 색으로 구분

**VS Code 1.60 이상:**
내장 기능 활성화 (설치 불필요)

```json
{
  "editor.bracketPairColorization.enabled": true,
  "editor.guides.bracketPairs": true
}
```

### 7. Path Intellisense

**용도:** 파일 경로 자동 완성

**설치:**
- "Path Intellisense" 검색
- christian-kohler.path-intellisense 설치

**예:**
```javascript
import Button from './components/Button'
//                 ← 자동 완성!
```

### 8. GitLens

**용도:** Git 기능 강화 (코드 작성자, 변경 이력 표시)

**설치:**
- "GitLens" 검색
- eamodio.gitlens 설치

### 9. Live Server (선택사항)

**용도:** HTML 파일 미리보기

**설치:**
- "Live Server" 검색
- ritwickdey.liveserver 설치

**사용:**
- HTML 파일에서 우클릭 → "Open with Live Server"

### 10. Material Icon Theme (선택사항)

**용도:** 파일 아이콘 예쁘게

**설치:**
- "Material Icon Theme" 검색
- pkief.material-icon-theme 설치

## VS Code 추천 설정

`Ctrl+,` (Mac: `Cmd+,`)로 설정 열기

### settings.json 직접 편집

`Ctrl+Shift+P` → "Preferences: Open Settings (JSON)"

```json
{
  // 에디터 기본 설정
  "editor.fontSize": 14,
  "editor.tabSize": 2,
  "editor.wordWrap": "on",
  "editor.minimap.enabled": false,
  "editor.lineNumbers": "on",

  // 자동 저장
  "files.autoSave": "afterDelay",
  "files.autoSaveDelay": 1000,

  // 포맷팅
  "editor.defaultFormatter": "esbenp.prettier-vscode",
  "editor.formatOnSave": true,
  "editor.formatOnPaste": true,

  // 괄호 색상
  "editor.bracketPairColorization.enabled": true,
  "editor.guides.bracketPairs": true,

  // 자동 완성
  "editor.suggestSelection": "first",
  "editor.quickSuggestions": {
    "strings": true
  },

  // 탭 완성
  "emmet.includeLanguages": {
    "javascript": "javascriptreact"
  },
  "emmet.triggerExpansionOnTab": true,

  // 터미널
  "terminal.integrated.fontSize": 13,

  // Git
  "git.autofetch": true,
  "git.confirmSync": false,

  // 파일 제외
  "files.exclude": {
    "**/.git": true,
    "**/node_modules": true,
    "**/.DS_Store": true
  },

  // 검색 제외
  "search.exclude": {
    "**/node_modules": true,
    "**/package-lock.json": true
  }
}
```

## Emmet 활용 (HTML/JSX 빠르게 작성)

Emmet은 VS Code에 내장된 HTML/CSS 자동 완성 도구입니다.

### 기본 사용법

```
div.container
→ <div className="container"></div>

ul>li*3
→ <ul>
    <li></li>
    <li></li>
    <li></li>
  </ul>

.header>.logo+nav>ul>li*3>a
→ <div className="header">
    <div className="logo"></div>
    <nav>
      <ul>
        <li><a href=""></a></li>
        <li><a href=""></a></li>
        <li><a href=""></a></li>
      </ul>
    </nav>
  </div>
```

## 유용한 팁

### 1. 멀티 커서

```
Alt+Click (Mac: Option+Click)
여러 위치에 동시 입력

Ctrl+D (Mac: Cmd+D)
같은 단어 선택 후 동시 수정

Alt+Shift+I (Mac: Option+Shift+I)
선택한 줄 끝에 커서 생성
```

### 2. 줄 이동

```
Alt+↑/↓ (Mac: Option+↑/↓)
현재 줄 위/아래로 이동

Shift+Alt+↑/↓ (Mac: Shift+Option+↑/↓)
현재 줄 복사
```

### 3. 코드 접기/펼치기

```
Ctrl+Shift+[ (Mac: Cmd+Option+[)
코드 블록 접기

Ctrl+Shift+] (Mac: Cmd+Option+])
코드 블록 펼치기

Ctrl+K Ctrl+0 (Mac: Cmd+K Cmd+0)
모두 접기

Ctrl+K Ctrl+J (Mac: Cmd+K Cmd+J)
모두 펼치기
```

### 4. 빠른 파일 이동

```
Ctrl+P (Mac: Cmd+P)
파일명으로 검색

Ctrl+Shift+O (Mac: Cmd+Shift+O)
현재 파일 내 심볼 검색 (함수, 변수 등)

Ctrl+T (Mac: Cmd+T)
프로젝트 전체 심볼 검색
```

### 5. 터미널 분할

```
Ctrl+Shift+5 (Mac: Cmd+D)
터미널 분할

Ctrl+` (Mac: Cmd+`)
터미널 토글
```

## 작업 공간 설정

프로젝트별로 다른 설정을 사용하려면 `.vscode` 폴더를 만듭니다.

### 프로젝트 루트에 .vscode 폴더 생성

```
my-project/
├── .vscode/
│   ├── settings.json      # 프로젝트 설정
│   ├── extensions.json    # 추천 확장 프로그램
│   └── launch.json        # 디버그 설정
├── src/
└── package.json
```

### .vscode/settings.json

```json
{
  "editor.tabSize": 2,
  "editor.formatOnSave": true,
  "files.exclude": {
    "**/node_modules": true,
    "**/.git": true
  }
}
```

### .vscode/extensions.json

팀원에게 확장 프로그램 추천:

```json
{
  "recommendations": [
    "dsznajder.es7-react-js-snippets",
    "esbenp.prettier-vscode",
    "dbaeumer.vscode-eslint",
    "formulahendry.auto-rename-tag"
  ]
}
```

## 자주 묻는 질문

### Q: Prettier와 ESLint가 충돌해요

A: `.eslintrc` 파일에 추가:

```json
{
  "extends": [
    "eslint:recommended",
    "prettier"
  ],
  "plugins": ["prettier"],
  "rules": {
    "prettier/prettier": "error"
  }
}
```

그리고 설치:
```bash
npm install -D eslint-config-prettier eslint-plugin-prettier
```

### Q: 한글이 깨져요

A: 파일 인코딩을 UTF-8로 변경:
- 우측 하단 인코딩 클릭
- "UTF-8로 저장"

### Q: 자동 완성이 안 나와요

A: `Ctrl+Space` (Mac: `Cmd+Space`)로 수동 트리거

### Q: 색상 테마를 바꾸고 싶어요

```
Ctrl+K Ctrl+T (Mac: Cmd+K Cmd+T)
→ 원하는 테마 선택

추천 테마:
- Dark+ (기본, 눈에 편함)
- One Dark Pro
- Dracula
- GitHub Theme
```

### Q: 폰트를 바꾸고 싶어요

```json
{
  "editor.fontFamily": "'Fira Code', 'Consolas', 'Courier New', monospace",
  "editor.fontLigatures": true,
  "editor.fontSize": 14
}
```

추천 폰트:
- Fira Code (무료, 리가쳐 지원)
- JetBrains Mono
- Cascadia Code

## 디버깅 설정 (고급)

`.vscode/launch.json`:

```json
{
  "version": "0.2.0",
  "configurations": [
    {
      "type": "chrome",
      "request": "launch",
      "name": "Launch Chrome",
      "url": "http://localhost:3000",
      "webRoot": "${workspaceFolder}/src"
    }
  ]
}
```

## 체크리스트

VS Code 설정을 완료했는지 확인하세요:

- [ ] VS Code 설치 완료
- [ ] 한국어 언어팩 설치 (선택)
- [ ] ES7 React Snippets 설치
- [ ] Prettier 설치 및 설정
- [ ] ESLint 설치
- [ ] Auto Rename Tag 설치
- [ ] settings.json 설정 완료
- [ ] Emmet 동작 확인
- [ ] 터미널에서 `code .` 명령어 테스트

## 다음 단계

VS Code 설정을 완료했다면:
1. ✅ 필수 확장 프로그램 설치
2. ✅ settings.json 설정
3. ✅ 단축키 익히기
4. 📖 다음: React 프로젝트 생성해보기

## 참고 자료

- [VS Code 공식 문서](https://code.visualstudio.com/docs)
- [VS Code 단축키 PDF](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-windows.pdf)
- [Emmet 치트시트](https://docs.emmet.io/cheat-sheet/)
- [Prettier 문서](https://prettier.io/docs/en/)

---

**축하합니다!** React 개발을 위한 완벽한 환경이 구축되었습니다!
