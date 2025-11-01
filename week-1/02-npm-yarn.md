# npm과 yarn 사용법

## jQuery 시대의 라이브러리 설치

jQuery를 사용할 때는 CDN 링크를 복사해서 붙여넣었습니다.

```html
<!-- jQuery -->
<script src="https://code.jquery.com/jquery-3.6.0.min.js"></script>

<!-- Bootstrap -->
<link href="https://cdn.jsdelivr.net/npm/bootstrap@5.1.3/dist/css/bootstrap.min.css" rel="stylesheet">

<!-- Slick Carousel -->
<script src="https://cdn.jsdelivr.net/npm/slick-carousel@1.8.1/slick/slick.min.js"></script>
```

**문제점:**
- 버전 관리가 어려움 (어떤 버전을 쓰는지 파악하기 힘듦)
- CDN이 죽으면 사이트가 안 됨
- 의존성 관리가 복잡함 (A 라이브러리가 B를 필요로 하는 경우)
- 로컬에서 개발할 때 인터넷 필요

## 패키지 관리자란?

npm과 yarn은 JavaScript 패키지(라이브러리)를 설치하고 관리하는 도구입니다.

**장점:**
- 명령어 하나로 설치 가능
- 버전을 정확히 관리
- 의존성 자동 해결
- 오프라인에서도 작업 가능
- 모든 패키지를 프로젝트 폴더에 저장

## npm (Node Package Manager)

Node.js를 설치하면 자동으로 같이 설치됩니다.

### 기본 개념

#### package.json

프로젝트의 설정 파일입니다. 어떤 패키지를 사용하는지 기록합니다.

```json
{
  "name": "my-project",
  "version": "1.0.0",
  "dependencies": {
    "react": "^18.2.0",
    "react-dom": "^18.2.0"
  }
}
```

#### node_modules

설치된 패키지들이 저장되는 폴더입니다.
- Git에 올리지 않음 (용량이 매우 큼)
- `package.json`만 있으면 언제든 재설치 가능

### npm 기본 명령어

#### 1. 프로젝트 초기화

```bash
# 새 프로젝트 시작
npm init

# 질문 건너뛰고 기본값으로 생성
npm init -y
```

`package.json` 파일이 생성됩니다.

#### 2. 패키지 설치

```bash
# React 설치
npm install react

# 여러 개 동시 설치
npm install react react-dom

# 축약형 (i = install)
npm i react

# 특정 버전 설치
npm install react@18.2.0

# 개발용 패키지 설치 (빌드 후에는 필요 없음)
npm install --save-dev eslint
npm install -D eslint  # 축약형
```

#### 3. 패키지 제거

```bash
# 패키지 삭제
npm uninstall react

# 축약형
npm un react
```

#### 4. package.json의 모든 패키지 설치

```bash
# GitHub에서 프로젝트를 받았거나
# node_modules를 삭제했을 때
npm install

# 또는
npm i
```

#### 5. 패키지 업데이트

```bash
# 모든 패키지 업데이트
npm update

# 특정 패키지 업데이트
npm update react
```

#### 6. 설치된 패키지 확인

```bash
# 설치된 패키지 목록
npm list

# 최상위 패키지만 보기
npm list --depth=0

# 전역으로 설치된 패키지
npm list -g --depth=0
```

### 전역 설치 vs 로컬 설치

#### 로컬 설치 (기본)

```bash
npm install react
```

- 현재 프로젝트의 `node_modules`에 설치
- 해당 프로젝트에서만 사용 가능
- **대부분의 경우 로컬 설치 사용**

#### 전역 설치

```bash
npm install -g create-react-app
```

- 컴퓨터 전체에서 사용 가능
- 주로 CLI 도구 설치할 때 사용
- 예: create-react-app, nodemon, typescript

### npm scripts

`package.json`에 자주 사용하는 명령어를 등록할 수 있습니다.

```json
{
  "name": "my-project",
  "scripts": {
    "start": "react-scripts start",
    "build": "react-scripts build",
    "test": "react-scripts test"
  }
}
```

실행:
```bash
npm run start
npm run build
npm run test

# start는 run 생략 가능
npm start
```

## yarn

npm의 대안으로, Facebook에서 만든 패키지 관리자입니다.

### yarn 설치

```bash
# npm으로 yarn 설치
npm install -g yarn

# 버전 확인
yarn --version
```

### npm vs yarn 명령어 비교

| 작업 | npm | yarn |
|------|-----|------|
| 프로젝트 초기화 | `npm init -y` | `yarn init -y` |
| 패키지 설치 | `npm install` | `yarn` 또는 `yarn install` |
| 패키지 추가 | `npm install react` | `yarn add react` |
| 개발 패키지 추가 | `npm install -D eslint` | `yarn add -D eslint` |
| 패키지 제거 | `npm uninstall react` | `yarn remove react` |
| 전역 설치 | `npm install -g pkg` | `yarn global add pkg` |
| 스크립트 실행 | `npm run dev` | `yarn dev` |
| 패키지 업데이트 | `npm update` | `yarn upgrade` |

### yarn의 장점

1. **속도**: 병렬 설치로 npm보다 빠름
2. **yarn.lock**: 정확한 버전 관리
3. **간결한 명령어**: `yarn add` vs `npm install`

### 어떤 것을 사용해야 할까?

**초보자:**
- npm 추천 (Node.js와 함께 설치되므로 별도 설정 불필요)

**팀 프로젝트:**
- 팀에서 사용하는 것과 동일하게 사용
- `package-lock.json` 있으면 npm
- `yarn.lock` 있으면 yarn
- **둘을 섞어 쓰지 말 것!**

## 실전 예제

### React 프로젝트 생성

```bash
# 1. 프로젝트 폴더 생성
mkdir my-react-app
cd my-react-app

# 2. package.json 생성
npm init -y

# 3. React 설치
npm install react react-dom

# 4. 개발 도구 설치
npm install -D webpack webpack-cli webpack-dev-server

# 5. 설치 확인
npm list --depth=0
```

### Create React App 사용 (추천)

```bash
# npx로 즉시 실행 (설치 불필요)
npx create-react-app my-app

cd my-app
npm start
```

### package.json 예제

```json
{
  "name": "my-react-app",
  "version": "1.0.0",
  "description": "나의 첫 React 앱",
  "scripts": {
    "start": "react-scripts start",
    "build": "react-scripts build",
    "test": "react-scripts test",
    "eject": "react-scripts eject"
  },
  "dependencies": {
    "react": "^18.2.0",
    "react-dom": "^18.2.0",
    "react-router-dom": "^6.8.0"
  },
  "devDependencies": {
    "eslint": "^8.36.0",
    "prettier": "^2.8.4"
  }
}
```

**dependencies vs devDependencies:**
- `dependencies`: 앱 실행에 필요 (React, Router 등)
- `devDependencies`: 개발할 때만 필요 (ESLint, Prettier 등)

## 버전 표기법 (Semantic Versioning)

```json
{
  "dependencies": {
    "react": "^18.2.0"
  }
}
```

**버전 번호:** MAJOR.MINOR.PATCH (18.2.0)
- **MAJOR (18)**: 큰 변경, 호환성 깨질 수 있음
- **MINOR (2)**: 새 기능 추가, 하위 호환
- **PATCH (0)**: 버그 수정

**기호:**
- `^18.2.0`: 18.x.x의 최신 버전 (18.2.1, 18.3.0 등)
- `~18.2.0`: 18.2.x의 최신 버전 (18.2.1, 18.2.2 등)
- `18.2.0`: 정확히 이 버전만
- `*` 또는 `latest`: 최신 버전

## 자주 사용하는 명령어 정리

```bash
# 프로젝트 시작
npm init -y

# 패키지 설치
npm install react
npm i react              # 축약형
npm i react react-dom    # 여러 개

# 개발 도구 설치
npm install -D eslint

# 모든 패키지 설치 (package.json 기준)
npm install

# 패키지 제거
npm uninstall react

# 스크립트 실행
npm start
npm run build
npm test

# 패키지 검색
npm search react-router

# 패키지 정보
npm info react

# 캐시 삭제 (문제 생길 때)
npm cache clean --force
```

## 문제 해결

### 설치가 안 될 때

```bash
# 1. 캐시 삭제
npm cache clean --force

# 2. node_modules 삭제 후 재설치
rm -rf node_modules
npm install

# Windows에서는
rmdir /s node_modules
npm install
```

### 권한 오류 (Permission denied)

**macOS/Linux:**
```bash
# sudo 사용하지 말고, npm 경로 변경
mkdir ~/.npm-global
npm config set prefix '~/.npm-global'
echo 'export PATH=~/.npm-global/bin:$PATH' >> ~/.zshrc
source ~/.zshrc
```

### 버전 충돌

```bash
# package-lock.json 삭제 후 재설치
rm package-lock.json
rm -rf node_modules
npm install
```

### npm이 느릴 때

```bash
# 레지스트리를 한국 미러로 변경
npm config set registry https://registry.npmjs.org/

# 또는 yarn 사용
npm install -g yarn
```

## 유용한 팁

### 1. package.json 자동 업데이트

```bash
# 최신 버전 확인
npm outdated

# 안전하게 업데이트 (minor, patch만)
npm update

# 메이저 버전까지 업데이트 (주의!)
npx npm-check-updates -u
npm install
```

### 2. npx 사용하기

패키지를 전역 설치 없이 바로 실행합니다.

```bash
# 전역 설치 대신
npx create-react-app my-app

# 일회성 실행
npx cowsay "Hello!"
```

### 3. 보안 취약점 확인

```bash
# 취약점 검사
npm audit

# 자동 수정
npm audit fix
```

### 4. .npmrc 설정

프로젝트 루트에 `.npmrc` 파일 생성:

```
# 정확한 버전 저장 (^ 제거)
save-exact=true

# 진행 상황 표시 안 함 (로그 깔끔)
progress=false
```

## 자주 묻는 질문

### Q: npm과 yarn 중 뭘 써야 하나요?

A: 초보자는 **npm**을 추천합니다.
- Node.js와 함께 설치되어 추가 설정 불필요
- 프로젝트에 이미 `yarn.lock`이 있으면 yarn 사용

### Q: node_modules를 Git에 올려야 하나요?

A: **절대 안 됩니다!**
- `.gitignore`에 `node_modules/` 추가
- `package.json`과 `package-lock.json`만 올림
- 다른 사람은 `npm install`로 설치

### Q: package-lock.json은 뭔가요?

A: 정확한 버전을 기록하는 파일입니다.
- 자동 생성됨
- Git에 포함해야 함
- 삭제하지 말 것 (문제 생기면 재생성)

### Q: devDependencies는 꼭 구분해야 하나요?

A: 권장하지만 필수는 아닙니다.
- 빌드 시 용량 최적화에 도움
- 초보자는 모두 `dependencies`에 넣어도 OK

### Q: 전역 설치한 패키지는 어디에 있나요?

```bash
# 전역 패키지 경로 확인
npm root -g

# Windows: C:\Users\사용자\AppData\Roaming\npm\node_modules
# macOS: /usr/local/lib/node_modules
```

## 다음 단계

npm/yarn 사용법을 익혔다면:
1. ✅ `npm init -y`로 프로젝트 생성
2. ✅ `npm install react`로 패키지 설치
3. ✅ `package.json` 이해
4. 📖 다음 학습: [VS Code 설정](./03-vscode-setup.md)

## 참고 자료

- [npm 공식 문서](https://docs.npmjs.com/)
- [yarn 공식 문서](https://yarnpkg.com/)
- [npmjs.com](https://www.npmjs.com/) - 패키지 검색
- [Semantic Versioning](https://semver.org/lang/ko/)

---

**축하합니다!** 이제 패키지 관리자를 사용할 수 있습니다. React 개발에 한 걸음 더 가까워졌습니다!
