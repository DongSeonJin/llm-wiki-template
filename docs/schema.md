---
type: doc
title: Frontmatter 스키마
updated: 2026-10-26
---
# Frontmatter 스키마

모든 노트는 YAML frontmatter를 가진다. 에이전트는 **필수 필드가 빠진 노트를 생성하지 않는다.** 스키마 변경은 Ask First.

## 공통 필드 (모든 type)

```yaml
type: <아래 type 중 하나>        # 필수
title: <한글 제목>               # 필수
created: YYYY-MM-DD              # 필수
updated: YYYY-MM-DD              # 필수 (내용 변경 시 갱신)
sensitivity: internal            # 필수. public | internal | confidential | restricted
tags:                            # 선택. YAML 리스트
  - example-domain
aliases:                         # 선택. 한글/영문 별칭 (검색·링크용)
  - 송금 취소
sources:                         # wiki 페이지는 필수. 역링크(inbox 원본, 티켓 URL, 레포 경로)
  - "[[inbox/slack/2026-01-01-example-thread]]"
  - "notion: https://app.notion.com/p/..."
  - "repo: my-repo / docs/ARCHITECTURE.md"
related:                         # 선택. 관련 wiki 페이지
  - "[[example-rule]]"
```

## sensitivity 등급
- `public` — 외부 공개 가능 (거의 없음)
- `internal` — 기본값. 사내 공유 가능
- `confidential` — 볼트에는 두되 외부 전송(메신저·외부 API) 금지. 로컬 모델 우선
- `restricted` — **볼트 반입 금지.** 크레덴셜·API 키, 개인정보, 원장성 데이터. 발견 시 `[REDACTED]`

## type별 추가 필드

### `moc` (Map of Content)
추가 필드 없음. 본문은 링크 목록.

### `domain-rule` (도메인 규칙) — `wiki/<domain>/`
```yaml
domain: <domain>                 # 필수. wiki/ 하위 도메인 폴더명
status: current                  # 필수. draft | current | conflict | deprecated
confidence: high                 # 필수. high | medium | low (출처 수·일치도 기준)
applies_to: []                   # 선택. 서비스/환경/국가
```
본문 섹션: `## 규칙` · `## 예외` · `## 근거` · `## 모순` (있을 때만)

### `adr` (결정 기록) — `wiki/decisions/`
```yaml
id: ADR-2026-001                 # 필수. 연도-순번
status: accepted                 # 필수. proposed | accepted | superseded | deprecated
date: YYYY-MM-DD                 # 필수
supersedes: "[[ADR-2025-010-...]]"   # 선택
```
본문 섹션: `## Context` · `## Decision` · `## Consequences`
> 코드 아키텍처 ADR이 레포에 이미 있으면 볼트에는 만들지 말고 링크만.

### `bug-log` (트러블슈팅) — `wiki/bugs/`
```yaml
severity: high                   # 필수. critical | high | medium | low
status: resolved                 # 필수. open | resolved | recurring
service: <service>               # 필수. 외부 작업 DB의 서비스 태그와 맞추면 좋음
symptom: "..."                   # 필수. 한 줄
root_cause: "..."                # resolved면 필수
resolution: "..."                # resolved면 필수
ticket: "https://app.notion.com/p/..."   # 선택
```

### `entity` — `wiki/entities/`
```yaml
entity_type: vendor              # 필수. person | vendor | system | service | team | regulator
```
본문: 역할, 관련 규칙 링크, 연락/연동 경로(크레덴셜 제외).

### `meeting` — `inbox/notion/` 또는 `inbox/manual/`
```yaml
date: YYYY-MM-DD
attendees: []
processed: false
```

### `daily` — `daily/`
```yaml
date: YYYY-MM-DD
```
본문 섹션: `## 오늘` · `## 메모` · `## 위키로 보낼 것` (에이전트가 ingest 대상으로 읽음)

### `raw` (inbox 원본) — `inbox/<source>/`
```yaml
source: slack                    # 필수. gmail | slack | notion | telegram | manual
source_ref: "C0123/p169..."      # 선택. 채널/스레드/메일 ID
captured_at: YYYY-MM-DDTHH:mm
processed: false                 # ingest 후 true
processed_at:
```

### `task-queue` — `projects/_tasks.md` (markdown 모드)
형식은 `docs/task-queue-contract.md` 참고.

### `skill-candidate` — `skills/_candidates/<slug>/SKILL.md`
```yaml
name: deploy-to-develop
description: "..."               # 언제 쓰는지 한 문장
status: candidate                # candidate | approved | rejected
observed_in:                     # 필수. 이 절차가 나온 작업 2개 이상
  - "task: ..."
  - "task: ..."
```

## 파일명 규칙
- 영문 kebab-case, 확장자 `.md`
- ADR: `ADR-YYYY-NNN-short-slug.md`
- bug-log: `YYYY-MM-DD-short-slug.md`
- inbox 원본: `YYYY-MM-DD-short-slug.md`
- daily: `YYYY-MM-DD.md`

## 태그 체계
- 도메인 태그 = 폴더명과 동일 (`auth`, `billing`, …)
- 서비스 태그 = 외부 작업 DB의 서비스 값과 동일하게
- 국가/환경 태그 = ISO2 또는 `develop|staging|prod`
- 그 외 자유 태그는 최소화. 태그보다 링크와 MOC를 우선.
