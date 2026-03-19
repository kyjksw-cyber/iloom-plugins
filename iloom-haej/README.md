# iloom-haej Plugin

일룸 마케팅팀 혜정님 업무 자동화 플러그인

## 포함된 스킬

| 스킬 | 트리거 | 설명 |
|------|--------|------|
| `ep-editor` | `/iloom-haej:ep-editor` | .md 품의서 → 네이버 SE 에디터 HTML 변환 |

## 설치

```bash
/plugin install iloom-haej
```

## 사용법

### ep-editor

```
/iloom-haej:ep-editor [파일경로.md]
```

변환된 HTML을 네이버 스마트에디터에 적용:
1. EP 에디터 메뉴에서 `</>` (소스보기) 클릭
2. 기존 내용 전체 삭제 후 변환 코드 붙여넣기
3. 다시 `</>` 클릭하여 일반 편집 모드 복귀

## 구조

```
iloom-haej/
├── .claude-plugin/
│   └── plugin.json
├── skills/
│   └── ep-editor/
│       └── SKILL.md
└── README.md
```

**버전:** 1.0.0 | **작성:** 2026-03-19
