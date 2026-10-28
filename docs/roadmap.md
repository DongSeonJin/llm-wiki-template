---
type: doc
title: 구축 로드맵 (Claude Code에서 진행)
updated: 2026-10-26
---
# 구축 로드맵

설계는 끝났다. 아래는 **로컬 + Claude Code**에서 진행할 체크리스트다. 각 단계는 끝나면 `log.md`에 `manual` 항목으로 남긴다.

## 0. 전제 결정 (확정)
- 볼트/레포 이름: `{{PROJECT_NAME}}-wiki` (GitHub private)
- 작업 상태 정본: `docs/task-queue-contract.md`에서 선택 (Notion AWQ 또는 `projects/_tasks.md`)
- 지식 정본: 이 볼트. 코드 규칙은 코드 레포(복제 금지)
- 파일명 영문 slug, 본문 한국어
- 1차 에이전트: Claude Code. 비서 레이어(OpenClaw 2 / Hermes / Claude Duet)는 2~4주 운영 후 결정
- OneDrive 동기화 폴더 **밖**에 볼트를 둔다

## 1주차 — 뼈대 가동
- [ ] `docs/SETUP.md` 체크리스트 완료
- [ ] GitHub private 레포 생성(빈 상태) → 이 폴더에서 `git remote add origin ... && git push -u origin main`
- [ ] Obsidian으로 볼트 열기. 커뮤니티 플러그인 설치:
  - [ ] **Obsidian Git** (자동 커밋 10분, 자동 push on/off는 취향)
  - [ ] **Templater** → `templates/` 폴더 지정
  - [ ] **Dataview** (MOC 자동 목록용, 선택)
- [ ] Claude Code를 볼트 루트에서 실행 → `CLAUDE.md`가 `AGENTS.md`를 로드하는지 확인
- [ ] `inbox/manual/`에 원본 2~3개 넣고 "ingest 해줘" → `wiki/`·`index.md`·`log.md` 갱신되는지 확인
- [ ] 도메인 규칙 시드: 각 도메인에 1~3개씩 직접 작성 또는 ingest

## 2주차 — 티켓 연동 + 스킬 루프
- [ ] 작업 큐 연결 (`docs/task-queue-contract.md`). Notion이면 MCP 연결, 아니면 `projects/_tasks.md`로 항목 1개 → `/resume` 테스트
- [ ] Slack MCP 연결. 채널은 처음엔 2~3개만 지정하고, 에이전트가 `inbox/slack/`로 캡처하도록 슬래시 커맨드(`/capture-slack <channel>`) 작성
- [ ] Gmail 연결(MCP 또는 Web Clipper) → `inbox/gmail/`
- [ ] 슬래시 커맨드 작성: `/ingest`, `/lint`, `/today`(어제 Run Log + 오늘 티켓 브리핑), `/resume <ticket-url>`
- [ ] 같은 절차 2회 반복 시 `skills/_candidates/`에 후보가 생기는지 확인 → 사람 승인 → `skills/`로 승격 1건

## 3주차 — 검색 레이어 + 자동화
- [ ] 노트 200개 넘으면: **Smart Connections**(로컬 임베딩) 또는 `qmd` MCP 설치. `.smart-env/`는 gitignore 확인
- [ ] Obsidian **Local REST API (MCP 내장)** 플러그인 설치 → 섹션 단위 패치 테스트. 처음엔 read-only
- [ ] `/loop` 또는 cron(launchd/systemd)으로 야간 `ingest` + 주 1회 `lint` 스케줄
- [ ] `lint` 결과로 `status: conflict` 페이지 정리

## 4주차+ — 비서 레이어 결정
- [ ] 2~4주 운영 로그(`log.md`)를 보고 "메신저에서 즉시 명령" 수요가 실제로 있는지 판단
- [ ] 있으면: OpenClaw 2 / Hermes Agent 중 하나를 **전용 사용자·read-only 볼트**로 설치, Telegram 읽기+보고부터. `docs/security.md` 추가 규칙 적용
- [ ] Claude Duet 한국 출시·로컬/레포 접근 지원 여부 재확인 → 가능하면 셀프호스트 대신 Duet 채택 검토
- [ ] 에이전트 2개 이상 운영 후 "세션 기억 단절"이 체감되면 agentmemory(rohitg00/agentmemory) 도입 검토

## 변경 트리거
- 노트 2,000개 초과 → qmd/커스텀 MCP 검색 필수
- `AGENTS.md` 150줄 초과 또는 에이전트가 지침을 무시하기 시작 → 도메인별 지침을 `docs/`로 분리하고 AGENTS.md는 포인터만
- `restricted` 범주 발견 빈도 증가 → `docs/security.md` 범주 확정 + 자동 redaction 훅 추가
