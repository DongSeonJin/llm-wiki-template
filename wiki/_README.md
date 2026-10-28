---
type: doc
title: wiki 사용법
---
# wiki/ — 에이전트가 소유하는 지식 레이어

- 도메인 폴더: `wiki/<domain>/` — `_example-domain`을 복사해서 생성, 각각 `_moc.md`가 진입점
- 횡단 폴더: `decisions/`(ADR) `bugs/`(트러블슈팅) `entities/`(사람·벤더·시스템)
- 모든 페이지는 `sources:`로 원본에 역링크. 고아 페이지 금지.
- 모순 발견 시 덮어쓰지 말고 `## 모순` 섹션 + `status: conflict`.
