# 다른 AI 프로그램에 연결하기

[English](other-clients.md)

Claude Desktop 연결 방법은 [처음 안내](../README.ko.md#2단계-ai-프로그램에-연결하기-claude-desktop)에 있습니다. 여기서는 그 밖의 프로그램을 설명합니다.

어느 프로그램이든 알려 줄 것은 하나입니다. **편집기 폴더 안에 있는 `effectvideo-mcp.exe`의 위치**입니다. 아래 예시는 편집기가 `C:\Tools\EffectVideo-1.0.0`에 있다고 가정합니다(`C:\Tools`에 압축을 풀면 이렇게 됩니다). 실제 폴더로 바꿔 쓰세요.

## ChatGPT 데스크톱 앱 (Work 모드)과 Codex

ChatGPT 데스크톱 앱의 **Work** 모드와 Codex(명령줄 프로그램)는 같은 설정 파일을 씁니다.

1. 파일 탐색기 주소창에 `%USERPROFILE%\.codex`를 입력해 폴더를 엽니다.
2. `config.toml` 파일을 메모장으로 열고, 맨 아래에 다음을 붙여 넣은 뒤 저장합니다.

   ```toml
   [mcp_servers.ai-effect-video]
   command = 'C:\Tools\EffectVideo-1.0.0\effectvideo-mcp.exe'
   startup_timeout_sec = 30
   tool_timeout_sec = 120
   ```

   - 이 파일에서는 경로를 작은따옴표 `' '`로 감싸고, `\`를 한 번만 씁니다.
   - 아래 두 줄(30, 120)은 기다리는 시간을 늘리는 설정입니다. 영상 구간을 파일로 만드는 데 시간이 걸려서, 빼면 중간에 시간 초과가 날 수 있습니다.
3. ChatGPT 앱(또는 Codex)을 완전히 종료했다가 다시 실행합니다.
4. ChatGPT 앱이라면 화면 위쪽에서 **Work**를 고른 뒤 "AI Effect Video 상태를 보여 줘"라고 말해 봅니다.

알아 둘 점:

- ChatGPT 앱의 **Chat** 모드(일반 대화)와 ChatGPT 웹사이트에서는 쓸 수 없습니다.
- 편집 내용을 바꾸거나 파일을 저장하는 요청에서는 Codex가 "허용할까요?"라고 확인을 받을 수 있습니다. 상태 조회나 화면 보기 같은 읽기 요청은 확인 없이 진행됩니다.

## Claude Code

명령 프롬프트나 터미널에서 한 줄을 실행합니다.

```
claude mcp add ai-effect-video -- "C:\Tools\EffectVideo-1.0.0\effectvideo-mcp.exe"
```

## VS Code

작업 중인 폴더 안에 `.vscode` 폴더를 만들고, 그 안에 `mcp.json` 파일을 다음 내용으로 만듭니다.

```json
{
  "servers": {
    "ai-effect-video": {
      "type": "stdio",
      "command": "C:\\Tools\\EffectVideo-1.0.0\\effectvideo-mcp.exe"
    }
  }
}
```

JSON 파일에서는 `\`를 `\\`로 두 번 씁니다.

## 그 밖의 프로그램

"MCP 서버 추가" 같은 메뉴가 있는 프로그램이라면 대부분 쓸 수 있습니다. 다음처럼 입력하세요.

| 항목 | 넣을 값 |
|---|---|
| 명령(Command) | `effectvideo-mcp.exe`의 전체 경로 |
| 인자(Arguments) | 비워 둠 |
| 방식(Transport) | stdio (로컬) |

## 고급: 환경 변수

보통은 필요 없습니다. 다른 프로그램과 충돌할 때만 씁니다.

| 변수 | 쓰임 |
|---|---|
| `EFFECTVIDEO_MCP_PORT` | 편집기와 연결에 쓰는 포트 번호(기본 9877). 다른 프로그램이 이미 9877을 쓰고 있을 때 바꿉니다. 편집기와 AI 프로그램 설정 양쪽에 같은 값을 줘야 합니다. |
| `EFFECTVIDEO_APP` | 편집기가 꺼져 있을 때 자동으로 켤 `effect_video.exe`의 위치. 기본은 `effectvideo-mcp.exe` 옆에 있는 파일입니다. |
