docs/task-queue-contract.md의 sync 알고리즘을 수행한다.
- 큐가 Notion AWQ면: 외부(팀) 작업 DB에서 내게 할당된 미완료 항목을 조회해 AWQ에 relation으로 연결된 행이 없으면 생성한다 (Agent Status=Backlog). 외부 DB는 수정하지 않는다.
- 큐가 projects/_tasks.md면: inbox/·daily/의 `## 위키로 보낼 것`/액션 아이템에서 새 작업을 뽑아 _tasks.md에 `status: backlog`로 추가한다.
생성/제안 목록을 보고한다.
