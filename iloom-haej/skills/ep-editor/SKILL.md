---
name: ep-editor
description: .md 품의서를 일룸 스타일가이드 기반 SE 에디터 HTML로 변환. "/ep-editor", "SE 에디터", "네이버 에디터 HTML", "스마트에디터 HTML", "품의서 변환" 언급 시 자동 실행.
---

# /ep-editor - SE 에디터 품의서 HTML 변환

## 메타데이터 제외 규칙

파일 상단의 아래 패턴은 변환에서 **제외**합니다.

| 제외 패턴 |
|---------|
| YAML frontmatter (`---` ~ `---`) |
| 파일 최상단 H1 제목 |
| `작성일:` / `날짜:` / `Date:` |
| `작성자:` / `담당자:` / `Author:` |
| `팀:` / `부서:` / `Team:` |
| `버전:` / `Version:` |
| `결재자:` / `Approver:` |

본문은 메타 블록 이후 첫 번째 실질적 내용(소제목, 단락)부터 시작합니다.

---

## 고정 첫 문장

본문 HTML의 **첫 번째 요소**는 항상 아래 문장입니다:

```html
<p style="font-family:'Noto Sans KR',sans-serif; font-size:15px; font-weight:400; color:#333333; line-height:24px; margin:0; text-align:center;">아래와 같이 진행하고자 하오니, 검토 후 재가 바랍니다.</p>
<p style="font-size:15px; line-height:24px;"><br></p>
```

---

## 변환 규칙

**원칙**: 모든 요소는 `<p>` 태그 기반, 인라인 `style`만 사용 (class 금지)

| 마크다운 요소 | SE 에디터 HTML |
|-------------|--------------|
| `## H2` (대제목) | `<p style="font-family:'Noto Sans KR',sans-serif; line-height:24px; margin:0;"><span style="font-size:17px; font-weight:700; color:#333333;">◆ 내용</span></p>` |
| `### H3` (소제목) | `<p style="font-family:'Noto Sans KR',sans-serif; line-height:24px; border-left:3px solid #c80a1e; padding-left:8px; margin:0;"><span style="font-size:15px; font-weight:700; color:#333333;">내용</span></p><p style="font-size:15px; line-height:24px;"><br></p>` |
| `#### H4` (소소제목) | `<p style="font-family:'Noto Sans KR',sans-serif; font-size:15px; font-weight:500; color:#555555; line-height:24px; margin:0;">내용</p>` |
| 본문 단락 | `<p style="font-family:'Noto Sans KR',sans-serif; font-size:15px; font-weight:400; color:#333333; line-height:24px; margin:0;">내용</p>` |
| `**굵게**` | `<span style="font-weight:700; color:#333333;">내용</span>` |
| `- 리스트` | `<p style="font-family:'Noto Sans KR',sans-serif; font-size:15px; color:#333333; line-height:24px; padding-left:16px; margin:0;">· 항목</p>` |
| 중첩 리스트 (`  -`) | `<p style="font-family:'Noto Sans KR',sans-serif; font-size:15px; color:#333333; line-height:24px; padding-left:32px; margin:0;">· 항목</p>` |
| `> 인용구` | `<p style="font-family:'Noto Sans KR',sans-serif; font-size:15px; color:#999999; line-height:24px; padding-left:12px; border-left:3px solid #E0E0E0; margin:0;">내용</p>` |
| `---` 구분선 | `<p style="border-top:1px solid #E0E0E0; margin:16px 0; padding:0;"><br></p>` |
| 빈 줄 | `<p style="font-size:15px; line-height:24px;"><br></p>` |
| `[텍스트](url)` | `<a href="url" style="color:#c80a1e; text-decoration:none;">텍스트</a>` |
| `![이미지]()` | `<!-- 이미지: [경로] 직접 삽입 필요 -->` |
| 표 `<th>` | `<th style="border:1px solid #E0E0E0; padding:8px 12px; background:#F8F8F8; font-weight:700; text-align:left;">` |
| 표 `<td>` | `<td style="border:1px solid #E0E0E0; padding:8px 12px;">` |
| 표 `<table>` | `<table style="border-collapse:collapse; width:100%; font-family:'Noto Sans KR',sans-serif; font-size:15px; color:#333333; line-height:24px;">` |

표 내 금액·퍼센트 강조: `style="color:#c80a1e; font-weight:700;"` / `**굵게**`는 `<strong>` 사용

---

## 출력 원칙

- 산문체 설명 금지
- 완료 메시지 한 줄 + HTML 코드블록만 출력

```
✅ [파일명] → SE 에디터 HTML 변환 완료
```
