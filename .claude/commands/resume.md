작업 큐 항목 $ARGUMENTS (Notion URL/AWQ-ID 또는 projects/_tasks.md의 task id)를 docs/task-queue-contract.md의 claim → run/resume 알고리즘대로 재개한다.
순서: 락 확인·획득 → Next Action 읽기(비어있으면 추론 후 사람 확인) → Blocked By 확인 → 하네스가 승인 필요로 판정하면 Awaiting Approval로 두고 멈춤 → 수행 → Run Log append(intent/done) → Done Scope/Next Action/Env State 갱신.
알게 된 지식은 wiki/로, 큐에는 경로만.
