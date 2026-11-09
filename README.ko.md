# AI Effect Video MCP

[English](README.md)

**AI Effect Video**는 Windows용 영상 편집기입니다. 원본 영상 위에 다른 영상(이펙트 영상)의 일부를 덮어 붙이고, 글자·모자이크·그림을 넣어 새 영상으로 내보냅니다.

이 저장소는 **Claude, ChatGPT(Codex) 같은 AI가 이 편집기를 대신 조작하게 하는 방법**을 안내합니다. 연결해 두면 AI에게 말로 부탁하는 것만으로 편집이 진행되고, 바뀐 내용은 그 자리에서 편집기 화면에 보입니다. 마음에 들지 않으면 Ctrl+Z로 되돌리거나 직접 이어서 고치면 됩니다.

> MCP(Model Context Protocol)는 AI가 내 PC의 프로그램을 쓸 수 있게 해 주는 표준 연결 방식입니다. 몰라도 아래 순서대로 따라 하면 됩니다.

## 이런 부탁을 할 수 있습니다

- "a.mp4를 원본으로, b.mp4를 이펙트로 열고 이펙트를 12프레임 늦춰 줘"
- "3초부터 5초까지 오른쪽 위에 '안녕' 글자를 넣어 줘"
- "두 번째 영상 앞부분에 1초 페이드 인을 넣고 잘 됐는지 확인해 줘"
- "D:\out\final.mp4로 내보내 줘"

더 많은 예시는 [부탁하는 말 예시](examples/conversations.ko.md)에 있습니다.

## 준비물

- Windows 11 (64비트) PC
- AI 프로그램 하나: **Claude Desktop**(가장 쉬움), Claude Code, ChatGPT 데스크톱 앱의 **Work** 모드, Codex, VS Code 중 하나
  - ChatGPT 웹사이트와 ChatGPT 데스크톱 앱의 **Chat** 모드에서는 쓸 수 없습니다.

## 1단계: 편집기 받기

1. [Releases 페이지](https://github.com/NexCreDesign/AIEffectVideo_release/releases/latest)에서 `EffectVideo-1.0.0.zip`을 내려받습니다.
2. `C:\Tools`(또는 계속 둘 다른 폴더)에 압축을 풉니다. zip 안에 이미 `EffectVideo-1.0.0` 폴더가 들어 있어서, 이 안내에서 쓰는 `C:\Tools\EffectVideo-1.0.0` 폴더가 만들어집니다. 설치 과정은 없습니다.
   - Windows의 **압축 풀기** 창에서 대상 폴더에 `C:\Tools`만 입력하세요. 대상 폴더가 `EffectVideo-1.0.0`으로 끝나면 같은 이름의 폴더가 한 겹 더 생깁니다.
3. 폴더 안의 `effect_video.exe`를 두 번 클릭해 한 번 실행해 봅니다.
   - "Windows의 PC 보호"라는 파란 창이 나오면 **추가 정보**를 누른 뒤 **실행**을 누르세요. 아직 코드 서명을 받지 않은 프로그램이라 나오는 안내입니다.

무료판은 모든 기능을 쓸 수 있고, 내보낸 영상 구석에 "AI Effect Video 무료판 / Free"라는 작은 표시가 들어갑니다.

> 같은 페이지의 `EffectVideo-1.0.0-ffmpeg-source.zip`은 편집기에 들어 있는 FFmpeg 부품의 소스 코드입니다(라이선스 때문에 함께 제공). 쓰는 데는 필요 없습니다.

## 2단계: AI 프로그램에 연결하기 (Claude Desktop)

* Claude Desktop에 PC의 파일을 고칠 수 있는 도구가 있다면, https://github.com/NexCreDesign/ai-effect-video-mcp 주소를 알려 주고 이 설정을 부탁할 수 있습니다. 그런 도구가 없거나 설정이 잘 안 되면 아래 순서대로 하세요.

1. Claude Desktop을 열고 **설정(Settings) › 개발자(Developer) › 구성 편집(Edit Config)** 을 누릅니다. `claude_desktop_config.json` 파일이 있는 폴더가 열립니다.
2. 그 파일을 메모장으로 열고 아래 내용을 넣은 뒤 저장합니다.

   ```json
   {
     "mcpServers": {
       "ai-effect-video": {
         "command": "C:\\Tools\\EffectVideo-1.0.0\\effectvideo-mcp.exe"
       }
     }
   }
   ```

   - 경로는 `effectvideo-mcp.exe`가 실제로 있는 곳으로 바꾸세요. 이 파일 안에서는 `\`를 `\\`처럼 **두 번** 써야 합니다.
   - 파일에 이미 `"mcpServers"`가 있다면, 그 중괄호 `{ }` 안에 `"ai-effect-video": { ... }` 부분만 쉼표로 이어 붙입니다.
3. Claude Desktop을 **완전히 종료했다가** 다시 실행합니다. (창을 닫아도 트레이에 남아 있으면 트레이 아이콘에서 종료하세요.)

다른 AI 프로그램은 [다른 AI 프로그램에 연결하기](examples/other-clients.ko.md)를 보세요.

## 3단계: 잘 연결됐는지 확인하기

AI에게 이렇게 말해 보세요.

> AI Effect Video 상태를 보여 줘

편집기가 꺼져 있었다면 자동으로 켜지고, AI가 버전과 지금 열린 작업을 알려 줍니다. 편집기 아래쪽 상태 표시줄에는 **AI 연결됨**이 보입니다.

안 되면 [문제 해결](docs/troubleshooting.ko.md)을 보세요.

## 알아 두면 좋은 점

- **안전:** AI는 저장하지 않은 작업을 버리거나 이미 있는 파일을 덮어쓰기 전에 먼저 물어보도록 되어 있습니다.
- **되돌리기:** AI의 부탁 한 번은 Ctrl+Z 한 번으로 되돌릴 수 있습니다. AI에게 "방금 것 취소해 줘"라고 해도 됩니다.
- **내 PC 안에서만:** 이 PC 안의 프로그램만 편집기에 연결할 수 있습니다. 인터넷을 통해 외부에서 들어올 수 없습니다.
- **끄고 싶을 때:** 편집기의 **설정 › AI 연결 허용 (MCP)** 를 끄면 AI가 연결하지 못합니다. 기본은 켜져 있습니다.
- **AI가 못 하는 것:** AI는 영상을 재생해서 보거나 소리를 들을 수 없습니다. 대신 장면을 사진처럼 여러 장 보고, 소리는 크기를 숫자로 잽니다. 자세한 내용은 [AI가 결과를 확인하는 방법](docs/checking-video-and-audio.ko.md)에 있습니다.

## 문서 안내

| 문서 | 이럴 때 보세요 |
|---|---|
| [AI에게 부탁하는 말 예시](examples/conversations.ko.md) | 무엇을 부탁할 수 있는지 보고 싶을 때 |
| [AI가 결과를 확인하는 방법](docs/checking-video-and-audio.ko.md) | AI에게 결과 점검을 맡기고 싶을 때 |
| [문제 해결](docs/troubleshooting.ko.md) | 연결이 안 되거나 오류가 날 때 |
| [다른 AI 프로그램에 연결하기](examples/other-clients.ko.md) | Claude Desktop이 아닌 프로그램을 쓸 때 |
| [도구 설명서](docs/tools.md), [AI용 규칙](docs/guide-for-ai.md) | 개발자·AI용 상세 자료 (영어) |
| [변경 내역](CHANGELOG.md) | 버전별로 달라진 점 (영어) |

## 문의와 버그 신고

[GitHub Issues](https://github.com/NexCreDesign/ai-effect-video-mcp/issues)에 글을 남겨 주세요. 다음을 함께 적어 주시면 빨리 도와드릴 수 있습니다.

- 편집기 버전: 편집기의 **도움말 › AI Effect Video 정보…**
- AI에게 부탁한 말과 AI가 보여 준 오류 문구
- 로그 파일: 편집기의 **도움말 › 로그 저장…** 으로 저장한 파일

## 라이선스

이 저장소의 문서와 예시는 [MIT 라이선스](LICENSE)로 자유롭게 쓸 수 있습니다. AI Effect Video 편집기 무료판은 무료로 쓸 수 있지만 MIT 라이선스가 아니며, 소스 코드는 공개하지 않습니다. 사용 조건은 [편집기 사용권 계약](https://github.com/NexCreDesign/AIEffectVideo_release/blob/main/LICENSE.md)을 따릅니다.
