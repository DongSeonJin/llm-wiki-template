---
type: doc
title: skills 사용법
---
# skills/ — 승격된 업무 절차

- 구조는 Claude Code Skills와 호환: `skills/<name>/SKILL.md` (+ 보조 파일)
- 승격 규칙: `skills/_candidates/`의 후보 중 **사람이 승인**한 것만 여기로 이동. 에이전트가 직접 승격하지 않는다.
- 후보 생성 조건: 같은 절차가 AWQ/티켓 **2건 이상**에서 반복. `observed_in`에 근거 2개 이상 필수.
- 승격 시 `status: approved`로 바꾸고 `log.md`에 `skill | promoted <name>` 기록.
