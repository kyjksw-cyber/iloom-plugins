# 일룸 AX 플러그인 마켓플레이스 가이드

> 일룸 영업효율개선팀 AI 전환(AX) 프로그램
> 마켓플레이스 주소: `kyjksw-cyber/iloom-plugins`

---

## 목차

1. [플러그인이 뭔가요?](#1-플러그인이-뭔가요)
2. [처음 세팅 (최초 1회)](#2-처음-세팅-최초-1회)
3. [남의 플러그인 설치하기](#3-남의-플러그인-설치하기)
4. [내 플러그인 만들기](#4-내-플러그인-만들기)
5. [내 플러그인 마켓플레이스에 올리기](#5-내-플러그인-마켓플레이스에-올리기)
6. [플러그인 목록 보기](#6-플러그인-목록-보기)
7. [관리 명령어 모음](#7-관리-명령어-모음)
8. [막힐 때](#8-막힐-때)

---

## 1. 플러그인이 뭔가요?

**기존 스킬/커맨드** → 나만 쓸 수 있고, 공유하려면 파일을 직접 보내야 함

**플러그인** → 마켓플레이스에 올려두면 팀원이 한 줄로 설치, 업데이트도 자동

```
# 팀원이 설치할 때
claude plugin install iloom-sunwoo

# 업데이트할 때
claude plugin update iloom-sunwoo
```

한 줄 요약: **Plugin = 내 스킬/커맨드를 팀과 나누는 방법**

---

## 2. 처음 세팅 (최초 1회)

### Step 1. GitHub CLI 설치 확인

PowerShell에서:

```powershell
gh --version
```

버전이 뜨면 OK. 안 뜨면 설치:

```powershell
winget install --id GitHub.cli
```

### Step 2. GitHub 로그인

```powershell
gh auth login
```

선택지:
1. `GitHub.com` → Enter
2. `HTTPS` → Enter
3. `Login with a web browser` → Enter
4. 브라우저에서 GitHub 계정으로 로그인

### Step 3. 마켓플레이스 등록

```powershell
claude plugin marketplace add kyjksw-cyber/iloom-plugins
```

`Successfully added marketplace: iloom-plugins` 뜨면 완료.

> 이 3단계는 최초 1회만 하면 됩니다.

---

## 3. 남의 플러그인 설치하기

### 설치 가능한 플러그인 목록 확인

```powershell
claude plugin search
```

또는 GitHub에서 직접 확인:
→ https://github.com/kyjksw-cyber/iloom-plugins

### 설치

```powershell
claude plugin install [플러그인이름]
```

예시:

```powershell
claude plugin install iloom-sunwoo
claude plugin install iloom-weekly-report
claude plugin install iloom-cx
```

### 설치 확인

Claude Code 재시작 후 `/help` 입력하면 `(plugin:플러그인이름)` 라벨 붙은 커맨드가 보입니다.

---

## 4. 내 플러그인 만들기

### 플러그인 이름 규칙

- 영문 소문자 + 하이픈만 가능 (한글 불가)
- 다른 사람과 겹치면 안 됨
- 형식: `iloom-[내이름 또는 업무]`

```
iloom-sunwoo       ← 이름 기반
iloom-weekly       ← 업무 기반
iloom-cx           ← 팀/역할 기반
```

### Claude Code에게 만들어달라고 시키기

Claude Code 채팅창에서:

```
"내 [스킬/커맨드 이름]을 iloom-[이름] 플러그인으로 만들어줘"
```

Claude Code가 자동으로 아래 구조를 만들어 줍니다:

```
iloom-[이름]/
├── .claude-plugin/
│   └── plugin.json          ← 플러그인 신분증 (필수)
├── skills/                  ← 도메인 지식
│   └── [스킬이름]/
│       └── SKILL.md
├── commands/                ← 슬래시 커맨드
│   └── [커맨드이름].md
└── agents/                  ← 자율 실행 에이전트
    └── [에이전트이름].md
```

### plugin.json 예시

```json
{
  "name": "iloom-sunwoo",
  "version": "1.0.0",
  "description": "일룸 AX 업무 자동화 - 선우"
}
```

필수 필드는 `name`, `version`, `description` 3개뿐입니다.

### 로컬 테스트

만든 플러그인을 올리기 전에 로컬에서 먼저 테스트:

```powershell
claude --plugin-dir C:\Users\[내이름]\iloom-plugins\iloom-[이름]
```

---

## 5. 내 플러그인 마켓플레이스에 올리기

### 사전 조건: 저장소 초대 수락

플러그인을 올리려면 `kyjksw-cyber/iloom-plugins` 저장소의 collaborator여야 합니다.
→ **김선우에게 GitHub 계정명 알려주면 초대해드립니다.**

초대 이메일이 오면 `Accept invitation` 클릭하면 됩니다.

### 최초 1회: 저장소 가져오기

```powershell
cd C:\Users\[내이름]
git clone https://github.com/kyjksw-cyber/iloom-plugins.git
cd iloom-plugins
```

### 내 플러그인 올리기

Claude Code에게 시키기:

```
"내가 만든 iloom-[이름] 플러그인을 iloom-plugins 저장소에 넣고 push해줘"
```

Claude Code가 자동으로 처리:
1. `git pull` 로 최신 상태 동기화
2. 내 플러그인 폴더를 저장소에 복사
3. `marketplace.json`에 플러그인 등록
4. `git push`

### 업데이트할 때

```
"iloom-[이름] 플러그인 수정했으니 버전 올리고 push해줘"
```

다른 팀원은:

```powershell
claude plugin update iloom-[이름]
```

### 주의사항

- **자기 폴더만 수정** — 다른 사람 플러그인 폴더는 건드리지 않기
- push 전 Claude Code가 자동으로 pull 먼저 함 → 충돌 거의 없음
- 충돌 나면: `"충돌 해결해줘"` 라고 Claude Code에게 시키면 됨

---

## 6. 플러그인 목록 보기

### 마켓플레이스에 있는 플러그인 검색

```powershell
claude plugin search
```

### GitHub에서 직접 보기

https://github.com/kyjksw-cyber/iloom-plugins

각 폴더가 하나의 플러그인입니다.

### 내가 설치한 플러그인 목록

```powershell
claude plugin list
```

### 특정 플러그인 상세 정보

```powershell
claude plugin info iloom-sunwoo
```

---

## 7. 관리 명령어 모음

```powershell
claude plugin list                     # 설치된 플러그인 목록
claude plugin search                   # 마켓플레이스 검색
claude plugin install [이름]           # 설치
claude plugin update [이름]            # 업데이트
claude plugin info [이름]              # 상세 정보
claude plugin disable [이름]           # 임시 비활성화 (삭제 아님)
claude plugin enable [이름]            # 다시 활성화
claude plugin uninstall [이름]         # 삭제
```

Claude Code 채팅창에서:

```
/reload-plugins                        # 재시작 없이 변경사항 적용
```

---

## 8. 막힐 때

### 플러그인이 안 보여요

Claude Code를 재시작하거나:

```
/reload-plugins
```

### 설치 오류가 나요

마켓플레이스가 등록됐는지 확인:

```powershell
claude plugin marketplace list
```

`iloom-plugins`가 없으면 다시 등록:

```powershell
claude plugin marketplace add kyjksw-cyber/iloom-plugins
```

### 플러그인 만들 때 막혀요

Claude Code에게:

```
"plugins-guide.md를 참고해서 [내 스킬/커맨드 이름]을 플러그인으로 만들어줘"
```

### GitHub 계정이 없어요

1. https://github.com 접속
2. Sign up 클릭
3. 이메일로 가입 (계정명은 영문으로)
4. 계정명을 김선우에게 알려주기

---

*일룸 AX 플러그인 마켓플레이스 | 관리자: 김선우 (@kyjksw-cyber)*
