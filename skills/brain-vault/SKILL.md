---
name: brain-vault
description: 팀(인스피릿) 관련 주제가 나오면 사용한다. 회사·프로젝트·과거 결정·팀원(CEO/CTO/CMO)·회의·일정 이야기, "팀 볼트에 있나", "우리가 정리했던 거" 류 질문 전부 해당. brain-vault MCP 서버로 팀 지식볼트를 검색·열람해 사전지식으로 활용한다.
---

# 팀 지식볼트 (brain-vault) 활용

brain-vault MCP 서버가 연결되어 있다. 팀 지식볼트(team-brain)를 서버가 대신 읽어주는 읽기 전용 창구이며, 데이터는 항상 조회 시점의 최신 상태다.

## 도구

- `search_vault { query, max_results? }`: 볼트 전체(공유 + 본인 personal) 키워드 검색. 여러 단어는 AND.
- `read_note { path }`: 문서 전문 읽기. 검색 결과의 상대경로 또는 위키 페이지 id를 넘긴다.
- `list_recent { limit? }`: 최근 수정된 문서 목록. 팀의 최근 흐름 파악용.

## 언제 쓰나

- 회사·팀·프로젝트·과거 결정에 관한 질문이면 **답하기 전에 볼트부터 검색**한다. 일반 지식으로 추측하지 않는다.
- "요즘 팀에서 무슨 일 있었어" 류는 `list_recent`부터.
- 볼트에 뭐가 있는지 감이 없으면 `read_note`로 `shared/wiki/index.md`(전체 목차)를 먼저 읽는다.

## 볼트 구조 (사전지식)

카파시 LLM Wiki 패턴. 사람이 원본을 모으고, AI 사서가 위키를 유지한다.

```
shared/                  팀 공유
├─ wiki/                 지식 페이지 (index.md = 목차, overview.md = 큰 그림, log.md = 작업 로그)
│  ├─ entities/          사람·회사·도구·제품·프로젝트
│  ├─ concepts/          아이디어·패턴·기법
│  ├─ sources/           인제스트한 소스별 요약
│  └─ synthesis/         질의 답변을 저장한 종합
├─ raw/                  불변 원본 (memos/ clippings/ files/)
├─ journal/              운영 큐 (인터뷰 질문 등)
└─ calendar/             팀 일정
personal/<이름>/          개인 공간 (같은 구조. 서버가 본인 것만 보여준다)
```

## 사용 규칙

- 볼트 내용을 인용할 때는 출처 경로를 함께 말한다 (예: shared/wiki/concepts/foo.md).
- 이 창구는 **읽기 전용**이다. 볼트에 기록·수정하려면 brain-chat 앱(https://brain.inspirit-dev.cloud)에서 한다.
- 다른 사람의 personal/은 서버가 차단한다. 접근 거부 응답이 오면 그대로 받아들이고 우회하지 않는다.
- 검색이 비면 단어를 줄이거나 동의어로 재시도하고, 그래도 없으면 "볼트에 기록 없음"을 명시한다.
