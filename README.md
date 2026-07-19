# brain-chat-plugin

팀 지식볼트(team-brain)를 각자의 Claude Code에서 사전지식으로 쓰게 해주는 플러그인.
볼트 사본 없이, brain-chat 서버의 MCP 엔드포인트(`/api/mcp`)를 통해 조회 시점의 최신 데이터를 읽는다.
읽기 전용이며, 다른 사람의 personal/은 서버가 차단한다.

## 팀원 설치 (3단계)

1. Claude Code 안에서 플러그인 설치:

   ```
   /plugin marketplace add als8921/brain-chat-plugin
   /plugin install brain-vault@brain-chat-plugin
   ```

2. 운영자에게 받은 API 키를 셸 설정에 등록 (`~/.zshrc`):

   ```bash
   export BRAIN_MCP_TOKEN="받은-키"
   ```

3. 터미널과 Claude Code를 재시작. 새 세션에서 `/mcp`로 brain-vault 연결 확인.

이후 팀·프로젝트 관련 질문을 하면 Claude가 알아서 볼트를 검색해 맥락에 활용한다.

## 운영자: 키 발급·회수

프로덕션 서버의 brain-chat 폴더에서:

```bash
node scripts/mcp-key.mjs issue CTO    # 발급 (출력된 키를 본인에게만 전달)
node scripts/mcp-key.mjs list         # 현황
node scripts/mcp-key.mjs revoke CTO   # 회수
```

서버 재시작은 필요 없다.

## 구성

- `.mcp.json`: brain-chat 서버의 MCP 엔드포인트 연결 (키는 `BRAIN_MCP_TOKEN` 환경변수에서 읽음)
- `skills/brain-vault/`: 볼트 구조와 도구 사용법을 Claude에게 알려주는 스킬
- 서버 쪽 구현: brain-chat 저장소 `app/api/mcp/route.ts`
