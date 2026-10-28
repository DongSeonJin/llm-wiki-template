AGENTS.md의 lint 워크플로우를 수행한다. 보고만 하고 자동 수정은 깨진 [[링크]] 교정까지만.
점검 항목: 깨진 위키링크, sources가 비어있는 wiki 페이지(고아), status: conflict 페이지, 90일 이상 갱신 없는 status: current 규칙, 30일 이상 방치된 skills/_candidates, frontmatter 필수 필드 누락, sensitivity: restricted 의심 내용(크레덴셜 패턴).
결과를 표 대신 불릿으로 보고하고 log.md에 `lint` 항목을 남긴다.
