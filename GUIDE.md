# 김비서 프로젝트 제작 가이드
## 다음 프로젝트 참고용 상세 기록

작성일: 2026년 5월 14일  
프로젝트명: 김비서 (AI 업무 관리 보조자)

---

## 📋 목차

1. [프로젝트 개요](#프로젝트-개요)
2. [단계별 제작 과정](#단계별-제작-과정)
3. [파일 구조](#파일-구조)
4. [디자인 시스템](#디자인-시스템)
5. [핵심 기술 스택](#핵심-기술-스택)
6. [GitHub 연동](#github-연동)
7. [버전 관리](#버전-관리)
8. [주의사항 및 팁](#주의사항-및-팁)

---

## 프로젝트 개요

### 목적
데이터를 읽고 분석하여 웹 대시보드로 시각화하고, 
AI 브리핑 기능을 제공하는 업무 관리 시스템

### 핵심 기능
- ✅ 프리미엄 웹 인터페이스 (글래스모피즘 디자인)
- ✅ 다크/라이트 모드
- ✅ 데이터 시각화 (표, 차트, 프로그레스 바)
- ✅ 페이지 간 네비게이션
- ✅ AI 브리핑 (/슬래시 명령어)
- ✅ GitHub 버전 관리

---

## 단계별 제작 과정

### STEP 1️⃣: 데이터 파악 (5분)

**목표**: 작업할 데이터 확인

```bash
# 폴더 구조 확인
Get-ChildItem -Path . | Format-Table Name, Mode, Length -AutoSize

# 데이터 폴더 내용 확인
Get-ChildItem -Path ".\폴더명" -Recurse -Depth 2
```

**우리의 경우:**
- 📁 김비서-데이터/
  - 매출데이터.csv (32개 거래, 79.6M 매출)
  - 업무목록.csv (11개 업무, 우선순위 표시)
  - 주간일정.txt (월~금 일정)
  - 프로젝트현황.csv (6개 프로젝트, 진행률)
  - 회의록.txt (회의내용, 액션아이템)

**시작 전 수집할 정보:**
```
□ 데이터 파일 형식 (CSV, TXT, JSON 등)
□ 데이터에 포함된 정보 (날짜, 우선순위, 상태 등)
□ 표시할 지표 (합계, 개수, 진행률 등)
□ 타겟 사용자 (경영진, 팀장, 일반 직원)
```

---

### STEP 2️⃣: 메인 페이지 만들기 (20분)

**목표**: 프로젝트 소개 페이지

**파일**: `index.html`

**구성 요소:**
1. **헤더**
   - 프로젝트 제목
   - 한 줄 설명

2. **설명 섹션**
   - 3가지 핵심 기능 (아이콘 + 텍스트)
   - 각 기능당 2-3줄 설명

3. **CTA 버튼**
   - 대시보드로 이동하는 버튼

**CSS 핵심:**
```css
/* 컨테이너 중앙 정렬 */
body {
    display: flex;
    align-items: center;
    justify-content: center;
}

/* 그라디언트 배경 */
background: linear-gradient(135deg, #f5f7ff 0%, #ffffff 50%, #f0f4ff 100%);

/* 글래스모피즘 */
backdrop-filter: blur(20px);
background: rgba(255, 255, 255, 0.8);
border: 1px solid rgba(255, 255, 255, 0.2);
```

**JavaScript (다크모드):**
```javascript
// localStorage에 테마 저장
localStorage.setItem('theme', 'dark');

// 페이지 로드 시 저장된 테마 적용
const savedTheme = localStorage.getItem('theme') || 'light';
if (savedTheme === 'dark') {
    document.body.classList.add('dark-mode');
}
```

---

### STEP 3️⃣: 대시보드 페이지 만들기 (45분)

**목표**: 데이터를 시각화하는 메인 페이지

**파일**: `dashboard.html`

**4개 섹션 구성:**

#### 섹션 1: 할 일 목록
```html
<div class="todo-item priority-high">
    <input type="checkbox">
    <span>업무 내용</span>
    <span class="priority">높음</span>
</div>
```

**우선순위 색상:**
- 높음: #dc2626 (빨강)
- 보통: #f59e0b (주황)
- 낮음: #3b82f6 (파랑)

#### 섹션 2: 주간 일정 (표)
```html
<table class="schedule-table">
    <tr>
        <th>요일</th>
        <th>일정</th>
    </tr>
    <!-- 월~금 반복 -->
</table>
```

#### 섹션 3: 프로젝트 진행률
```html
<div class="project-item">
    <div class="project-header">
        <span>프로젝트명</span>
        <span>65%</span>
    </div>
    <div class="progress-bar">
        <div class="progress-fill" style="width: 65%"></div>
    </div>
</div>
```

#### 섹션 4: 매출 요약 (카드)
```html
<div class="sales-card">
    <div class="sales-label">총 매출액</div>
    <div class="sales-value">79.6M</div>
    <div class="sales-unit">원</div>
</div>
```

**그리드 레이아웃:**
```css
.dashboard {
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 24px;
}

/* 풀 너비 (2개 열) */
.full-width {
    grid-column: 1 / -1;
}

/* 모바일 반응형 */
@media (max-width: 768px) {
    .dashboard {
        grid-template-columns: 1fr;
    }
}
```

---

### STEP 4️⃣: 보고서/분석 페이지 만들기 (30분)

**목표**: 데이터 분석 또는 웹사이트 리뷰 페이지

**파일들:**
- `meeting-result.html` - 회의록 분석
- `report.html` - 웹사이트 분석

**구성 요소:**
1. **헤더 섹션**
   - 제목
   - 메타정보 (날짜, 참석자 등)

2. **섹션별 카드**
   ```html
   <div class="card">
       <h2>섹션 제목</h2>
       <ul>
           <li>항목 1</li>
           <li>항목 2</li>
       </ul>
   </div>
   ```

3. **테이블**
   ```html
   <table>
       <thead>
           <tr>
               <th>열1</th>
               <th>열2</th>
           </tr>
       </thead>
       <tbody>
           <!-- 데이터 행 -->
       </tbody>
   </table>
   ```

**인쇄 최적화:**
```css
@media print {
    .nav-buttons, .theme-toggle {
        display: none;
    }
    .card {
        page-break-inside: avoid;
    }
}
```

---

### STEP 5️⃣: 다이어그램/시각화 (15분)

**목표**: 프로세스/플로우 표시

**방법 1: SVG로 직접 그리기**
```html
<svg viewBox="0 0 800 200">
    <!-- 도형들 -->
    <rect x="20" y="50" width="110" height="100" rx="12" fill="#B3D9FF"/>
    <!-- 화살표 -->
    <line x1="130" y1="100" x2="150" y2="100" class="arrow"/>
</svg>
```

**방법 2: HTML 래퍼로 변환**
SVG 파일을 diagram.html로 변환하여 네비게이션 추가

---

### STEP 6️⃣: 글래스모피즘 디자인 적용 (60분)

**목표**: 모든 페이지에 프리미엄 디자인 통일

**CSS 변수 설정:**
```css
:root {
    /* 라이트 모드 */
    --bg-primary: #ffffff;
    --card-bg: rgba(255, 255, 255, 0.8);
    --card-border: rgba(255, 255, 255, 0.2);
    --text-primary: #1a1a1a;
    --accent: #3b82f6;
    --shadow: 0 8px 32px rgba(31, 38, 135, 0.15);
    --gradient-bg: linear-gradient(135deg, #f5f7ff 0%, #ffffff 50%, #f0f4ff 100%);
}

body.dark-mode {
    /* 다크 모드 */
    --bg-primary: #0f0f23;
    --card-bg: rgba(255, 255, 255, 0.05);
    --card-border: rgba(255, 255, 255, 0.1);
    --text-primary: #e0e0e0;
    --shadow: 0 8px 32px rgba(0, 0, 0, 0.3);
    --gradient-bg: linear-gradient(135deg, #0f0f23 0%, #1a1a3e 50%, #16213e 100%);
}
```

**핵심 CSS 패턴:**

1. **글래스모피즘 카드**
```css
.card {
    background: var(--card-bg);
    border: 1px solid var(--card-border);
    backdrop-filter: blur(20px);
    border-radius: 16px;
    box-shadow: var(--shadow);
}
```

2. **그라디언트 텍스트**
```css
h1 {
    background: linear-gradient(135deg, var(--accent) 0%, var(--accent-dark) 100%);
    -webkit-background-clip: text;
    -webkit-text-fill-color: transparent;
    background-clip: text;
}
```

3. **호버 효과**
```css
.card:hover {
    transform: translateY(-4px);
    box-shadow: 0 12px 40px rgba(59, 130, 246, 0.2);
}
```

4. **배경 애니메이션**
```css
body::before {
    content: '';
    position: fixed;
    background: radial-gradient(circle, rgba(59, 130, 246, 0.1) 0%, transparent 70%);
    animation: float 15s ease-in-out infinite;
}

@keyframes float {
    0%, 100% { transform: translate(0, 0) rotate(0deg); }
    50% { transform: translate(30px, 30px) rotate(180deg); }
}
```

---

### STEP 7️⃣: 다크/라이트 모드 추가 (20분)

**목표**: 모든 페이지에서 테마 전환 가능

**HTML:**
```html
<button class="theme-toggle" id="themeToggle">
    <span id="themeIcon">🌙</span>
</button>
```

**JavaScript (모든 페이지에 동일):**
```javascript
const themeToggle = document.getElementById('themeToggle');
const themeIcon = document.getElementById('themeIcon');
const savedTheme = localStorage.getItem('theme') || 'light';

function setTheme(theme) {
    if (theme === 'dark') {
        document.body.classList.add('dark-mode');
        themeIcon.textContent = '☀️';
        localStorage.setItem('theme', 'dark');
    } else {
        document.body.classList.remove('dark-mode');
        themeIcon.textContent = '🌙';
        localStorage.setItem('theme', 'light');
    }
}

setTheme(savedTheme);

themeToggle.addEventListener('click', () => {
    const currentTheme = document.body.classList.contains('dark-mode') ? 'dark' : 'light';
    setTheme(currentTheme === 'dark' ? 'light' : 'dark');
});
```

---

### STEP 8️⃣: 네비게이션 메뉴 추가 (25분)

**목표**: 모든 페이지 간 쉬운 이동

**HTML 구조:**
```html
<div class="nav-menu">
    <a href="index.html" class="nav-item active">🏠 메인</a>
    <a href="dashboard.html" class="nav-item">📊 대시보드</a>
    <a href="meeting-result.html" class="nav-item">📋 회의록</a>
    <a href="diagram.html" class="nav-item">📌 프로세스</a>
    <a href="report.html" class="nav-item">🔍 분석</a>
</div>
```

**CSS:**
```css
.nav-menu {
    display: flex;
    gap: 12px;
    background: var(--card-bg);
    border: 1px solid var(--card-border);
    backdrop-filter: blur(20px);
    border-radius: 12px;
    padding: 8px;
    flex-wrap: wrap;
    justify-content: center;  /* ★ 가운데 정렬 */
    align-items: center;
}

.nav-item {
    padding: 10px 18px;
    border-radius: 8px;
    color: var(--text-secondary);
    text-decoration: none;
    transition: all 0.3s ease;
    white-space: nowrap;
}

.nav-item.active {
    background: linear-gradient(135deg, var(--accent) 0%, var(--accent-dark) 100%);
    color: white;
    box-shadow: 0 4px 12px rgba(59, 130, 246, 0.3);
}
```

**JavaScript (자동 활성화):**
```javascript
const navItems = document.querySelectorAll('.nav-item');
const currentPath = window.location.pathname;
const currentFile = currentPath.split('/').pop() || 'index.html';

navItems.forEach(item => {
    item.classList.remove('active');
    const href = item.getAttribute('href');
    const itemFile = href.split('/').pop();
    if (itemFile === currentFile) {
        item.classList.add('active');
    }
});
```

---

### STEP 9️⃣: 슬래시 명령어 만들기 (30분)

**목표**: `/김비서` 같은 커스텀 명령어

**파일 위치:**
`C:\Users\[사용자명]\AppData\Roaming\Claude\skills\[스킬명]\SKILL.md`

**내용:**
```markdown
---
name: 김비서
description: |
  업무 상황을 한눈에 파악하는 AI 비서
  /김비서 를 입력하면 오늘의 할 일을 우선순위별로 정리합니다
---

# [스킬명]

[상세 지시사항]
```

**구성 요소:**
1. 데이터 폴더의 모든 파일 읽기
2. 현재 날짜 기준 필터링
3. 우선순위별 정렬
4. 브리핑 포맷 생성

---

### STEP 🔟: 환경설정 파일 만들기 (10분)

**파일**: `.env.local`

**내용:**
```
# GitHub Configuration
GITHUB_TOKEN=ghp_xxxxxxxxxxxxx
GITHUB_USERNAME=사용자명

# API 설정 (필요시)
# API_KEY=
# API_SECRET=
```

**보안 주의:**
- 절대 공개 저장소에 올리지 않기
- `.gitignore`에 포함시키기

---

### STEP 1️⃣1️⃣: GitHub 연동 (30분)

**1단계: Git 초기화**
```bash
cd 프로젝트폴더
git init
git config user.email "이메일"
git config user.name "이름"
```

**2단계: 원격 저장소 연결**
```bash
git remote add origin https://github.com/사용자/저장소.git
git branch -M main
```

**3단계: 첫 커밋**
```bash
git add .
git commit -m "초기 커밋: 프로젝트명 - 설명

포함 사항:
- 메인페이지
- 대시보드
- 보고서

Co-Authored-By: Claude Haiku 4.5 <noreply@anthropic.com>"
```

**4단계: 푸시**
```bash
git push -u origin main
```

---

### STEP 1️⃣2️⃣: 보안 설정 (15분)

**파일**: `.gitignore`

**내용:**
```
# 환경설정
.env
.env.local
.env.*.local

# IDE
.vscode/
.idea/

# 임시 파일
.tmp/
temp/
*.log
```

**GitHub 토큰 노출 시:**
1. GitHub에서 "I'll fix it later" 선택
2. 토큰 재생성 (권장)
3. 재푸시

---

### STEP 1️⃣3️⃣: 버전 관리 시스템 (15분)

**파일**: `VERSION.md`

**내용:**
```markdown
# 프로젝트명 버전

## 현재 버전: v0.2.0

### 버전 히스토리

#### v0.2.0 (날짜)
- 기능 1
- 기능 2

#### v0.1.0 (날짜)
- 기능 1
- 기능 2
```

**Git 태그 만들기:**
```bash
git tag -a v0.2.0 -m "메시지"
git push origin v0.2.0
```

---

## 파일 구조

```
프로젝트폴더/
├── 📄 index.html           ← 메인 페이지
├── 📊 dashboard.html       ← 대시보드 (4섹션)
├── 📋 meeting-result.html  ← 회의록 분석
├── 🔍 report.html          ← 웹사이트 분석
├── 📌 diagram.html         ← 프로세스 다이어그램
├── 🎨 diagram.svg          ← SVG 다이어그램
├── 📁 김비서-데이터/        ← 원본 데이터
│   ├── 매출데이터.csv
│   ├── 업무목록.csv
│   ├── 주간일정.txt
│   ├── 프로젝트현황.csv
│   └── 회의록.txt
├── 🔐 .env.local           ← 환경설정 (미포함)
├── 📝 .gitignore           ← Git 제외 파일
├── 📖 VERSION.md           ← 버전 히스토리
├── 📋 GUIDE.md             ← 이 파일
└── .git/                   ← Git 저장소

```

---

## 디자인 시스템

### 색상 팔레트

**라이트 모드:**
- 배경: #ffffff
- 카드: rgba(255, 255, 255, 0.8)
- 텍스트: #1a1a1a
- 강조: #3b82f6 (파랑)

**다크 모드:**
- 배경: #0f0f23 (검정)
- 카드: rgba(255, 255, 255, 0.05)
- 텍스트: #e0e0e0 (밝은 회색)
- 강조: #3b82f6 (파랑)

**의미별 색상:**
- 높음: #dc2626 (빨강)
- 보통: #f59e0b (주황)
- 낮음: #3b82f6 (파랑)
- 성공: #10b981 (초록)

### 타이포그래피

**폰트:**
```css
font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', 'Roboto', sans-serif;
```

**크기:**
- H1: 36-42px, 700 (굵음)
- H2: 18px, 600 (중간)
- 본문: 13-15px, 400 (보통)
- 라벨: 11-12px, 500 (중간)

### 스페이싱

```css
/* 갭 */
gap: 12px;     /* 요소 간 간격 */
gap: 16px;     /* 섹션 간 간격 */
gap: 24px;     /* 카드 간 간격 */

/* 패딩 */
padding: 8px;   /* 버튼 내부 */
padding: 12px;  /* 카드 내부 */
padding: 28px;  /* 큰 카드 */

/* 마진 */
margin-bottom: 20px;  /* 제목 아래 */
margin-bottom: 30px;  /* 섹션 아래 */
```

### 보더 레디우스

```css
border-radius: 8px;   /* 작은 요소 (버튼)  */
border-radius: 12px;  /* 중간 요소 (카드) */
border-radius: 16px;  /* 큰 요소 (섹션) */
border-radius: 24px;  /* 초대형 (컨테이너) */
```

---

## 핵심 기술 스택

### 프론트엔드
- **HTML5** - 구조
- **CSS3** - 디자인 (글래스모피즘, 그라디언트, 애니메이션)
- **JavaScript (Vanilla)** - 상호작용 (테마 전환, 네비게이션)

### 저장소
- **GitHub** - 코드 관리
- **Git 태그** - 버전 관리

### 개발 도구
- **Claude Code** - IDE
- **Git** - 버전 관리

### 특수 기능
- **localStorage** - 테마 저장
- **CSS 변수** - 동적 색상
- **Flex/Grid** - 레이아웃
- **backdrop-filter** - 글래스모피즘

---

## GitHub 연동

### 초기 설정

```bash
# 1. 저장소 생성 (GitHub 웹에서)
# https://github.com/new

# 2. 로컬에서 초기화
git init

# 3. 사용자 설정
git config user.email "이메일"
git config user.name "이름"

# 4. 첫 커밋
git add .
git commit -m "초기 커밋"

# 5. 원격 연결
git remote add origin https://github.com/사용자/저장소.git
git branch -M main
git push -u origin main
```

### 이후 워크플로우

```bash
# 변경사항 추가
git add 파일이름

# 커밋
git commit -m "메시지"

# 푸시
git push origin main

# 버전 태그
git tag -a v0.X.X -m "설명"
git push origin v0.X.X
```

### 보안 주의사항

❌ **하지 마세요:**
- 토큰/키를 커밋에 포함
- 민감한 정보를 공개 저장소에

✅ **하세요:**
- `.gitignore`에 `.env.local` 추가
- 토큰은 로컬에서만 보관
- 노출되면 즉시 재생성

---

## 버전 관리

### 버전 규칙 (Semantic Versioning)

```
v[Major].[Minor].[Patch]

v1.0.0 = 주요 릴리스 (큰 변화)
v0.2.0 = 마이너 업데이트 (기능 추가)
v0.0.1 = 버그 수정 (작은 변화)
```

### 커밋 메시지 규칙

```
[타입]: 설명

타입:
- feat: 새 기능
- fix: 버그 수정
- ui: 디자인/UI 개선
- docs: 문서 작성
- refactor: 코드 정리
- perf: 성능 개선

예:
feat: 다크모드 추가
fix: 네비게이션 메뉴 정렬 수정
docs: README 작성
```

---

## 주의사항 및 팁

### ⚠️ 주의사항

1. **파일명**
   - 한글 파일명 피하기 (GitHub 호환성)
   - 소문자 + 하이픈 사용 (meeting-result.html)

2. **경로 참조**
   - 상대 경로 사용 (./images/logo.png)
   - 절대 경로 피하기 (C:\Users\...)

3. **Git 무시 파일**
   ```
   .env.local    ← 토큰
   node_modules/ ← 설치 파일
   .DS_Store     ← OS 파일
   *.log         ← 로그
   ```

4. **GitHub 토큰**
   - Classic PAT (Personal Access Token) 추천
   - 최소 권한 설정 (repo, workflow)
   - 정기적으로 재생성

### 💡 팁

1. **개발 순서**
   ```
   1. 메인페이지 (사용자 이해)
   2. 데이터 분석 (무엇을 표시할 건가)
   3. 대시보드 (주요 정보)
   4. 보고서 (분석)
   5. 디자인 (통일성)
   6. 네비게이션 (연결성)
   ```

2. **CSS 재사용**
   ```css
   /* 변수 활용으로 색상 통일 */
   --card-bg: rgba(255, 255, 255, 0.8);
   background: var(--card-bg);
   ```

3. **모바일 대응**
   ```css
   @media (max-width: 768px) {
       .grid {
           grid-template-columns: 1fr;
       }
   }
   ```

4. **성능**
   - 불필요한 애니메이션 최소화
   - 이미지 최적화
   - CSS 변수로 중복 제거

5. **접근성**
   - 색상만으로 정보 표현하지 않기
   - 아이콘 + 텍스트 함께 사용
   - 충분한 명도 대비

### 🚀 최적화 팁

1. **개발 속도 향상**
   - CSS 변수 미리 정의
   - 재사용 가능한 컴포넌트 만들기
   - 템플릿 준비

2. **유지보수 용이**
   - 명확한 파일 구조
   - 충분한 주석
   - 버전 기록 유지

3. **품질 보증**
   - 다양한 브라우저에서 테스트
   - 라이트/다크 모드 확인
   - 모바일 반응형 확인

---

## 다음 프로젝트 체크리스트

새 프로젝트를 시작할 때 이 항목들을 체크하세요:

### 계획 단계
- [ ] 데이터 소스 파악
- [ ] 핵심 기능 정의
- [ ] 페이지 구조 설계
- [ ] 색상 팔레트 선택

### 개발 단계
- [ ] index.html 생성
- [ ] dashboard.html 생성 (데이터 시각화)
- [ ] 보고서 페이지 생성
- [ ] 글래스모피즘 디자인 적용
- [ ] 다크/라이트 모드 추가
- [ ] 네비게이션 메뉴 추가
- [ ] 테마 토글 기능 (localStorage)

### 배포 단계
- [ ] .env.local 생성 (토큰 저장)
- [ ] .gitignore 생성
- [ ] Git 저장소 초기화
- [ ] GitHub에 푸시
- [ ] 보안 설정 확인

### 마무리
- [ ] VERSION.md 작성
- [ ] Git 태그 추가
- [ ] 최종 테스트
- [ ] README.md 작성

---

## 마지막 팁

이 가이드를 참고할 때:

1. **단계 순서를 지키세요**
   - 디자인부터 시작하면 후반에 수정이 많습니다
   - 기능을 먼저, 디자인은 나중에

2. **CSS 변수를 활용하세요**
   - 색상 변경이 쉬워집니다
   - 다크모드 구현이 간단해집니다

3. **Git 커밋을 자주 하세요**
   - 작은 단위로 나눠서 커밋
   - 오류 발생 시 되돌리기 쉬워집니다

4. **테스트하면서 개발하세요**
   - 각 단계마다 브라우저에서 확인
   - 라이트/다크 모드 모두 확인
   - 모바일에서도 확인

---

**이 가이드로 다음 프로젝트도 성공적으로 진행할 수 있습니다! 🚀**
