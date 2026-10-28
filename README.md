# {{PROJECT_NAME}}-wiki

> 템플릿에서 생성한 볼트. 처음이면 `docs/SETUP.md`부터.

{{PROJECT_NAME}} 지식을 담는 LLM 위키(Obsidian 볼트)입니다. 사람이 읽기도 하지만, **에이전트(Claude Code 등)가 읽고 유지·확장하는 지식 베이스**가 1차 목적입니다.

## 경계 규칙 (가장 중요)

| 질문 | 정본(source of truth) | 비고 |
|---|---|---|
| 무엇을 하고 있고 어디까지 했나 (작업 상태) | **작업 큐** — Notion Agent Work Queue 또는 `projects/_tasks.md` | `docs/task-queue-contract.md` |
| 왜, 어떻게, 규칙은? (지식) | **이 볼트** | 티켓이 닫혀도 남는 정보만 |
| 코드 규칙·아키텍처 | **코드 레포** (그 레포의 AGENTS.md/CLAUDE.md/docs) | 볼트는 **복제하지 않고 링크만** (`docs/repos.md`) |
| 반복되는 절차 | `skills/` | 티켓 2개 이상에서 같은 절차가 나오면 승격 |

한 줄 요약: **티켓이 닫히면 버려도 되는 정보 → Notion. 다음 티켓에서도 쓰이는 정보 → 볼트.**

## 폴더 구조 (3계층 LLM 위키)

```
<project>-wiki/
├── AGENTS.md            # 에이전트 계약 (정본). CLAUDE.md는 shim
├── index.md             # 최상위 MOC — 에이전트가 가장 먼저 읽음
├── log.md               # append-only 작업 로그
├── inbox/               # [raw] 불변 원본. 에이전트는 읽기만
│   ├── gmail/ slack/ notion/ telegram/ manual/
├── daily/               # 데일리 노트 (사람 작성, 에이전트 참고)
├── projects/            # 진행 중인 프로젝트 컨텍스트(티켓 아님, 배경·도메인만)
├── wiki/                # [compiled] 에이전트가 소유·생성하는 지식 레이어
│   ├── <domain>/        # 도메인별 (wiki/_example-domain 복사해서 생성)
│   ├── decisions/       # ADR
│   ├── bugs/            # 트러블슈팅 로그
│   └── entities/        # 사람·벤더·시스템 등 엔티티 페이지
├── skills/              # 승격된 업무 절차 (Claude Code Skills 호환)
│   └── _candidates/     # 자동 생성된 스킬 후보 (사람 승인 전)
├── templates/           # Templater/에이전트 공용 템플릿
├── docs/                # 이 볼트 자체의 설계 문서
├── archive/             # 비활성
└── assets/              # 첨부
```

## 운영 루프

1. **ingest** — `inbox/*`의 새 원본을 읽고 `wiki/`에 엔티티/개념/규칙 페이지를 생성·갱신·교차링크. `index.md`, `log.md` 갱신. 처리된 원본은 frontmatter `processed: true`로 표시(이동·삭제 금지).
2. **query** — 질문을 받으면 `index.md` → 관련 MOC → 페이지 순으로 읽고 답변. 유의미한 탐색 결과는 새 페이지로 파일링.
3. **lint** — 깨진 링크, 고아 페이지, 모순되는 규칙, 오래된 `status: current` 페이지를 점검하고 보고.
4. **task → wiki 역류** — 작업 중 발견한 도메인 규칙·트러블슈팅·절차를 `wiki/` 또는 `skills/_candidates/`에 남김.

## 시작하기

`docs/SETUP.md` → `docs/roadmap.md` 순서.
