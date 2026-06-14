# Claude Code + OMC 개발 환경 설정

이 레포를 작업할 때 사용하는 Claude Code 환경 설정 기록.

---

## oh-my-claudecode (OMC) 설정

### 이 PC에서 완료된 설정

| 항목 | 값 |
|------|-----|
| OMC 버전 | 4.14.7 |
| 설치 위치 | Global (`~/.claude/CLAUDE.md`) |
| 기본 실행 모드 | ultrawork |
| MCP 서버 | Context7 |
| 에이전트 팀 | 활성화 (3명, Auto 디스플레이) |
| OMC CLI | 설치됨 (`omc 4.14.7`) |

---

## 다른 PC에서 동일하게 설정하기

### 1단계: 사전 요구사항 확인

```bash
node --version   # 18+ 필요
npm --version
gh --version
jq --version
ruby --version   # ralph 워크플로우용 (ralph는 Rust 아닌 Ruby 기반)
```

### 2단계: OMC 플러그인 설치

Claude Code 내부에서 실행:

```
/plugin marketplace add https://github.com/Yeachan-Heo/oh-my-claudecode
/plugin install oh-my-claudecode
/reload-plugins
```

### 3단계: OMC 셋업 위자드 실행

```
/oh-my-claudecode:omc-setup
```

셋업 중 선택 기준:

| 질문 | 선택 |
|------|------|
| 설치 위치 | Global |
| 기본 실행 모드 | ultrawork |
| OMC CLI 설치 | Yes |
| 에이전트 팀 활성화 | Yes |
| 팀 표시 방식 | Auto |
| 기본 에이전트 수 | 3명 |
| MCP 서버 | Context7 |

### 4단계: 권한 설정

`~/.claude/settings.json`의 `permissions.allow` 블록을 수동으로 추가하거나, 현재 PC에서 복사:

```bash
# 현재 PC의 permissions 블록 출력
jq '.permissions' ~/.claude/settings.json
```

아래 내용을 `~/.claude/settings.json`에 병합:

```json
{
  "permissions": {
    "allow": [
      "Bash(git add:*)",
      "Bash(git commit:*)",
      "Bash(git push:*)",
      "Bash(git status:*)",
      "Bash(git diff:*)",
      "Bash(git log:*)",
      "Bash(git checkout:*)",
      "Bash(git branch:*)",
      "Bash(git merge:*)",
      "Bash(git pull:*)",
      "Bash(gh:*)",
      "Bash(npm:*)",
      "Bash(npx:*)",
      "Bash(pnpm:*)",
      "Bash(python:*)",
      "Bash(python3:*)",
      "Bash(uv:*)",
      "Bash(uvx:*)",
      "Bash(cat:*)",
      "Bash(curl:*)",
      "Bash(wget:*)",
      "Bash(docker:*)",
      "Bash(docker-compose:*)",
      "Bash(sh:*)",
      "Bash(bash:*)",
      "Bash(make:*)",
      "Bash(yq:*)",
      "Write(*)"
    ]
  }
}
```

### 5단계: Context7 MCP 등록

MCP 서버는 프로젝트별 `.claude.json`에 저장되므로 각 프로젝트에서 실행:

```bash
claude mcp add context7 -- npx -y @upstash/context7-mcp
```

### 6단계: Claude Code 재시작

재시작 후 상태바(HUD)에 OMC 정보가 표시됩니다.

---

## HUD 상태바 항목 설명

Claude Code 하단 상태바에 표시되는 항목들:

| 항목 | 의미 |
|------|------|
| `5h: N%` | **5시간 롤링 rate limit** 소진율. 5시간 단위 API 사용량 창 중 몇 % 사용했는지. |
| `wk: N%` | **주간(weekly) rate limit** 소진율 |
| `sn: N%` | **Sonnet 모델** 별도 사용량 제한 소진율 |
| `op: N%` | **Opus 모델** 별도 사용량 제한 소진율 |
| `(Xh Ym)` | 해당 제한 리셋까지 남은 시간 (예: `5h:45%(3h42m)`) |
| `ctx: N%` | 현재 대화 **컨텍스트 창** 사용률 |
| `Sonnet` / `Opus` / `Haiku` | 현재 사용 중인 **모델** |
| `ralph: N/M` | ralph 루프 현재 반복/최대 횟수 |
| `bg: N/M` | 백그라운드 태스크 완료/전체 |
| `T:N A:N S:N` | Tool call / Agent 호출 / Skill 사용 횟수 |

> `5h: 5%`는 "5시간이 지났다"는 뜻이 아니라 "5시간 한도의 5%를 사용했다"는 의미.

---

## 주요 키워드 / 명령어

| 키워드 | 효과 |
|--------|------|
| `ralph` | 완료까지 반복 실행하는 지속 모드 |
| `ulw` | ultrawork — 최대 병렬 실행 |
| `ralplan` | 반복 기획 인터뷰 |
| `/oh-my-claudecode:team 3:executor "..."` | 3개 에이전트 팀 실행 |
| `/oh-my-claudecode:omc-setup` | 셋업 재실행 또는 업데이트 |
| `/oh-my-claudecode:omc-doctor` | 설치 진단 |
