---
type: doc
title: 코드 레포 레지스트리
updated: {{TODAY}}
---
# 코드 레포 레지스트리

볼트는 코드 레포를 **복제하지 않는다.** 에이전트가 레포를 읽어야 하면 이 표의 경로로 간다. `.claude/settings.json`의 `additionalDirectories`와 맞춘다.

| 이름 | 절대 경로 | 원격 | 정본 문서 | 담당 도메인 |
|---|---|---|---|---|
| {{REPO_NAME}} | {{REPO_PATH}} | github / gitlab | `AGENTS.md`, `docs/…` (있으면) | [[wiki/_example-domain/_moc]] |

## 레포별 MOC
- `projects/<repo>/_moc.md` — 레포 경로, 정본 문서 경로, 담당 도메인 링크, 온보딩 메모(코드엔 없는데 알아야 하는 것). 템플릿 `templates/repo-moc.md`.
- 레포 그룹이 여럿이면(예: 내부망 / 외부) `projects/<group>/<repo>/`로 한 단계 더.
