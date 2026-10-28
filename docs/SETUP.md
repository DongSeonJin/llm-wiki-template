---
type: doc
title: 새 볼트 셋업 체크리스트
updated: {{TODAY}}
---
# SETUP — 템플릿에서 새 볼트 만들기

## 1. 플레이스홀더 치환
볼트 루트에서 (macOS):
```bash
PROJECT=myproj; TODAY=$(date +%F)
grep -rl '{{' --include='*.md' --include='*.json' . | xargs sed -i '' \
  -e "s/{{PROJECT_NAME}}/$PROJECT/g" -e "s/{{TODAY}}/$TODAY/g"
grep -rn '{{' . --include='*.md' --include='*.json'   # 남은 것 수동 처리
```
남는 플레이스홀더:
- `{{TASK_QUEUE_MODE}}` → `notion` 또는 `markdown` (`docs/task-queue-contract.md`)
- `{{TASK_QUEUE}}` → AWQ URL 또는 `projects/_tasks.md` (`index.md`)
- `{{AWQ_URL}}`, `{{AWQ_DATASOURCE}}`, `{{TEAM_DB_LIST}}` → notion 모드일 때만
- `{{REPO_NAME}}`, `{{REPO_PATH}}` → `docs/repos.md`, `.claude/settings.json`. 레포 없으면 행/항목 삭제
- `{{RESTRICTED_CATEGORIES}}` → `docs/security.md`

## 2. 도메인 폴더
```bash
for d in auth billing infra; do cp -R wiki/_example-domain wiki/$d; done
rm -rf wiki/_example-domain
```
각 `_moc.md`의 title/tags 수정, `index.md` 도메인 목록에 링크 추가.

## 3. 작업 큐
- **markdown 모드**: 끝. `projects/_tasks.md` 그대로 사용.
- **notion 모드**: 노션 MCP에서 아래 DDL로 DB 생성 후 URL/data source를 계약 문서에 기입. 외부(팀) DB가 있으면 `RELATION('<그 DB data source id>')` 컬럼 추가.
```sql
CREATE TABLE (
  "Name" TITLE, "ID" UNIQUE_ID PREFIX 'AWQ',
  "Agent Status" STATUS('Backlog':gray:to_do,'Ready':yellow:to_do,'Claimed':blue:in_progress,'Running':green:in_progress,'Awaiting Approval':orange:in_progress,'Blocked':red:in_progress,'Handed Off':purple:in_progress,'Done':green:complete,'Dropped':default:complete),
  "Source" SELECT('team':pink,'slack':purple,'gmail':red,'telegram':green,'personal':gray),
  "Source URL" URL, "Priority" SELECT('Critical':pink,'High':red,'Medium':yellow,'Low':green),
  "Service" MULTI_SELECT('example':gray), "Environment Target" SELECT('develop':green,'staging':yellow,'prod':red,'n/a':gray),
  "Requires Human Approval" CHECKBOX, "Approved By Human" CHECKBOX,
  "Done Scope" RICH_TEXT, "Next Action" RICH_TEXT, "Blocked By" RICH_TEXT, "Env State" RICH_TEXT,
  "Knowledge Refs" RICH_TEXT, "Agent Lock" RICH_TEXT, "Due" DATE, "Created" CREATED_TIME, "Updated" LAST_EDITED_TIME
)
```
그다음 self-relation: `ADD COLUMN "Parent" RELATION('<자기 data source id>', DUAL 'Subtasks' 'subtasks')`.

## 4. 코드 레포 연결
`docs/repos.md` 표 채우기 → `projects/<repo>/_moc.md` (템플릿 `repo-moc`) → `.claude/settings.json`의 `additionalDirectories`에 절대경로.

## 5. Obsidian
- 볼트를 클라우드 동기화 폴더(OneDrive/iCloud) **밖**에 둔다.
- `.obsidian/`이 템플릿에 포함돼 있으면 플러그인이 그대로 온다. 없으면 Templater · Git · Dataview 설치.
- Settings: 새 노트 `inbox/manual`, 첨부 `assets`, Daily `daily` + `templates/daily`, Templater 폴더 `templates`, Git auto-commit 10분.

## 6. Git
```bash
git init && git add -A && git commit -m "chore: vault from template" && git branch -M main
```
원격은 필요해질 때 (`docs/roadmap.md`). 개인/회사 계정 분리가 필요하면 레포 전용 Deploy Key.

## 7. 첫 동작
Claude Code를 볼트 루트에서 열고 `inbox/manual/`에 메모 하나 → `/ingest` → `wiki/`·`index.md`·`log.md` 갱신 확인.
