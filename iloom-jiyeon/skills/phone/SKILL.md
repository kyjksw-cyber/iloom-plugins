---
name: phone
classification: workflow
classification-reason: "파일을 클라우드 동기화 폴더로 복사하여 모바일에서 접근 가능하게 하는 워크플로우"
deprecation-risk: none
description: |
  파일을 클라우드 동기화 폴더에 저장해서 핸드폰에서 바로 볼 수 있게 해주는 스킬.
  iCloud, Google Drive, Dropbox, OneDrive 자동 감지.
  Triggers: /phone
  Keywords: phone, 핸드폰, mobile, iCloud, Google Drive, Dropbox, OneDrive, 저장, 폰에서 보기
argument-hint: "/phone {파일경로} 또는 /phone list 또는 /phone setup"
user-invocable: true
allowed-tools:
  - Read
  - Write
  - Edit
  - Glob
  - Grep
  - Bash
---

# phone - 핸드폰에서 보기

파일을 클라우드 동기화 폴더에 저장해서 핸드폰에서 바로 열어볼 수 있게 해주는 스킬.

## Step 1: 클라우드 서비스 자동 감지

첫 실행 시 (`/phone setup` 또는 자동) 아래 경로를 순서대로 확인하여 사용 가능한 서비스를 감지한다:

| 서비스 | 감지 경로 (macOS) | 감지 경로 (Linux/Windows) |
|--------|-------------------|--------------------------|
| iCloud Drive | `~/Library/Mobile Documents/com~apple~CloudDocs/` | (macOS 전용) |
| Google Drive | `~/Library/CloudStorage/GoogleDrive-*/My Drive/` 또는 `~/Google Drive/` | `~/Google Drive/` |
| Dropbox | `~/Dropbox/` 또는 `~/Library/CloudStorage/Dropbox/` | `~/Dropbox/` |
| OneDrive | `~/Library/CloudStorage/OneDrive-*/` 또는 `~/OneDrive/` | `~/OneDrive/` |

**감지 로직:**
```bash
# 1. iCloud (macOS)
test -d ~/Library/Mobile\ Documents/com~apple~CloudDocs/

# 2. Google Drive
ls -d ~/Library/CloudStorage/GoogleDrive-*/My\ Drive/ 2>/dev/null || test -d ~/Google\ Drive/

# 3. Dropbox
test -d ~/Dropbox/ || ls -d ~/Library/CloudStorage/Dropbox/ 2>/dev/null

# 4. OneDrive
ls -d ~/Library/CloudStorage/OneDrive-*/ 2>/dev/null || test -d ~/OneDrive/
```

**감지 결과 처리:**
- 1개 발견 → 자동 선택
- 여러 개 발견 → 사용자에게 선택 요청
- 0개 발견 → 수동 경로 입력 안내

설정을 `.phone-sync.json`에 저장:
```json
{
  "provider": "icloud",
  "base_path": "~/Library/Mobile Documents/com~apple~CloudDocs/my-pocket",
  "folders": ["scripts", "images", "notes"]
}
```

## Step 2: 폴더 생성

선택된 클라우드 경로 아래에 `my-pocket/` 폴더 구조 생성:

```
{cloud_base}/my-pocket/
├── scripts/   ← 코드, 스크립트
├── images/    ← 이미지, 스크린샷
└── notes/     ← 메모, 마크다운
```

## Step 3: 파일 저장 — `/phone {파일경로}`

파일 확장자에 따라 자동으로 하위 폴더 분류:
- `.md`, `.txt` → `notes/`
- `.png`, `.jpg`, `.jpeg`, `.gif`, `.svg`, `.webp` → `images/`
- 그 외 (`.js`, `.ts`, `.py`, `.sh`, `.css`, `.html` 등) → `scripts/`

**실행 방법:**
1. `.phone-sync.json` 읽어서 base_path 확인 (없으면 Step 1 실행)
2. 파일 확장자로 하위 폴더 결정
3. 파일 복사
4. 완료 메시지 출력

**서비스별 안내 메시지:**
- iCloud: "핸드폰 **파일 앱** → iCloud Drive → my-pocket에서 확인하세요!"
- Google Drive: "핸드폰 **Google Drive 앱** → my-pocket에서 확인하세요!"
- Dropbox: "핸드폰 **Dropbox 앱** → my-pocket에서 확인하세요!"
- OneDrive: "핸드폰 **OneDrive 앱** → my-pocket에서 확인하세요!"

## 추가 명령어

### `/phone list`
my-pocket 폴더의 전체 파일 목록을 카테고리별로 보여줌.

### `/phone delete {파일명}`
특정 파일을 my-pocket에서 삭제. 삭제 전 확인 요청.

### `/phone setup`
클라우드 서비스 재감지 및 설정 변경.

### `/phone status`
현재 설정된 클라우드 서비스와 경로, 저장된 파일 수 표시.

## 자연어 트리거

사용자가 다음과 같이 말하면 자동으로 이 스킬 동작:
- "이거 폰에서 보고 싶어"
- "핸드폰에 저장해줘"
- "pocket에 넣어줘"
- "모바일에서 볼 수 있게 해줘"
- "save to phone"
- "send to mobile"

## 사전 준비 가이드 (클라우드 서비스 없는 사용자용)

`/phone setup` 실행 시 클라우드 서비스가 감지되지 않으면, 아래 가이드를 사용자에게 안내한다.

### iCloud Drive (Apple 기기 사용자 추천)

> **필요 조건**: Mac + iPhone/iPad
>
> 1. Mac에서 **시스템 설정** → **Apple ID** → **iCloud** → **iCloud Drive** 켜기
> 2. iPhone에서 **설정** → **Apple ID** → **iCloud** → **iCloud Drive** 켜기
> 3. 다시 `/phone setup` 실행하면 자동 감지됩니다!
>
> **핸드폰에서 보는 법**: 파일 앱 → iCloud Drive → my-pocket

### Google Drive (Android 사용자 추천, 범용)

> **필요 조건**: Google 계정
>
> **컴퓨터 설정:**
> 1. [Google Drive 데스크톱](https://www.google.com/drive/download/) 설치
> 2. Google 계정으로 로그인
> 3. 다시 `/phone setup` 실행하면 자동 감지됩니다!
>
> **핸드폰 설정:**
> - Android: 기본 설치되어 있음
> - iPhone: App Store에서 "Google Drive" 설치
>
> **핸드폰에서 보는 법**: Google Drive 앱 → my-pocket

### Dropbox

> **필요 조건**: Dropbox 계정 (무료 2GB)
>
> **컴퓨터 설정:**
> 1. [Dropbox 데스크톱](https://www.dropbox.com/install) 설치
> 2. Dropbox 계정으로 로그인
> 3. 다시 `/phone setup` 실행하면 자동 감지됩니다!
>
> **핸드폰 설정:**
> - App Store / Google Play에서 "Dropbox" 설치
>
> **핸드폰에서 보는 법**: Dropbox 앱 → my-pocket

### OneDrive (Windows 사용자 추천)

> **필요 조건**: Microsoft 계정 (무료 5GB)
>
> **컴퓨터 설정:**
> 1. [OneDrive](https://www.microsoft.com/ko-kr/microsoft-365/onedrive/download) 설치 (Windows는 기본 설치)
> 2. Microsoft 계정으로 로그인
> 3. 다시 `/phone setup` 실행하면 자동 감지됩니다!
>
> **핸드폰 설정:**
> - App Store / Google Play에서 "OneDrive" 설치
>
> **핸드폰에서 보는 법**: OneDrive 앱 → my-pocket

## 주의사항
- 클라우드 동기화에 몇 초~몇 분 걸릴 수 있음
- 같은 이름 파일이 있으면 덮어쓰기 전에 사용자에게 확인
- 대용량 파일(100MB+)은 경고 후 확인받기
- `.phone-sync.json`은 프로젝트 루트에 생성됨 (`.gitignore`에 추가 권장)
