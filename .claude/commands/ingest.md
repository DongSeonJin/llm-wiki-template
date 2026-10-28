AGENTS.md의 ingest 워크플로우를 수행한다.
1. inbox/ 하위에서 frontmatter `processed`가 없거나 false인 파일을 모두 찾는다 ($ARGUMENTS 가 주어지면 그 경로만).
2. 각 파일을 요약하고 docs/schema.md 스키마에 맞춰 wiki/ 페이지를 생성·갱신한다 (외과적 갱신, 모순은 `## 모순` + status: conflict).
3. 해당 도메인 _moc.md와 index.md에 링크를 추가한다.
4. log.md에 한 줄 append, 원본 frontmatter에 processed: true, processed_at 기록.
5. 변경 요약을 보고하고 `ingest: ...` 접두사로 커밋한다. inbox 원본의 본문은 절대 수정하지 않는다.
