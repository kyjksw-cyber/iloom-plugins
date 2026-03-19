# iloom-plugins

일룸 AX 팀 플러그인 마켓플레이스

## 설치 방법

### 1단계: 마켓플레이스 등록 (터미널, 최초 1회)

```bash
claude plugin marketplace add kyjksw-cyber/iloom-plugins
```

### 2단계: 원하는 플러그인 설치 (Claude Code 안에서)

```bash
/plugin install 플러그인이름
```

## 플러그인 목록

| 플러그인 | 담당자 | 설명 |
|---------|--------|------|
| iloom-sunwoo | 김선우 | 일룸 AX 업무 자동화 |
| iloom-jihye | 서지혜 | 일룸 마케팅팀 업무 자동화 |
| iloom-jiyeon | 최지연 | 일룸 리테일사업팀 영업관리 자동화 |

## 내 플러그인 올리는 법

Claude Code에게:
```
"내가 만든 [플러그인이름]을 iloom-plugins 저장소에 넣고 push해줘"
```

## 관리 명령어

```bash
/plugin list                    # 설치된 플러그인 목록
/plugin update 플러그인이름      # 업데이트
/plugin uninstall 플러그인이름   # 제거
```
